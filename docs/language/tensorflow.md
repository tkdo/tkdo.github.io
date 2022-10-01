
# 常用函数
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
