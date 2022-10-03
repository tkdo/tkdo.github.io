
### 三个指针
## [leetcode-17.09 第k个数](https://leetcode.cn/problems/get-kth-magic-number-lcci/)


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


