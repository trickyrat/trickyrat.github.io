---
title: Leetcode-33 Search in Rotated Sorted Array
date: 2021-10-26 13:13:38
categories: [Algorithms, LeetCode]
tags: Algorithms
banner_img: /img/bg/Leetcode-25/the-road.jpg
---

## 题目描述

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为  [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回  -1 。

## 示例

### 示例 1

> 输入：nums = [4,5,6,7,0,1,2], target = 0  
> 输出：4

### 示例 2

> 输入：nums = [4,5,6,7,0,1,2], target = 3  
> 输出：-1

### 示例 3

> 输入：nums = [1], target = 0  
> 输出：-1

## 提示

> 1 <= nums.length <= 5000  
> -10^4 <= nums[i] <= 10^4  
> nums 中的每个值都 独一无二  
> 题目数据保证 nums 在预先未知的某个下标上进行了旋转
> -10^4 <= target <= 10^4

## 进阶

你可以设计一个时间复杂度为 O(log n) 的解决方案吗？

## 思路

本题主要考察在有序的数组中查找元素。看到有序数组查找，第一时间我们应该想到的是二分查找，但是二分查找针对的是整个数组都是有序的。其实，我们可以发现在数组反转后，仍然可以找到一部分是有序的数组，我们只需要判断目标值是否落在这个有序的数组部分。所以我们需要做两个判断，首先判断拆分后的数组，哪一个部分是有序的以及目标值是否在此区间内，即

1. 如果[l, mid - 1]是有序的且 target 在[nums[l],nums[mid]]范围内，则搜索范围缩小到[l, mid - 1],反之则在[mid+1, ,r]中查找
2. 如果[mid, r]是有序的且 target 在[nums[mid + 1], nums[r]]范围内，则搜索范围缩小到[mid + 1, r]，反之则在[l, mid - 1]中查找

这里判断哪个部分是否有序，只需要取中间元素和首尾元素进行比较就行了。

## 代码

```csharp
public class Solution {
    public int Search(int[] nums, int target) {
        int n = nums.Length;
        if (n == 0) // 空的数组
        {
            return -1;
        }
        if (n == 1) // 只有一个元素
        {
            return nums[0] == target ? 0 : -1;
        }
        int l = 0, r = n - 1;
        while (l <= r)
        {
            int mid = l + (r - l) / 2;
            if (nums[mid] == target)
            {
                return mid;
            }
            if (nums[0] <= nums[mid]) // 左边部分有序
            {
                if (nums[0] <= target && target < nums[mid])
                {
                    r = mid - 1;
                }
                else
                {
                    l = mid + 1;
                }
            }
            else
            {
                if (nums[mid] < target && target <= nums[n - 1])
                {
                    l = mid + 1;
                }
                else
                {
                    r = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

C++ 代码

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
      int n = (int)nums.size();
      if (n == 0) {
        return -1;
      }
      if (n == 1) {
        return nums[0] == target ? 0 : -1;
      }
      int l = 0, r = n - 1;
      while (l <= r) {
        int mid = (l + r) / 2;
        if (nums[mid] == target) {
          return mid;
        }
        if (nums[0] <= nums[mid]) {
          if (nums[0] <= target && target < nums[mid]) {
            r = mid - 1;
          } else {
            l = mid + 1;
          }
        } else {
          if (nums[mid] < target && target <= nums[n - 1]) {
            l = mid + 1;
          } else {
            r = mid - 1;
          }
        }
      }
      return -1;
    }
};
```

Python 代码

```python
class Solution:
  def search(self, nums: List[int], target: int) -> int:
      """
      33. Search in Rotated Sorted Array
      """
      n = len(nums)
      if n == 0:
          return -1
      if n == 1:
          return 0 if nums[0] == target else -1
      l = 0
      r = n - 1
      while l <= r:
          mid = l + (r - l) // 2
          if nums[mid] == target:
              return mid
          if nums[0] <= nums[mid]:
              if nums[0] <= target and target < nums[mid]:
                  r = mid - 1
              else:
                  l = mid + 1
          else:
              if nums[mid] < target and target <= nums[n - 1]:
                  l = mid + 1
              else:
                  r = mid - 1
      return -1
```

C 代码

```c
int search(int* nums, int numSize, int target) {
    if (numSize < 1) {
        return -1;
    }
    if(numSize == 1) {
        return nums[0] ==  target ? 0 : -1;
    }
    int l = 0, r = numSize - 1;
    while(l <= r) {
        int mid = l + (r - l) / 2;
        if(nums[mid] == target) {
            return mid;
        }
        if(nums[0] <= nums[mid]) {
            if(nums[0] <= target && target < nums[mid]) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        } else {
            if(nums[mid] < target && target <= nums[numSize - 1]) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
    }
    return -1;
}
```
