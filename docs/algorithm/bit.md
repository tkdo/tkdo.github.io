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


=== "python[公式计算]"

    ```python
    class Solution:
        def missingTwo(self, nums: List[int]) -> List[int]:
            n = len(nums) + 2
            sum2 = n * (1 + n) // 2 - sum(nums) # 前n个数和不可能为小数
            t = sum2 // 2 #缺失的这两个值不可能在sum2//2的同一侧
            cur = t * (1 + t) // 2 -  sum([x for x in nums if x <= t]) 
            return [cur, sum2 - cur]
    ```

# 统计数字的二进制中1的个数
    对于任意整数 xx，令 x=x & (x−1)，该运算将 x 的二进制表示的最后一个 1 变成 0。
    例如：3的二进制是11，2的二进制是10，3&2=2，也就是10。消掉了3的最后一个1。

##  [OfferII003-前n个数字二进制中1的个数](https://leetcode.cn/problems/w3tCBm/)
    给定一个非负整数 n，请计算 0 到 n 之间的每个数字的二进制表示中 1 的个数，并输出一个数组。
    示例 1:
    输入: n = 2
    输出: [0,1,1]
    解释: 
    0 --> 0
    1 --> 1
    2 --> 10
    示例2:
    输入: n = 5
    输出: [0,1,1,2,1,2]
    解释:
    0 --> 0
    1 --> 1
    2 --> 10
    3 --> 11
    4 --> 100
    5 --> 101
    说明 :
    0 <= n <= 105

=== "c++[遍历]"
    ```c++
    class Solution {
    public:

        int countOnes(int number){
            int ones = 0;
            while(number > 0)
            {
                number = number & (number - 1);
                ones++;
            }
            return ones;
        }
        vector<int> countBits(int n) {

            vector<int> bits(n + 1);
            for(int i=0; i <=n; i++)
            {
                bits[i] = countOnes(i);
            }
            return bits;
        }
    };
    ```
