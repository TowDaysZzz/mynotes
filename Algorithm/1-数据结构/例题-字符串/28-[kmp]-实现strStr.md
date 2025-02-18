题目：[28. 实现 strStr()](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

## 调库

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int idx = haystack.find(needle);
        return idx;
    }
};
```

## 朴素匹配

时间复杂度：`O(n*m)`

空间复杂度：`O(1)`

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size();
        int m = needle.size();
        if (m == 0) return 0;
        if (n < m) return -1;

        for (int i = 0; i < n - m + 1; i++) {
            bool flag = true;
            for (int j = 0; j < m; j++) {
                if (haystack[i + j] != needle[j]) {
                    flag = false;
                    break;
                }
            }
            if (flag) return i;
        }

        return -1;
    }
};
```

## kmp

时间复杂度：`O(n+m)`

空间复杂度：`O(m)`

```c++
class Solution {
public:
    // KMP 算法
    // haystack: 原串(string)  needle: 匹配串(pattern)
    int strStr(string haystack, string needle) {

        if (needle.empty()) return 0;
        
        // 分别读取原串和匹配串的长度
        int n = haystack.size(), m = needle.size();
        // 原串和匹配串前面都加空格，使其下标从 1 开始
        haystack = " " + haystack;
        needle = " " + needle;

        // 构建 next 数组，数组长度为匹配串的长度（next 数组是和匹配串相关的）
        vector<int> next(m+1, 0);

        // 构造过程 i = 2，j = 0 开始，i 小于等于匹配串长度 【构造 i 从 2 开始】
        for (int i = 2, j = 0; i <= m; i++) {
            // 匹配不成功的话，j = next(j)
            while (j > 0 && needle[i] != needle[j + 1]) j = next[j];
            // 匹配成功的话，先让 j++
            if (needle[i] == needle[j + 1]) j++;
            // 更新 next[i]，结束本次循环，i++
            next[i] = j;
        }

        // 匹配过程，i = 1，j = 0 开始，i 小于等于原串长度 【匹配 i 从 1 开始】
        for (int i = 1, j = 0; i <= n; i++) {
            // 匹配不成功 j = next(j)
            while (j > 0 && haystack[i] != needle[j + 1]) j = next[j];
            // 匹配成功的话，先让 j++，结束本次循环后 i++
            if (haystack[i] == needle[j + 1]) j++;
            // 整一段匹配成功，直接返回下标
            if (j == m) return i - m;
        }

        return -1;
    }
};
```

