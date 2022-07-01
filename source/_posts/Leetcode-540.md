---
title: Leetcode-540 Single Element in a Sorted Array
date: 2022-02-14 20:52:33
categories:  [Algorithms, LeetCode]
tags: Algorithms
---


## Description

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.  

Return the single element that appears only once.  

Your solution must run in O(log n) time and O(1) space.  

## Examples

Example  1:

> Input: nums = [1,1,2,3,3,4,4,8,8]  
> Output: 2  

Example 2:

> Input: nums =  [3,3,7,7,10,11,11]  
> Output: 10  

## 思路

因为只出现一次的元素下标x的左右两边均有偶数个元素，那么数组的长度一定为奇数。又因数组是有序的，那么相同的元素必然是相连的。  
当索引为x的元素，如果其左边的元素的索引满足 `nums[y] = nums[y+1]` y必定为偶数；相对地，如果其右边元素的索引满足 `nums[y] = nums[y+1]` y必定为奇数。  
我们可以得出数组中存在奇偶分界条件，则可以使用二分搜索来找到改分界点，便是目标元素。搜索的左右边界分别为0为数组最大索引，即[0, N],N为数组长度减一。  
当mid为偶数时，我们需要比较nums[mid]和nums[mid+1]是否相等   
当mid为奇数时，我们需要比较nums[mid]和nums[mid-1]是否相等  
如果`mid < x`，那么目标值便是在右侧，相应地，当 `mid > x`，则目标值在左侧。特殊地，我们可以使用异或操作目标，因为当mid为偶数时，`mid + 1 = mid ^ 1`；当mid为奇数时，`mid - 1 = mid ^ 1`。因此可以不用再去判断mid的奇偶性，而是判断 `nums[mid]` 和 `nums[mid ^ 1]` 大小进行搜索。

## Solution

### C#

```csharp
public int SingleNonDuplicate(int[] nums)
{
    int low = 0, high = nums.Length - 1;
    while (low < high)
    {
        int mid = (high - low) / 2 + low;
        if (nums[mid] == nums[mid ^ 1])
        {
            low = mid + 1;
        }
        else
        {
            high = mid;
        }
    }
    return nums[low];
}
```

### Typescript

```typescript
function singleNonDuplicate(nums: number[]): number
{
    let low = 0, high = nums.length - 1;
    while (low < high)
    {
        let mid = Math.floor((high - low) / 2) + low;
        if (nums[mid] == nums[mid ^ 1])
        {
            low = mid + 1;
        }
        else
        {
            high = mid;
        }
    }
    return nums[low];
}
```

### Python

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        low, high = 0, len(nums) - 1
        while low < high:
            mid = (low + high) // 2
            if nums[mid] == nums[mid ^ 1]:
                low = mid + 1
            else:
                high = mid
        return nums[low]
```

### C++

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;
        while (low < high) {
            int mid = (high - low) / 2 + low;
            if (nums[mid] == nums[mid ^ 1]) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return nums[low];
    }
};
```