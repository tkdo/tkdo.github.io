
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
