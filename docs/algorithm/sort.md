
## 记数排序
### [leetcode791-自定义字符串排序](https://leetcode.cn/problems/custom-sort-string/)
    给定两个字符串 order 和 s 。order 的所有单词都是 唯一 的，并且以前按照一些自定义的顺序排序。
    对 s 的字符进行置换，使其与排序的order相匹配。更具体地说，如果在order中的字符 x 出现字符 y 之前，那么在排列后的字符串中， x也应该出现在 y 之前。
    返回 满足这个性质的 s 的任意排列。
    示例 1:
    输入: order = "cba", s = "abcd"
    输出: "cbad"
    解释: 
    “a”、“b”、“c”是按顺序出现的，所以“a”、“b”、“c”的顺序应该是“c”、“b”、“a”。
    因为“d”不是按顺序出现的，所以它可以在返回的字符串中的任何位置。“dcba”、“cdba”、“cbda”也是有效的输出。
    示例 2:
    输入: order = "cbafg", s = "abcd"
    输出: "cbad"
    提示:
    1 <= order.length <= 26
    1 <= s.length <= 200
    order和s由小写英文字母组成
    order中的所有字符都 不同


=== "python"

    ```python
    class Solution:
        def customSortString(self, order: str, s: str) -> str:
            freq = Counter(s)
            ans =  []
            for ch in  order:
                if ch in freq:
                    ans.extend([ch] *  freq[ch])
                    freq[ch] = 0
            print(ans)
            for (ch, k) in freq.items():
                if k>0:
                    ans.extend([ch] * k)
            return  "".join(ans)
    ```

=== "c++"

    ```c++
    class Solution {
    public:
        string customSortString(string order, string s) {
            vector<int> freq(26);
            string ans;
            for(auto ch : s)  ++freq[ch-'a'];//freq[ch-'a']++;
            for(auto ch : order)
            {
                if(freq[ch-'a']>0)//如果order里面的字符在s中存在 
                {
                ans += string(freq[ch-'a'], ch);
                freq[ch-'a'] = 0;
                }
            }
            for(int i=0; i<26; i++)
            {
                if(freq[i]>0) ans += string(freq[i], i + 'a');
            }
            return ans;
        }
    };
    ```