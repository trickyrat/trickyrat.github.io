---
title: Leetcode-03 Longest Substring Without Repeating Characters
date: 2019-11-22 14:53:11
categories: [Algorithms, LeetCode]
tags: Algorithms
banner_img: /img/bg/Leetcode-03/20191121.jpg
---

## 无重复字符的最长子串（Longest Substring Without Repeating Characters）

## 目录

### 描述

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

### 示例

#### 示例  1:

> 输入: "abcabcbb"  
> 输出: 3  
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

#### 示例 2:

> 输入: "bbbbb"  
> 输出: 1  
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

#### 示例 3:

> 输入: "pwwkew"  
> 输出: 3  
> 解释: 因为无重复字符的最长子串是  "wke"，所以其长度为 3。

请注意，你的答案必须是 子串 的长度，"pwke"  是一个子序列，不是子串。

### 思路

我们可以维护一个范围[i, j)的窗口来保存子串，使用 HashSet/HashMap 作为容器，对 s 进行遍历。如果窗口中不包含有 s[j]，则将 s[j]添加到窗口中并计数，反之，将 s[i]从窗口中移除。也可以使用一个 int 数组来代替 HashSet。

|   数组    |       说明       |
| :-------: | :--------------: |
| int [26]  | 'A'-'Z'和'a'-'z' |
| int [128] |     ASCII 码     |
| int [256] |   ASCII 扩展码   |

时间复杂度为 O(n)，遍历一次 s，n 为 s 的长度；  
空间复杂度为 O(min(m,n))，需要额外的 HashSet，取决于 s 的长度 n 和窗口长度 m。

### 代码

```csharp
class Solution
{
    public int LongestSubstringWithoutRepeating(string s)
    {
        // 96 ms int[]
        // int n = s.Length, ans = 0;
        // int[] index = new int[128]; // new int[256];
        // for (int j = 0, i = 0; j < n; j++)
        // {
        //     i = Math.Max(index[s[j]], i);
        //     ans = Math.Max(ans, j - i + 1);
        //     index[s[j]] = j + 1;
        // }
        // return ans;

        // 100 ms HashSet<char>
        // int len = s.Length;
        // HashSet<char> set = new HashSet<char>();
        // int ans = 0, i = 0, j = 0;
        // while (i < len && j < len)
        // {
        //     if (!set.Contains(s[j]))
        //     {
        //         set.Add(s[j++]);
        //         ans = Math.Max(ans, j - i);
        //     }
        //     else set.Remove(s[i++]);
        // }
        // return ans;

        // 100ms Dictionary<char, int>
        int n = s.Length, ans = 0;
        Dictionary<char, int> dic = new Dictionary<char, int>();
        for (int j = 0, i = 0; j < n; j++)
        {
            if (dic.ContainsKey(s[j]))
            {
                i = Math.Max(dic[s[j]], i);
                dic[s[j]] = j + 1;
            }
            else dic.Add(s[j], j + 1);
            ans = Math.Max(ans, j - i + 1);
        }
        return ans;
    }
}
```

C++代码

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(std::string s) {
        std::vector<int> dic(256, -1);
        int maxlen = 0, start = -1;
        int len = s.length();
        for (int i = 0; i != len; i++) {
            if (dic[s[i]] > start)
                start = dic[s[i]];
            dic[s[i]] = i;
            maxlen = std::max(maxlen, i - start);
        }
        return maxlen;
    }
}
```

Python 代码

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        size = len(s)
        ans = i = 0
        index = [0] * 128
        for j in range(0, size):
            i = max(index[ord(s[j])], i)
            ans = max(ans, j - i + 1)
            index[ord(s[j])] = j + 1
        return ans
```

Typescript 代码

```typescript
function lengthOfLongestSubstring(s: string): number {
  let len: number = s.length,
    ans: number = 0;
  let index: Array<number> = new Array<number>(128).fill(0);
  for (let i = 0, j = 0; j < len; j++) {
    i = Math.max(index[s[j].charCodeAt(0)], i);
    ans = Math.max(ans, j - i + 1);
    index[s[j].charCodeAt(0)] = j + 1;
  }
  return ans;
}
```
