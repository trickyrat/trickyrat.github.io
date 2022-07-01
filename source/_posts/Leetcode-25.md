---
title: Leetcode-25 Reverse Nodes in k-Group
date: 2020-04-09 13:34:52
categories: [Algorithms, LeetCode]
tags: Algorithms
banner_img: /img/bg/Leetcode-25/the-road.jpg
---

## 题目描述

给你一个链表，每  k  个节点一组进行翻转，请你返回翻转后的链表。  
k  是一个正整数，它的值小于或等于链表的长度。  
如果节点总数不是  k  的整数倍，那么请将最后剩余的节点保持原有顺序。

## 示例

> 给你这个链表：1->2->3->4->5  
> 当  k = 2 时，应当返回: 2->1->4->3->5  
> 当  k = 3 时，应当返回: 3->2->1->4->5

## 说明

- 你的算法只能使用常数的额外空间。
- 你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

## 思路

这道题可以拆分为两个步骤，首先将链表根据 k 拆分成若干组，然后每一组开始反转链表。  
分组很简单，比如，链表为`[1, 2, 3, 4, 5], k = 2`  
先拆分成`[1, 2]` 和 `[3, 4, 5]`，  
然后递归去拆分`[3, 4, 5]`，最终得到`[1, 2], [3, 4], [5]` 三组数据。其实会发现每拆分一组时，就可以进行链表的反转，然后将反转后的链表在与之前的组合并便能得到结果。

反转链表的代码模板

```csharp
ListNode prev = null;
ListNode curr = head;
while(curr != null)
{
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}
```

这里的 prev 节点便是我们递归一次返回处理好的链表结点。时间复杂度为 O(n)，空间复杂度为 O(1)。

## 代码

C# 代码

```csharp
public ListNode ReverseKGroup(ListNode head, int k)
{
    ListNode tmp = head;
    int count = 0;
    while (count < k) // 分组
    {
        if(tmp == null)
            return head;
        tmp = tmp.next;
        count++;
    }
    ListNode prev = ReverseKGroup(tmp, k);
    while (count-- > 0) // 反转链表
    {
        ListNode next = head.next;
        head.next = prev;
        prev = head;
        head = next;
    }
    return prev;
}
```

C++ 代码

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if(head == nullptr) {
            return nullptr;
        }
        ListNode *tmp = head;
        int count = 0;
        while(count < k) {
            if(!tmp) {
                return head;
            }
            tmp = tmp->next;
            count++;
        }
        ListNode *prev = reverseKGroup(tmp, k);
        while(count-- > 0) {
            ListNode* next = head->next;
            head->next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
};
```
