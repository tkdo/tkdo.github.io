# 并查集
## 描述
- 查找：确定某个元素属于哪个子集。
- 合并：将两个字集合并成一个子集。
## 模版
- n 表示节点的个数。
- p 表示存储每个点的父节点，初始时每个点的父节点都是自己。
- size 只有当前节点是祖宗节点才有意义，表示祖宗节点中点的数量。
- find(x) 函数用于查找x所在集合的祖宗节点。
- union(a, b) 函数用于合并a和b所在集合。

```python
def find(x):
    if p[x] ! = x:
        p[x] = find(p[x])
    return p[x]

def union(a, b):
    pa, pb = find(a), find(b)
    if pa !=  pb:
        return  
    p[pa] = pb
    size[pb] = size[pb] + size[pa]
```
## [leetcode-826-最大人工岛](https://leetcode.cn/problems/making-a-large-island/)
```python
class Solution:
def largestIsland(self, grid: List[List[int]]) -> int:
    def find(x):
        if p[x] != x:
            p[x] = find(p[x]) # 向上一直找父亲节点
        return p[x]

    def union(a, b):
        pa, pb = find(a), find(b)
        if pa == pb:
            return 
        p[pa] = pb # pa的父亲等于pb 也就是把pa挂到了pb上
        size[pb] = size[pb] + size[pa] # 更新pb的大小
    
    n = len(grid)
    p = list(range(n * n))
    size = [1] * (n * n)
    
    for i, row in enumerate(grid):
        for j, v in enumerate(row):
            if v == 1:
                for a, b in [[0, -1],[-1, 0]]: #因为grid需要遍历一遍，所以可以从当前的点往上往左走，每个都能算一遍
                    x, y = i + a, j + b
                    if 0 <= x <= len(grid) and 0 <= y <=len(grid[0]) and grid[x][y] ==1:
                        union(x * n + y, i * n + j)
    ans = max(size)
    for i, row in enumerate(grid):
        for j, v in enumerate(row):
            if v == 0:# 找到0的位置
                vis = set()
                t = 1 
                for a, b in [[0, -1], [0, 1], [1, 0], [-1, 0]]:
                        x, y = i + a, j + b
                        if 0 <= x < n and 0 <= y < n and grid[x][y] == 1: #找到周边的1
                            root = find(x * n + y)
                            if root not in vis:
                                vis.add(root)
                                t += size[root]
                ans = max(ans, t)
    return ans 
```










