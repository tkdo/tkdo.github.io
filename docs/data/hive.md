
##  配置.hiverc
```bash
set hive.mapred.mode=nostrict;
set hive.execution.engine=mr;
set mapred.job.queue.name=root;
set hive.exec.script.allow.partial.consumption=true;
set hive.exec.dynamici.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict;
```

## 如何对一个序列，按照时间戳，进行排序
- 描述：其中ts表示字段为时间戳，item_id表示为物料ID。
    - ① concat_ws 把物料和时间戳粘在一起，其中时间戳在前面。
    - ② collect_list 把组装后的字符串收集到list中。
    - ③ sort_array 把array变成有序。
    - ④ regexp_replace 把时间戳剃掉。
```sql
regexp_replace(concat_ws(',', 
                sort_array(
                collect_list(
                concat_ws(':', ts, 
                cast(item as string))))), 
                '\\\\d+\:', '') seq
```


