![Offer-II-009 乘积小于 K 的子数组](https://leetcode.cn/problems/ZVAVXX/)
    给定一个正整数数组nums和整数 k，请找出该数组内乘积小于k的连续的子数组的个数。
    示例 1:
    输入: nums = [10,5,2,6], k = 100
    输出: 8
    解释: 8 个乘积小于 100 的子数组分别为: [10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]。
    需要注意的是 [10,5,2] 并不是乘积小于100的子数组。
    示例 2:
    输入: nums = [1,2,3], k = 0
    输出: 0
    提示:
    1 <= nums.length <= 3 * 104
    1 <= nums[i] <= 1000
    0 <= k <= 106

=== "c++/v1"

    ```c++
    class Solution {
    public:
        int numSubarrayProductLessThanK(vector<int>& nums, int k) {
            int left = 0;
            int right = 0;
            int product = 1;
            int len = nums.size();
            int cnt = 0;
            while(left <= right  && left < len)
            {
                product = product * nums[right];
                if(product < k)
                {
                    cnt++;
                    right++;
                    if(right >= len){
                        left++;
                        right = left;
                        product = 1;
                    }
                }
                else
                {
                    left++;
                    right = left;
                    product = 1;
                }
            }
            return cnt;
        }
    };
    ```
=== "c++/v2"

    ```c++
    class Solution {
    public:
        int numSubarrayProductLessThanK(vector<int>& nums, int k) {
            int L=0, N=nums.size(), product=1, ans=0;
            for (int i = 0; i < N; ++i) 
            {
                product = product * nums[i];
                while (L <= i && product >=k ) product = product/nums[L++];
                ans += (i - L + 1);
            }
            return ans;
        }
    };
    ```
