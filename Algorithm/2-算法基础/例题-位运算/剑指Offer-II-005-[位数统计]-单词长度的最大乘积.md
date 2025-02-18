题目：[剑指 Offer II 005. 单词长度的最大乘积](https://leetcode.cn/problems/aseY1I/)

- 对于验证两个单词长度的最大乘积，首先需要每两个单词都比较一遍，这个就没办法避免了……

- 考虑加快判断两个字符串是否包含相同的字符：
    - 首先，需要一个单词中的每一个字符都保存起来
    - 之后，比较依次比较两个单词中每一个字符，确定或者都没出现过，或者只有一个单词中出现过
    - 考虑二进制值 与 的特性，`0 & 0 = 0`, `0 & 1 = 1 & 0 = 0`, `1 & 1 =1`，刚好符合要求
    - 用一个数字二进制表示中的每一位记录一个字母是否出现，例如，字符串 `ac` 可以用 `00000000 00000000 00000000 00000101 = 3` 表示
    - 将每一个字符串中字母是否出现均用一个数字表示表示出来，当比较两个字符串是否存在相同字母时，使用 `num1 & num2 == 0 `就可以轻易判断出来了

```c++
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int n = words.size();
        vector<int> words_num(n, 0);
        for (int i = 0; i < n; i++) {
            for (char c : words[i]) {
                words_num[i] |= (1 << (c - 'a'));
            }
        }
        int ans = 0;
        for (int i = 0; i < n-1; i++) {
            for (int j = i + 1; j < n; j++) {
                if ((words_num[i] & words_num[j]) == 0) {
                    int mul = words[i].size() * words[j].size();
                    ans = max(ans, mul);
                }
            }
        }
        return ans;
    }
};
```

