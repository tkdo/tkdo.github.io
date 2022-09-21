
# 预测用户的兴趣向量
- hive pyton udf  
    ```python
    import sys,os
    from collections import defaultdict
    import tensorflow as tf

    shell_dir = "mind_predict.tar.gz/"
    if os.path.exists(shell_dir):  # 切换到当前目录
        os.chdir(shell_dir)
        sys.path.insert(0, "./")
    os.environ["TF_CPP_MIN_LOG_LEVEL"] = "3"

    flags = tf.flags
    FLAGS = flags.FLAGS
    flags.DEFINE_integer("batch_size", 256, "batch_size")
    flags.DEFINE_integer("seq_len", 256, "seq_len")

    def parse_row(row):
        flag, uuid, seq = True, str(), []
        row = row.replace("\n", "")
        if "\t" in row:
            cells = row.split("\t")
        else:
            cells = row.split("\1")
        if cells is None or len(cells) != 2:
            flag = False
            return flag, uuid, seq
        uuid, seq_str = cells
        if "," in seq_str:
            seq = seq_str.split(",")
        elif "-" in seq_str:
            seq = seq_str.split("-")
        else:
            seq = [seq_str]
        if seq is None or len(seq) <= 0:
            flag = False
            return flag, uuid, seq
        seq = [int(s) for s in seq]
        return flag, uuid, seq


    def seq_padding(seq):
        s_len = len(seq)
        mask = [1.0 for _ in range(s_len)]
        if s_len >=FLAGS.seq_len:
           return  mask[-FLAGS.seq_len:], seq[-FLAGS.seq_len:]
        else:
           seq += [0] * (FLAGS.seq_len - s_len)
           mask += [0] * (FLAGS.seq_len - s_len)
           return mask, seq

    def batch_predict(uuid_all, batch_instance):
        feed_dict = {"feature_ph": batch_instance["feature_ph"],
                     "mask_ph": batch_instance["mask_ph"]}
        user_embs = predict_fn(feed_dict)["user_emb"]
        if user_embs is not None and len(user_embs) > 0:
            num_batch = user_embs.shape[0]
            assert len(uuid_all) == num_batch
            for i, ele in enumerate(uuid_all):
                u_emb = user_embs[i]
                u_emb = u_emb.reshape(-1)
                tmp_emb = [str(w) for w in u_emb]
                print(f"{uuid_all[i]}\t{','.join(tmp_emb)}")

    def predict_std():
        batch_uuid = []
        batch_instance = defaultdict(list)
        cur = 0
        for line in sys.stdin:
            try:
                flag, uuid, seq = parse_row(line)
                if flag is False:
                    continue
                mask, seq = seq_padding(seq)
                batch_uuid.append(uuid)  # uuid
                batch_instance["feature_ph"].append(seq)  # seq
                batch_instance["mask_ph"].append(mask)  # seq_len
                cur += 1
                if cur > 0 and cur % FLAGS.batch_size == 0:
                    batch_predict(batch_uuid, batch_instance)
                    batch_uuid.clear()
                    batch_instance.clear()
            except Exception as e:
                sys.stderr.write(f"{line}\n")
                sys.stderr.write(f"{e.message}\n")
        if len(batch_uuid) > 0:
            batch_predict(batch_uuid, batch_instance)
            
    if __name__ == "__main__":
        predict_fn = tf.contrib.predictor.from_saved_model(export_dir="./saved_model", signature_def_key="serving_default")
        predict_std()
    ```
-   hive sql
    ```sql
    set hive.mapred.mode=nostrict;
    set hive.exec.script.allow.partial.consumption=true;
    set hive.exec.dynamici.partition=true;
    set hive.exec.dynamic.partition.mode=nonstrict;
    set yarn.nodemanager.vmem-check-enabled=false;
    set yarn.nodemanager.vmem-pmem-ratio=8;
    set io.sort.mb=10;
    set mapreduce.task.io.sort.factor=10;
    set mapred.max.split.size=62000000;
    set mapred.min.split.size.per.node=62000000;
    set mapred.min.split.size.per.rack=62000000;
    set mapreduce.map.cpu.vcores=2;
    set mapreduce.reduce.cpu.vcores=2;
    set mapreduce.map.memory.mb=7168;
    set mapreduce.task.timeout=4800000;
    add ARCHIVE env_py36_tf15.tar.gz;
    add ARCHIVE model_predict.tar.gz;

    insert overwrite table predict_tab partition(day, hid)
    select uid, embedding, 'active' as day, substr(md5(uid), 0, 1) as hid
    from (select transform(*)  USING 'env_py36_tf15.tar.gz/env_py36_tf15/bin/python -u model_predict.tar.gz/predict.py --batch_size=256' as (uid,embedding)
    from (
        select uid, seq from feed_feature
        where uid is not null and length(uid)>1 and seq is not null and length(seq)>5
        ) map_output
    ) as tab;
    ```

# 从百万物料中检索和用户喜欢的物料
- hive python udf
    ```python
    #!/usr/bin/env python
    # -*- coding: UTF-8 -*-
    import sys
    import os
    import faiss
    import argparse
    import numpy as np
    from faiss import normalize_L2

    parser = argparse.ArgumentParser()
    parser.add_argument("--batch_size", type=int, default=64)
    parser.add_argument("--emb_dim", type=int, default=16)
    parser.add_argument("--num_interest", type=int, default=8)
    parser.add_argument("--top_neighbor", type=int, default=100)
    parser.add_argument("--item_emb", type=str, default="item_emb.csv")
    parser.add_argument("--normalize", type=int, default=1)  # normalize item
    parser.add_argument("--precision", type=int, default=6)  # decimal precision

    def load_item_emb(file):
        idx_dict, emb_list = dict(), list()
        item_file = open(file, "r")
        rows = item_file.readlines()
        item_file.close()
        new_rows = []
        for row in rows:
            cells = row.strip().split("\t")
            if cells is not None and len(cells) == 2:
                idx, emb = cells
                emb = [float(i) for i in emb.split(",")]
                new_rows.append([idx, emb])
        for i, (k, v) in enumerate(new_rows):
            idx_dict[i] = k
            emb_list.append(v)
        return idx_dict, emb_list

    def build_index(dim, emb):
        emb = np.array(emb)
        emb = emb.reshape(-1, args.emb_dim).astype(np.float32)
        if args.normalize > 0:
            normalize_L2(emb)
        f_index = faiss.IndexFlatIP(dim)
        f_index.add(emb)
        return f_index

    def batch_recall(batch_uuid, D, I):
        cur_size = len(batch_uuid)
        # I:(batch_size * num_interest, topN)
        for k in range(cur_size):
            item_list_set = set()
            item_res = list()
            # item_list:(num_interest * topN)
            item_list = list(zip(np.reshape(I[k * args.num_interest:(k + 1) * args.num_interest], -1),
                                 np.reshape(D[k * args.num_interest:(k + 1) * args.num_interest], -1)))
            item_list.sort(key=lambda x: x[1], reverse=True)
            for j in range(len(item_list)):
                # index:0 reserved.
                item_key = idx_kv[item_list[j][0]]
                item_score = round(item_list[j][1].item(), args.precision)
                if item_key not in item_list_set:
                    item_list_set.add(item_key)
                    item_res.append(f"{item_key}:{item_score}")
                    if len(item_list_set) >= args.topN:
                        break
            print(f"{batch_uuid[k]}\t{','.join([str(j) for j in list(item_res)])}")

    def recall_std():
        batch_uuid = []
        batch_emb = []
        cur = 0
        for row in sys.stdin:
            cells = row.strip().split("\t")
            if cells is not None and len(cells) == 2:
                uuid, emb = cells
                emb = [float(i) for i in emb.split(",")]
            else:
                continue
            batch_uuid.append(uuid)
            batch_emb.append(emb)
            cur += 1
            if cur > 0 and cur % args.batch_size == 0:
                batch_emb = np.array(batch_emb).astype(np.float32)
                batch_emb = batch_emb.reshape([-1, args.emb_dim])
                D, I = index.search(batch_emb, args.topN)
                batch_recall(batch_uuid, D, I)
                batch_uuid, batch_emb = [], []
        if cur > 0 and cur % args.batch_size > 0:
            batch_emb = np.array(batch_emb).astype(np.float32)
            batch_emb = batch_emb.reshape([-1, args.emb_dim])
            D, I = index.search(batch_emb, args.topN)
            batch_recall(batch_uuid, D, I)


    if __name__ == "__main__":
        args = parser.parse_args()
        shell_dir = './'
        if os.path.exists(shell_dir):  # 切换到当前目录
            os.chdir(shell_dir)
            sys.path.insert(0, './')
        idx_kv, item_emb = load_item_emb(args.item_emb)
        index = build_index(dim=args.emb_dim, emb=item_emb)
        recall_std()
    ```

- hive sql
    ```sql
    set hive.exec.script.allow.partial.consumption=true;
    set hive.exec.dynamici.partition=true;
    set hive.exec.dynamic.partition.mode=nonstrict;
    set yarn.nodemanager.vmem-check-enabled=false;
    set yarn.nodemanager.vmem-pmem-ratio=8;
    set io.sort.mb=10;
    set mapreduce.task.io.sort.factor=10;
    set mapred.max.split.size=15000000;
    set mapred.min.split.size.per.node=15000000;
    set mapred.min.split.size.per.rack=15000000;
    set mapred.task.timeout=18000000;
    set mapreduce.map.memory.mb=4096;
    set mapreduce.map.cpu.vcores=2;
    add ARCHIVE env_py36_tf15.tar.gz;
    add ARCHIVE recall.py;
    add ARCHIVE item_embedding.csv;
    insert overwrite table recall_tab partition(day, hid)
    select uid, rec_res, 'active' as day, substr(md5(uid), 0, 1) as hid
    ```
