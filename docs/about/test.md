---
title: 测试文档
---

# 测试文档

Here is an inline example, $\pi(\theta)$, 

an equation,

$$\nabla f(x) \in \mathbb{R}^n,$$

and a regular \$ symbol.

[](https://github.com/tkdo/tkdo.github.io)

<https://github.com/tkdo/tkdo.github.io>

https://github.com/tkdo/tkdo.github.io

**bold** _iiii_

$\vec{a}$ $\vec a$

$\boldsymbol u$ $\boldsymbol{a}$

- 1
    - 2
        - 3
            - 4
- 1
    - 2
        - 3
            - 4


??? note "tf.reshap"
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
