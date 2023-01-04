---
title: LeetCode-02 Add Two Numbers
date: 2019-11-21 23:22:16
categories: [Algorithms, LeetCode]
tags: Algorithms
banner_img: /img/bg/LeetCode-02/20191117.jpg
---

### 描述

给出两个非空的链表用来表示两个非负的整数。其中，它们各自的位数是按照逆序的方式存储的，并且它们的每个节点只能存储一位数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0  开头。

### 示例

> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)  
> 输出：7 -> 0 -> 8  
> 原因：342 + 465 = 807

### 分析

其实和我们算加法一样，从个位开始计算就行了,如图所示  
![figure1](/img/bg/Leetcode-02/figure1.png)
只要把两条链表相应位置相加，记录是否有进位(通过 sum/10 来判断)，有则进位，添加到一条新的链表结点就行了。值得注意的是，相加后有几种情景，分别是

|  情景  |    结果    |
| :----: | :--------: |
|  1+1   |   不进位   |
|  12+8  |  进一次位  |
| 9993+7 |  连续进位  |
|  0+12  | 有空的结点 |

对应结点的和 sum = x + y + carry，carry 则是 sum/10，对应的该位结点值为 sum%10。

因为只需要遍历最长的链表，挨个结点相加，所以时间复杂度为 O(max(m,n))，m 和 n 分别是两条链表的长度；  
需要新的一条链表来记录结果，最多为 max(m,n)+1(最高位有进位情况)，所以空间复杂度为 O(max(m,n))。

### 代码

#### C#

```csharp
class Solution
{
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2)
    {
        ListNode dummyHead = new ListNode(0);
        if (l1 == null && l2 == null)
            return dummyHead;
        int carry = 0;
        ListNode curr = dummyHead;
        while (l1 != null || l2 != null)
        {
            int num1 = l1?.val ?? 0;
            int num2 = l2?.val ?? 0;
            int sum = num1 + num2 + carry;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            carry = sum / 10;
            l1 = l1?.next;
            l2 = l2?.next;
        }
        if (carry != 0)
        {
            curr.next = new ListNode(carry);
        }
        return dummyHead.next;
    }
}
```

#### C++

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode preHead(0), *p = &preHead;
        int carry = 0;
        while (l1 || l2 || carry) {
            int sum = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + carry;
            carry = sum / 10;
            p->next = new ListNode(sum % 10);
            p = p->next;
            l1 = l1 ? l1->next : l1;
            l2 = l2 ? l2->next : l2;
        }
        return preHead.next;
    }
};
```

#### Python

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        carry = 0
        dummy_head = ListNode(None)
        curr = dummy_head
        while l1 or l2:
            sum = 0;
            if l1:
                sum += l1.val
                l1 = l1.next
            if l2:
                sum += l2.val
                l2 = l2.next
            sum +=  carry
            curr.next = ListNode(sum%10)
            curr = curr.next
            carry = sum//10
        if carry != 0:
            curr.next = ListNode(carry)
        return dummy_head.next
```

#### Rust

```rust
impl Solution {
    pub fn add_two_numbers(l1: Option<Box<ListNode>>, l2: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        carried(l1, l2, 0)
    }
}
fn carried(l1: Option<Box<ListNode>>, l2: Option<Box<ListNode>>, mut carry: i32) -> Option<Box<ListNode>> {
  if l1.is_none() && l2.is_none() && carry == 0 {
    None
  } else {
    Some(Box::new(ListNode {
      next: carried(l1.and_then(|x| {carry += x.val; x.next}),
                    l2.and_then(|x| {carry += x.val; x.next}),
                    carry / 10
      ),
      val: carry % 10
    }))
  }
}
```

#### Typescript

```typescript
function addTwoNumbers(
  l1: ListNode | null,
  l2: ListNode | null
): ListNode | null {
  let dummyHead: ListNode | null = new ListNode();
  let carry: number = 0;
  let curr: ListNode | null = dummyHead;
  while (l1 || l2) {
    let sum: number = 0;
    if (l1) {
      sum += l1.val;
      l1 = l1.next;
    }
    if (l2) {
      sum += l2.val;
      l2 = l2.next;
    }
    sum += carry;
    curr.next = new ListNode(sum % 10);
    curr = curr.next;
    carry = Math.floor(sum / 10);
  }
  if (carry != 0) {
    curr.next = new ListNode(carry);
  }
  return dummyHead.next;
}
```

#### Javascript

```javascript
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    let dummyHead = new ListNode(0);
    let carry = 0;
    let curr = dummyHead;
    while (l1 || l2 || carry > 0) {
        let num1 = l1 ? l1.val : 0;
        let num2 = l2 ? l2.val : 0;
        let sum = num1 + num2 + carry;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        carry = Math.floor(sum / 10);
        l1 = l1 ? l1.next : l1;
        l2 = l2 ? l2.next : l2;
    }
    if (carry != 0) curr.next = new ListNode(carry);
    return dummyHead.next;
};
```

#### Java

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(0);
        if (l1 == null && l2 == null)
            return head;
        int sum = 0, carry = 0;
        ListNode curr = head;
        while (l1 != null || l2 != null) {
            int num1 = l1 == null ? 0 : l1.val;
            int num2 = l2 == null ? 0 : l2.val;
            sum = num1 + num2 + carry;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            carry = sum / 10;
            l1 = l1 == null ? l1 : l1.next;
            l2 = l2 == null ? l2 : l2.next;
        }
        if (carry != 0)
            curr.next = new ListNode(carry);
        return head.next;
    }
}
```

### 引用

截图来自[LeetCode](https://leetcode.cn/problems/add-two-numbers).
