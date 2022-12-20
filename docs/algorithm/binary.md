
## [leetcode-OfferII001整数除法](https://leetcode.cn/problems/xoh6Oh/)
    给定两个整数 a 和 b ，求它们的除法的商 a/b ，要求不得使用乘号 '*'、除号 '/' 以及求余符号 '%'。
    注意：
    整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8以及truncate(-2.7335) = -2
    假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2^31,2^31−1]。本题中，如果除法结果溢出，则返回 2^31− 1
    示例 1：
    输入：a = 15, b = 2
    输出：7
    解释：15/2 = truncate(7.5) = 7
    示例 2：
    输入：a = 7, b = -3
    输出：-2
    解释：7/-3 = truncate(-2.33333..) = -2
    示例 3：
    输入：a = 0, b = 1
    输出：0
    示例 4：
    输入：a = 1, b = 1
    输出：1
    提示:
    -2^31<= a, b <= 2^31- 1
    b != 0


=== "c++[暴力]"

    ```c++
    class Solution {
    public:
        int divide(int a, int b) {
            if (b==0)
                return 0;
            int sign = 1;
            if ((a>0) ^ (b>0))
                sign = -1;
            //32位最大值为:2**31 -1 = 2147483647
            //32位最小值为:-2**31 = -2147483648
            //-2147483648/(-1) = 2147483648 > 2147483647 越界了
            if(a==INT_MIN &&  b == -1)
            return INT_MAX;
            //abs(-2147483648) = -2147483648
            cout<<abs(INT_MIN)<<endl;
            if(a>0) a = -a;
            if(b>0) b = -b;
            int res = 0;
            while (a <= b)
            {
                a = a - b;
                res = res + 1;
            }
            return sign*res;
        }
    };
    ```

## [leetcode-1760 袋子里最少数目的球](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/)
    给你一个整数数组nums，其中nums[i]表示第i个袋子里球的数目。同时给你一个整数maxOperations。
    你可以进行如下操作至多maxOperations次：
    选择任意一个袋子，并将袋子里的球分到2 个新的袋子中，每个袋子里都有 正整数个球。
    比方说，一个袋子里有5个球，你可以把它们分到两个新袋子里，分别有 1个和 4个球，或者分别有 2个和 3个球。
    你的开销是单个袋子里球数目的 最大值，你想要 最小化开销。

    请你返回进行上述操作后的最小开销。
    示例 1：
    输入：nums = [9], maxOperations = 2
    输出：3
    解释：
    - 将装有 9 个球的袋子分成装有 6 个和 3 个球的袋子。[9] -> [6,3] 。
    - 将装有 6 个球的袋子分成装有 3 个和 3 个球的袋子。[6,3] -> [3,3,3] 。
    装有最多球的袋子里装有 3 个球，所以开销为 3 并返回 3 。
    示例 2：
    输入：nums = [2,4,8,2], maxOperations = 4
    输出：2
    解释：
    - 将装有 8 个球的袋子分成装有 4 个和 4 个球的袋子。[2,4,8,2] -> [2,4,4,4,2] 。
    - 将装有 4 个球的袋子分成装有 2 个和 2 个球的袋子。[2,4,4,4,2] -> [2,2,2,4,4,2] 。
    - 将装有 4 个球的袋子分成装有 2 个和 2 个球的袋子。[2,2,2,4,4,2] -> [2,2,2,2,2,4,2] 。
    - 将装有 4 个球的袋子分成装有 2 个和 2 个球的袋子。[2,2,2,2,2,4,2] -> [2,2,2,2,2,2,2,2] 。
    装有最多球的袋子里装有 2 个球，所以开销为 2 并返回 2 。
    示例 3：
    输入：nums = [7,17], maxOperations = 2
    输出：7


=== "c++[暴力]"

    ```c++
    class Solution {
    public:
        int minimumSize(vector<int>& nums, int maxOperations) {
            int left = 1, right = *max_element(nums.begin(), nums.end());
            int ans = 0;
            while(left <= right)
            {
                int y = (left + right) / 2;
                long long ops = 0;
                for(int x: nums){
                    ops += (x-1) / y;
                }
                if (ops <= maxOperations)
                {
                    ans = y;
                    right =  y - 1;
                }
                else
                {
                    left = y + 1;
                }
            }
            return ans;
        }
    };
    ```
