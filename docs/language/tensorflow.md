

## concat报错定位
- 问题
    ```python
    (0) Invalid argument: ConcatOp : Dimensions of inputs should match: shape[0] = [10240,4] vs. shape[37] = [10236,106]
    (1) Invalid argument: ConcatOp : Dimensions of inputs should match: shape[0] = [10240,4] vs. shape[37] = [10236,106]
    ```
- 方案：
    concat时候报错，某一个特征为Bx106，找到这个特征去掉

## 打印签名
```bash
saved_model_cli show --dir export/1524906774 --tag_set serve --signature_def serving_default
```


## tensorflow读取hdfs错误
```bash
loadFileSystems error:
(unable to get stack trace for java.lang.NoClassDefFoundError exception: ExceptionUtils::getStackTrace error.)
hdfsBuilderConnect(forceNewInstance=0, nn=default, port=0, kerbTicketCachePath=(NULL), userName=(NULL)) error:
(unable to get stack trace for java.lang.NoClassDefFoundError exception: ExceptionUtils::getStackTrace error.)
```
```
export CLASSPATH=${HADOOP_HOME}/etc/hadoop:`find ${HADOOP_HOME}/share/hadoop/ | awk '{path=path":"$0}END{print path}'`
export LD_LIBRARY_PATH="${HADOOP_HOME}/lib/native":$LD_LIBRARY_PATH
```


## 禁用tf使用gpu
```bash
#设置环境变量
export CUDA_VISIBLE_DEVICES=-1
```
```python
import os
os.environ["CUDA_VISIBLE_DEVICES"]="-1"    
import tensorflow as tf
```

## 常用函数
- tf.matmul
    - shape的变幻，例如：(1, 1, 64) (1, 64, 1) = (1, 1, 1)
    ```python
    >>> item_emb = [[[0.1,0.2,0.3,0.4]]]
    >>> user_emb = [[[0.1],[0.2],[0.3],[0.4]]]
    >>> inner_dot = tf.matmul(tf.constant(item_emb), tf.constant(user_emb))
    >>> user_emb
    [[[0.1], [0.2], [0.3], [0.4]]]
    >>> item_emb
    [[[0.1, 0.2, 0.3, 0.4]]]
    >>> inner_dot
    <tf.Tensor: id=47, shape=(1, 1, 1), dtype=float32, numpy=array([[[0.3]]], dtype=float32)>
    ```
- tf.tile
    - 通过变幻可以让对应的维度复制
    - multiples = [1, 4, 1] 其中1表示第1个维度复制1倍，4表示第2个维度复制4倍，1表示第3个维度复制1倍
    ```python
    >>> x = tf.tile(inner_dot, multiples=[1, 4, 1])
    >>> x
    <tf.Tensor: id=52, shape=(1, 4, 1), dtype=float32, numpy=
    array([[[0.3],
            [0.3],
            [0.3],
            [0.3]]], dtype=float32)>
    ```
- tf.enable_eager_execution
- tf.reshap
    - 变幻shape
    ```python
    >>> x = tf.constant([
        [[1,1],[2,2],[3,3],[4,4],[5,5]], 
        [[1,1],[2,2],[3,3],[4,4],[5,5]], 
        [[1,1],[2,2],[3,3],[4,4],[5,5]]])
    >>> x 
    <tf.Tensor: id=0, shape=(3, 5, 2), dtype=int32>
    >>> tf.reshape(x, shape=[-1, 10])
    <tf.Tensor: id=3, shape=(3, 10), dtype=int32, numpy=
    array([[1, 1, 2, 2, 3, 3, 4, 4, 5, 5],
           [1, 1, 2, 2, 3, 3, 4, 4, 5, 5],
           [1, 1, 2, 2, 3, 3, 4, 4, 5, 5]], dtype=int32)>

    x = tf.constant([
        [[1,2,3,4], [1,2,3,4]], 
        [[1,2,3,4], [1,2,3,4]]])
    >>> x
    <tf.Tensor: id=7, shape=(2, 2, 4), dtype=int32>
    >>> tf.reshape(x, shape=[-1, 4])
    <tf.Tensor: id=10, shape=(4, 4), dtype=int32, numpy=
    array([[1, 2, 3, 4],
           [1, 2, 3, 4],
           [1, 2, 3, 4],
           [1, 2, 3, 4]], dtype=int32)>
    ```
- tf.gather
    ```python
    >>> import tensorflow as tf            
    >>> tf.enable_eager_execution()
    >>> params = tf.constant([[0, 1.0, 2.0],
    ...                       [10.0, 11.0, 12.0],
    ...                       [20.0, 21.0, 22.0],
    ...                       [30.0, 31.0, 32.0]])
    >>> params
    <tf.Tensor: id=0, shape=(4, 3), dtype=float32, numpy=
    array([[ 0.,  1.,  2.],
        [10., 11., 12.],
        [20., 21., 22.],
        [30., 31., 32.]], dtype=float32)>
    >>> tf.gather(params, indices=[1,2]).numpy()
    array([[10., 11., 12.],
        [20., 21., 22.]], dtype=float32)
    >>> tf.gather(params, indices=[0,1]).numpy()
    array([[ 0.,  1.,  2.],
        [10., 11., 12.]], dtype=float32)
    >>> 
    ```
