---
title: Leetcode-242 Valid Anagram
date: 2020-04-09 12:52:50
categories: [算法, LeetCode]
tags: 算法
banner_img: /img/bg/Leetcode-242/landscape.jpg
---

## 题目描述

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

## 示例

示例  1:

> 输入: s = "anagram", t = "nagaram"  
> 输出: true

示例 2:

> 输入: s = "rat", t = "car"  
> 输出: false

## 说明

异位词是指两个字符串所含相同字母的个数相同，你可以假设字符串只包含小写字母。

## 思路

这道题其实就是在考察怎么统计字符出现的次数，我们可以使用数组或者 hashmap 来作为字母的容器，遍历`s`统计该字符串每个字母出现的次数，将其放到数组中存储，然后再遍历`t`，如果对应的字母出现，便将数组中的值减一，直到最后数组中的元素应该都为零，否则`s`和`t`便不是异位词。  
其实可以再优化一下，只需要一次遍历便可以将两个字符串统计完成。因为只考虑小写字母可以使用一个 int[26]的数组作为容器，或者是 Dictionary<char, int>的 hashmap 作为容器去统计。只需遍历整个字符串，所以时间复杂度 O(n)；只需一个固定大小的数组，所以空间复杂度为 O(1)

## 代码

```csharp
public bool IsAnagram(string s, string t)
{
    if(s.Length != t.Length) // 长度不相等肯定不是
    {
        return false;
    }
    int len = s.Length;
    int[] alpha = new int[26];
    for(int i = 0; i < len; i++)
    {
        alpha[s[i] - 'a']++;  // 字母出现一次便在相应的元素下加一
        alpha[t[i] - 'a']--;  // 字母出现一次便在相应的元素下减一
    }
    return alpha.All(x => x == 0);  // 判断元素是否全为零
}
```
