

## 类库计算cosine
1. 在Python中使用scipy计算余弦相似性
scipy 模块中的spatial.distance.cosine() 函数可以用来计算余弦相似性，但是必须要用1减去函数值得到的才是余弦相似度。
```python
from scipy import spatial
vec1 = [1, 2, 3, 4]
vec2 = [5, 6, 7, 8]
cos_sim = 1 - spatial.distance.cosine(vec1, vec2)
print(cos_sim)
```
2. 在Python中使用numpy计算余弦相似性
numpy模块没有直接提供计算余弦相似性的函数，我们可以根据余弦相似性的计算公式来计算。其中numpy.doy()函数可以计算两个向量的内积，numpy.linalg.norm()函数返回向量的模。
```python
import numpy as np
vec1 = np.array([1, 2, 3, 4])
vec2 = np.array([5, 6, 7, 8])
cos_sim = vec1.dot(vec2) / np.linalg.norm(vec1) * np.linalg.norm(vec2)
print(cos_sim)
```
注意numpy只能计算numpy.ndarray类型向量的余弦相似性。

3. 在Python中使用sklearn计算余弦相似性
sklearn提供内置函数cosine_similarity()可以直接用来计算余弦相似性。
```python
import numpy as np
from sklearn.metric.pairwise import cosine_similarity()
vec1 = np.array([1, 2, 3, 4])
vec2 = np.array([5, 6, 7, 8])
cos_sim = cosine_similarity(vec1.reshape(1, -1), vec2.reshape(1, -1))
print(cos_sim[0][0])
```
## ecoc
```
ecoc_ctr =sum(clicked)/sum(p_ctr)
```
