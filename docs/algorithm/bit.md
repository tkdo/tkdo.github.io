# 位运算

##  [leetcode-17.19 消失的两个数字](https://leetcode.cn/problems/missing-two-lcci/)

    给定一个数组，包含从 1 到 N 所有的整数，但其中缺了两个数字。
    你能在 O(N) 时间内只用 O(1) 的空间找到它们吗？
    以任意顺序返回这两个数字均可。
    示例 1:
    输入: [1]
    输出: [2,3]
    示例 2:
    输入: [2,3]
    输出: [1,4]
    提示：
    nums.length <=30000


=== "公式计算"

    ```python
    class Solution:
    def missingTwo(self, nums: List[int]) -> List[int]:
        n = len(nums) + 2
        sum2 = n * (1 + n) // 2 - sum(nums) # 前n个数和不可能为小数
        t = sum2 // 2 #缺失的这两个值不可能在sum2//2的同一侧
        cur = t * (1 + t) // 2 -  sum([x for x in nums if x <= t]) 
        return [cur, sum2 - cur]
    ```
