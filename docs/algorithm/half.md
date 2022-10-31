
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


