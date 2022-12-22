

##  [OfferII008 和大于等于target的最短子数组](https://leetcode.cn/problems/2VG8Kg/)

    给定一个含有n个正整数的数组和一个正整数 target 。
    找出该数组中满足其和 ≥ target 的长度最小的 连续子数组$[nums_l, nums_{l+1}, ..., nums_{r-1}, nums_r]$ ，并返回其长度。如果不存在符合条件的子数组，返回 0 。
    示例 1：
    输入：target = 7, nums = [2,3,1,2,4,3]
    输出：2
    解释：子数组[4,3]是该条件下的长度最小的子数组。
    示例 2：
    输入：target = 4, nums = [1,4,4]
    输出：1
    示例 3：
    输入：target = 11, nums = [1,1,1,1,1,1,1,1]
    输出：0
    提示：
    1 <= target <= 10^9
    1 <= nums.length <= 10^5
    1 <= nums[i] <= 10^5


=== "python"

    ```python
        class Solution:
            def minSubArrayLen(self, target: int, nums: List[int]) -> int:
                n = len(nums)
                ans = n + 10
                prefixSum = [0] * (n + 10)
                for i in range(1, n+1):
                    #print(i, nums[i-1])
                    prefixSum[i] = prefixSum[i-1] + nums[i-1]
                #print(prefixSum)
                for i  in range(1, n+1):
                    s = prefixSum[i]
                    d = s - target
                    l = 0
                    r = i
                    while l < r:
                        mid = (l + r + 1) >> 1
                        if prefixSum[mid] <= d: 
                            l = mid
                        else:
                            r = mid -1
                    if prefixSum[r] <= d:
                        ans = min(ans, i-r)
                return 0 if ans == n + 10 else ans
    ```


##  [Offer-II-010 和为k的子数组](https://leetcode.cn/problems/QTMn0o/)
    
    给定一个整数数组和一个整数k ，请找到该数组中和为k的连续子数组的个数。
    示例 1：
    输入:nums = [1,1,1], k = 2
    输出: 2
    解释: 此题 [1,1] 与 [1,1] 为两种不同的情况
    示例 2：
    输入:nums = [1,2,3], k = 3
    输出: 2

    提示:
    1 <= nums.length <= 2 * 104
    -1000 <= nums[i] <= 1000
    -107<= k <= 107
=== "c++"

    ```c++
        class Solution {
        public:
            int subarraySum(vector<int>& nums, int k) {
                unordered_map<int, int> mp;
                mp[0] = 1;
                int pre = 0;
                int ans = 0;
                for (auto& x:nums)
                {
                    pre += x;
                    if(mp.find(pre - k) != mp.end())
                    {
                        ans  += mp[pre - k];
                    }
                    mp[pre]++;
                }
                return ans;
            }
        };
    ```
