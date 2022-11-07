
##  [leetcode-927-三等分](https://leetcode.cn/problems/three-equal-parts/)
    给定一个由 0 和 1 组成的数组arr，将数组分成 3个非空的部分 ，使得所有这些部分表示相同的二进制值。
    如果可以做到，请返回任何[i, j]，其中 i+1 < j，这样一来：
    arr[0], arr[1], ..., arr[i]为第一部分；
    arr[i + 1], arr[i + 2], ..., arr[j - 1]为第二部分；
    arr[j], arr[j + 1], ..., arr[arr.length - 1]为第三部分。
    这三个部分所表示的二进制值相等。
    如果无法做到，就返回[-1, -1]。
    注意，在考虑每个部分所表示的二进制时，应当将其看作一个整体。例如，[1,1,0]表示十进制中的6，而不会是3。此外，前导零也是被允许的，所以[0,1,1] 和[1,1]表示相同的值。

    示例 1：
    输入：arr = [1,0,1,0,1]
    输出：[0,3]
    示例 2：

    输入：arr = [1,1,0,1,1]
    输出：[-1,-1]

    示例 3:
    输入：arr = [1,1,0,0,1]
    输出：[0,2]

    提示：
    3 <= arr.length <= 3 * 104
    arr[i]是0或1

=== "python"

    ```python
    class Solution:
        def threeEqualParts(self, arr: List[int]) -> List[int]:
            def find(x):
                s = 0
                for i, a in enumerate(arr):
                    s = s + a
                    if s == x:
                        return i

            n = len(arr)
            cnt, mod = divmod(sum(arr), 3)
            if mod > 0:
                return [-1, -1] # 如果1的总数不够整除直接返回false
            if cnt == 0:
                return [0, n-1] # 如果cnt为0，说明整个数组全部为0
            # 分成3段，分别找到第一二三段的第一个1，i、j、k分别指向它们
            i, j, k = find(1), find(cnt + 1), find(2*cnt + 1)
            while k < n and arr[i] == arr[j] == arr[k]:
                i = i + 1
                j = j + 1
                k = k + 1
            return [i-1, j] if k == n else [-1, -1]
    ```


① i、j、k分别指向三段中第1个1

    0 1 1 0 0| 0 1 1 0 0 0| 0 0 1 1 0 0
      ^          ^              ^
      i          j              k
② i、j、k跳出循环

    0 1 1 0 0| 0 1 1 0 0 0| 0 0 1 1 0 0
               ^         ^              ^
               i         j              k



##  [leetcode-OfferII-007-数组中和为0的三个数](https://leetcode.cn/problems/1fGaJU/)
    给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k，
    同时还满足 nums[i] + nums[j] + nums[k] == 0 。请你返回所有和为 0 且不重复的三元组。
    注意：答案中不可以包含重复的三元组。
    示例 1：
    输入：nums = [-1,0,1,2,-1,-4]
    输出：[[-1,-1,2],[-1,0,1]]
    解释：
    nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
    nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
    nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
    不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
    注意，输出的顺序和三元组的顺序并不重要。
    示例 2：
    输入：nums = [0,1,1]
    输出：[]
    解释：唯一可能的三元组和不为 0 。
    示例 3：
    输入：nums = [0,0,0]
    输出：[[0,0,0]]
    解释：唯一可能的三元组和为 0 。
=== "python"

    ```python
    class Solution:
        def threeSum(self, nums: List[int]) -> List[List[int]]:
            nums.sort()
            ans = set()
            n = len(nums)
            for i in range(n - 2):
                j = i + 1
                k = n - 1
                x = nums[i]
                while j < k:
                    cur = x + nums[j] + nums[k]
                    if cur > 0:
                        k -= 1
                    elif cur < 0:
                        j += 1
                    else:
                        ans.add((x, nums[j], nums[k])) # i和j同时移动
                        j += 1
                        k -= 1
            return [list(a) for a in ans]
    ```
