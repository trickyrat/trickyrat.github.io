---
date: "2019-11-20 21:10:36"
layout: post
title: LeetCode-01 Two Sum
categories: [Algorithms, LeetCode]
tags: Algorithms
banner_img: /img/bg/LeetCode-01/20191116.jpg
---

## Description

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.  

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.  

You can return the answer in any order.  


## Examples

### Example 1

> Input: nums = [2,7,11,15], target = 9
> Output: [0,1]
> Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

### Example 2

> Input: nums = [3,2,4], target = 6
> Output: [1,2]

### Example 3

> Input: nums = [3,3], target = 6
> Output: [0,1]

## Analysis

We can solve it by two for loop and this solution will take O(n^2) time complexity and O(1) space complexity. Fortunately, we can use a hash table to save the pair that target-nums[i] as key and i as value and find the key if exists in in nums. This soluion will take O(n) time complexity but O(n) space complexity.

## Solution

### C#

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

### C++

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

### Python

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

### Rust

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

### Javascript

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

### Typescript

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

### Go

```go
// 1.Two Sum
func twoSum(nums []int, target int) []int {
	hashTable := map[int]int{}
	for i, x := range nums {
		if p, ok := hashTable[target-x]; ok {
			return []int{p, i}
		}
		hashTable[x] = i
	}
	return nil
}
```

### Java

```java
public class Solution {
    /**
     * 1. Two Sum
     *
     * @param nums
     * @param target
     * @return
     */
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; ++i) {
            if (map.containsKey(target - nums[i])) {
                res[1] = i;
                res[0] = map.get(target - nums[i]);
                break;
            }
            if (!map.containsKey(nums[i])) {
                map.put(nums[i], i);
            }
        }
        return res;
    }
}
```