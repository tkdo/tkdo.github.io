# Listwise Loss
## KL散度 loss
- 分别对模型输出结构与label，进行softmax就可以分别得到rank_logits（也就是该item排在当前的位置的概率）与label_logits（也就是真实该item排在的位置的概率）
    ```python
    label_logits = tf.softmax(label)
    rank_logits = tf.softmax(pred)
    ```
- 衡量两个概率分布差异，自然就会想到kl散度
- kl散度具体代码
    ```python
    def kl_loss(label_prob, pred_prob):
    """
    :param label_prob:真实概率分布
    :param pred_prob:预测概率分布
    """
    p_q = label_prob/tf.clip_by_value(pred_prob, 1e-8, 1)
    kl_loss = tf.reduce_sum(label_prob * tf.log(p_q), axis=-1)
    return kl_loss
    ```
- kl散度做loss的局限性
    - kl散度是衡量两个概率分布的差异，真实推荐场景中，只要满足rank_logits的顺序与label_logits相同，他的loss就应该为0，并不需要强调概率接近。
    - 计算kl loss需要提前设计一个真实的排序得分，比如：下单、加购、点击与曝光分别为5、4、3、1分。然后是预测的rank_logits尽量与这个分布相近，然而这个分布是人为定义的，不是真实的分布。
- kl散度做loss的优势
    - 实现简单
    - 设计不同的得分，相当于设计不同的样本权重，当需要提高转化率，则可以把下单的分数设置高一点

## ListMLE loss
$L_{MLE}(P_n) = - log P_M(o|p_n)$

- 解释
    - o=[o1, o2, ..., omn]表示答案中的正确序列，
    - k=1时，表示o1被争取的放在了第1位的概率；
    - k=2是，表示o2在除去o1的剩下的候选中，被选中做第2位的概率；
    - 依次类推，最后概率乘起来；因为概率乘积可能会导致数值过小，所以在第一个公式中套上了log。
          
- 案例 
    - label=[2,5,3,1]
    - logits=[0.7,1.1,2.1,0.5]
    - 先给label排序[3,3,2,1],按照label的排序方式，给logits排序则变成[1.1,2.1,0.7,0.5]
    - 可以理解按照label的那列的由大到小排序
    - 然后套公式计算：
    - o1放首位的概率po1=1.1/(1.1+2.1+0.7+0.5)
    - 去掉o1，o2放首位的概率po2=2.1/(2.1+0.7+0.5),依次类推
    - 最后loss=-log(po1*po2...*pon)
- 代码
    ```python
    # 参考：https://zhuanlan.zhihu.com/p/66514492
    import tensorflow as tf
    def mle_loss(true_scores,rank_scores):
        '''
        :param true_scores: 真实得分（分值不重要，能确定先后顺序就行）,shape (bt,list_size)
        :param rank_scores: 预测得分, shape (bt,list_size)
        :return:
        '''
        # batch_size=4
        list_size=3      #list 不一定为真实的，也可以指定任意值（mle 可以理解为该item排在首位的概率的联乘）
        index = tf.argsort(true_scores, direction='DESCENDING')
        # print(index)[1,2,0] [1,0,2]
        #按真实得分对rank_scores排序
        S_predict=tf.batch_gather(rank_scores, index)
        # S_predict=tf.gather(rank_scores, index,axis=-1,batch_dims=0)
        #分子
        initm_up=tf.constant([[1.0]]) #shape 为 1,1
        for i in range(list_size):#3为 list_size
                    # index=tf.constant([[i]]*batch_size,dtype=tf.int32) #shape 为 bt,1
                    # a=tf.batch_gather(S_predict,index)
                    a=tf.slice(S_predict,[0,i],[-1,1]) #shape 为 bt,1
                    initm_up=initm_up*a #bt,1

        #分母
        initm_down=tf.constant([[1.0]]) #shape 为 1,1
        for i in range(list_size):
                    b=tf.reduce_sum(tf.slice(S_predict, [0, i], [-1, list_size - i]), axis=-1,keep_dims=True)     # bt
                    initm_down*=b
        loss=tf.log(tf.divide(initm_up,initm_down))
        mleloss=-tf.reduce_mean(loss)
        return mleloss
    ```

## ListNet Loss
- 解释
    - ListNet损失，同样先构造答案序列中每个item得分，如[y1,y2,y3]=[0,0.5,1]
    - 第一个公式计算答案中每个item被当做首个item的概率。
    - 第二个公式计算预测结果中每个item当做首个item的概率。
    - 第三个公式是前面两个概率分布的交叉熵。

参考链接：
- https://blog.csdn.net/qq_40859560/article/details/120676204
- https://zhuanlan.zhihu.com/p/66514492