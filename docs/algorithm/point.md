
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





[leetcode792-匹配子序列的单词数](https://leetcode.cn/problems/number-of-matching-subsequences/)
    给定字符串 s和字符串数组words, 返回words[i]中是s的子序列的单词个数。
    字符串的 子序列 是从原始字符串中生成的新字符串，可以从中删去一些字符(可以是none)，而不改变其余字符的相对顺序。
    例如， “ace” 是 “abcde” 的子序列。
    示例 1:
    输入: s = "abcde", words = ["a","bb","acd","ace"]
    输出: 3
    解释: 有三个是s 的子序列的单词: "a", "acd", "ace"。
    Example 2:
    输入: s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]
    输出: 2
    提示:
    1 <= s.length <= 5 * 104
    1 <= words.length <= 5000
    1 <= words[i].length <= 50
    words[i]和 s都只由小写字母组成。

=== "python"

```python
class Solution:
    def numMatchingSubseq(self, s: str, words: List[str]) -> int:
        d = defaultdict(list)
        res = 0
        for w in words:
            d[w[0]].append(w)
        for c in s:
            for _ in range(len(d[c])): ###>>>>这两需要配合保证3条，只弹出3次
                w = d[c].pop(0)        ###>>>>while循环会一直循环里面的东西为空
                                    ###例如：bb，弹出1次，之后紧接着又进入，在次弹出，这个时候c是没有变化的
                if len(w) == 1:
                    res = res + 1
                else:
                    d[w[1]].append(w[1:])
        return res
```
