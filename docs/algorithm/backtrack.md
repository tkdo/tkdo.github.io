



##  [leetcode-784-字母大小写全排列](https://leetcode.cn/problems/letter-case-permutation/)

    给定一个字符串s，通过将字符串s中的每个字母转变大小写，我们可以获得一个新的字符串。
    返回 所有可能得到的字符串集合 。以 任意顺序 返回输出。
    示例 1：
    输入：s = "a1b2"
    输出：["a1b2", "a1B2", "A1b2", "A1B2"]
    示例 2:
    输入: s = "3z4"
    输出: ["3z4","3Z4"]


=== "python"
```python
class Solution:
    def letterCasePermutation(self, s: str) -> List[str]:
        ans = []
        def dfs(s: List[str], pos: int) -> None:
            while pos < len(s) and s[pos].isdigit():
                pos += 1
            if pos == len(s):
                ans.append(''.join(s))
                return
            dfs(s, pos + 1)
            s[pos] = s[pos].swapcase()
            dfs(s, pos + 1)
            s[pos] = s[pos].swapcase()
        dfs(list(s), 0)
        return ans
```
=== "c++"
```c++
class Solution {
public:
    vector<string> ans;
    void dfs (string a, int s) {
        ans.push_back(a);
        for (int i = s; i < a.size(); i ++ ) {
            if (!isdigit(a[i])) {
                a[i] ^= 32;
                dfs (a, i + 1);
                a[i] ^= 32;
            }
        }
    }
    vector<string> letterCasePermutation(string s) {
        dfs (s, 0);
        return ans;
    }
};
```

