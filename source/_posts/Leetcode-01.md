---
date: "2019-11-20 21:10:36"
layout: post
title: Leetcode-01 Two Sum
categories: [Algorithms, LeetCode]
tags: Algorithms
banner_img: /img/bg/Leetcode-01/20191116.jpg
---

## 两数之和（Two Sum）

### 题目描述

给定一个整数数组和一个目标值，找出数组中和为目标值的两个数，
你可以假设每个输入只对应一个答案，且同样的元素不能被重复利用。

### 示例

> 给定数组 nums = [2, 7, 11, 15], target = 9，  
> 因为 nums[0] + nums[1] = 2 + 7 =9，  
> 所以返回结果 [0, 1]。

### 分析过程：

初看题目，首先想到两次循环暴力匹配。遍历数组 nums 查找是否有满足 target-nums[i]的元素。显然时间复杂度为 O(n^2)，空间复杂度为 O(1)。​

C# 代码

```csharp
public class Solution {
    public static int[] TwoSum(int[] nums, int target)
    {
        int[] res = new int[2];
        int len = nums.Length;
        for (int i = 0; i < len; i++)
        {
            int numberToFind = target - nums[i];
            for (int j = i + 1; j < len; j++)
            {
                if(nums[j] == numberToFind)
                {
                    res[0] = i;
                    res[1] = j;
                    return res;
                }
            }
        }
        return res;
    }
}
```

其实细想，我们只需要遍历一次数组，将然后根据 target-nums[i]为 key 在 HashTable 中查找是否有与之匹配的 value，如果有则说明找到了目标，并将当前索引和 hashtable 中的 value 返回；没有则将当前值作为 key，当前索引作为 value 存入 hashtable 中。  
因为 hashtable 的查询平均时间复杂度为 O(1)，所以我们的算法时间复杂度从 O(n^2)降到 O(n),因为引入了一个 hashtable，并且最坏情况需要将整个数组的数据放入，所以空降复杂度为 O(n)。

C# 代码

```csharp
public class Solution {
    public int[] TwoSum(int[] nums, int target)
    {
        int[] res = new int[2];
        Dictionary<int,int> dict = new Dictionary<int, int>();
        for (int i = 0; i < nums.Length; i++)
        {
            if(dic.TryGetValue(target - nums[i], out int value)) // O(1)
            {
              res[1] = i;
              res[0] = value;
              break;
            }
            if (!dic.ContainsKey(nums[i])) // O(1)
            {
                dic.Add(nums[i], i);
            }
        }
        return res;
    }
}
```

C++ 代码

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int size = nums.size();
        unordered_map<int, int> hash;
        vector<int> result;
        for (int i = 0; i < size; i++) {
            int numberToFind = target - nums[i];
            if (hash.contains(numberToFind)) {
                result.push_back(hash[numberToFind]);
                result.push_back(i);
                return result;
            }
            hash[nums[i]] = i;
        }
        return result;
    }
};
```

Python 代码

```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if len(nums) <= 1:
            return False
        buff_dict = {}
        for i in range(len(nums)):
            if nums[i] in buff_dict:
                return [buff_dict[nums[i]], i]
            else:
                buff_dict[target - nums[i]] = i
```

Rust 代码

```rust
use std::collections::HashMap;
impl Solution {
    pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
        let mut hash = HashMap::new();
        for (index, value) in nums.iter().enumerate() {
            let number_to_find = target - *value;
            if hash.contains_key(&number_to_find) {
                return vec![hash[&number_to_find], index as i32];
            }
            hash.insert(value, index as i32);
        }
        return vec![];
    }
}
```

Javascript 代码

```javascript
var twoSum = function (nums, target) {
  var hash = {};
  for (let i = 0; i < nums.length; i++) {
    let index = hash[target - nums[i]];
    if (typeof index !== "undefined") {
      return [index, i];
    }
    hash[nums[i]] = i;
  }
  return null;
};
```

Typescript 代码

```typescript
function twoSum(nums: number[], target: number): number[] {
  let res: number[] = [0, 0];
  let dic: Map<number, number> = new Map<number, number>();
  for (let i = 0; i < nums.length; i++) {
    if (dic.has(target - nums[i])) {
      res[0] = dic.get(target - nums[i]) as number;
      res[1] = i;
      return res;
    } else {
      dic.set(nums[i], i);
    }
  }
  return [];
}
```
