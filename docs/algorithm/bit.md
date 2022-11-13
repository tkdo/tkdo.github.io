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


##  [Offer-II-005-单词长度的最大乘积](https://leetcode.cn/problems/aseY1I/)
    给定一个字符串数组words，请计算当两个字符串 words[i] 和 words[j] 不包含相同字符时，它们长度的乘积的最大值。假设字符串中只包含英语的小写字母。如果没有不包含相同字符的一对字符串，返回 0。
    示例1:
    输入: words = ["abcw","baz","foo","bar","fxyz","abcdef"]
    输出: 16 
    解释: 这两个单词为 "abcw", "fxyz"。它们不包含相同字符，且长度的乘积最大。
    示例 2:
    输入: words = ["a","ab","abc","d","cd","bcd","abcd"]
    输出: 4 
    解释: 这两个单词为 "ab", "cd"。
    示例 3:
    输入: words = ["a","aa","aaa","aaaa"]
    输出: 0 
    解释: 不存在这样的两个单词。
    提示：
    2 <= words.length <= 1000
    1 <= words[i].length <= 1000
    words[i]仅包含小写字母

=== "python"

    ```python
    class Solution:
        def maxProduct(self, words: List[str]) -> int:
            # 使用位运算来实现hashset
            lengths = [0] * len(words)
            whash  = [0] * len(words)
            ans = 0
            for i in  range(len(words)):
                lengths[i] = len(words[i])
                # 计算每个word的hash
                for c in words[i]:
                    whash[i] |= (1 << (ord(c) - ord('a'))) #计算每个字符串的hash值
                    #print(hashs)
                for j in range(i):
                    if whash[i] & whash[j] ==  0: # 不含相同的char
                        ans = max(ans, lengths[i] * lengths[j])
            return ans
    ```
    ```c++
    class Solution {
    public:
        int maxProduct(vector<string>& words) {
            //使用位运算来实现hash
            int lenghts[words.size()];
            int hashs[words.size()];
            memset(lenghts,0,sizeof(lenghts));
            memset(hashs,0,sizeof(hashs));//给数组用0进行初始化
            int ans = 0;
            for(int i=0; i <words.size(); i++)
            {
                lenghts[i]  = words[i].size();
                for(auto c : words[i])
                {
                    cout << c <<endl;
                    //hashs[i] = hashs[i] | 1 << c - 'a';
                    hashs[i] |= (1 << (c - 'a')); //为了保证计算优先级多用括号括下
                    //cout << hashs[i] <<endl;
                }
                for(int j=0; j<i; j++){
                    if((hashs[i] & hashs[j]) == 0) //字符计算优先级
                    {
                        ans = max(ans, lenghts[i]*lenghts[j]);
                        //cout << i<<j << endl;
                    } 
                }
            }
            return ans;
        }
    };
    ```