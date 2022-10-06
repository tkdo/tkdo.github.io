
### 三个指针
## [leetcode-17.09 第k个数](https://leetcode.cn/problems/get-kth-magic-number-lcci/)
    有些数的素因子只有 3，5，7，请设计一个算法找出第 k 个数。注意，不是必须有这些素因子，而是必须不包含其他的素因子。
    例如，前几个数按顺序应该是 1，3，5，7，9，15，21。
    示例 1:
    输入: k = 5
    输出: 9

=== "python"

    ```python
    class Solution:
        def getKthMagicNumber(self, k: int) -> int:
            nums = [0] * (k + 1)
            p3, p5, p7 = 1, 1, 1 # 表示三个指针
            nums[1] = 1 # nums[0]=0,与k+1个才能到k
            for i in range(2, k + 1):
                nums[i] = min(nums[p3]*3, nums[p5]*5, nums[p7]*7)
                if nums[i] == nums[p3]*3:# 三个指针依次交替
                    p3 = p3 + 1
                if nums[i] == nums[p5]*5:
                    p5 = p5 + 1
                if nums[i] == nums[p7]*7:
                    p7 = p7 + 1
            return nums[k]
    ```



##  参考链接
- https://yft.github.io/yft/#/
