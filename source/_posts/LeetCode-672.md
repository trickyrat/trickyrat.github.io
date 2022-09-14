---
title: LeetCode-672. Bulb Switcher II
categories:
  - Algorithms
  - LeetCode
tags: Algorithms
date: 2022-09-15 01:45:51
---

## Description

There is a room with n bulbs labeled from 1 to n that all are turned on initially, and four buttons on the wall. Each of the four buttons has a different functionality where:

- **Button 1**: Flips the status of all the bulbs.  
- **Button 2**: Flips the status of all the bulbs with even labels (i.e., 2, 4, ...).  
- **Button 3**: Flips the status of all the bulbs with odd labels (i.e., 1, 3, ...).
- **Button 4**: Flips the status of all the bulbs with a label j = 3k + 1 where k = 0, 1, 2, ... (i.e., 1, 4, 7, 10, ...).  
  
You must make **exactly** presses button presses in total. For each press, you may pick **any** of the four buttons to press.  

Given the two integers n and presses, return the number of **different possible statuses** after performing all presses button presses.  


## Examples

### Example 1:

Input: n = 1, presses = 1  
Output: 2  
Explanation: Status can be:  
- [off] by pressing button 1  
- [on] by pressing button 2  

### Example 2:

Input: n = 2, presses = 1
Output: 3
Explanation: Status can be:
- [off, off] by pressing button 1
- [on, off] by pressing button 2
- [off, on] by pressing button 3

### Example 3:

Input: n = 3, presses = 1
Output: 4
Explanation: Status can be:
- [off, off, off] by pressing button 1
- [off, on, off] by pressing button 2
- [on, off, on] by pressing button 3
- [off, on, on] by pressing button 4

## Constraints:

- 1 <= n <= 1000
- 0 <= presses <= 1000



## Analysis

## Solution

### C#

```csharp
public class Solution {
    public int FlipLights(int n, int presses) {
        HashSet<int> seen = new HashSet<int>();
        for (int i = 0; i < 1 << 4; i++)
        {
            int[] pressArray = new int[4];
            for (int j = 0; j < 4; j++)
            {
                pressArray[j] = i >> j & 1;
            }

            int sum = pressArray.Sum();
            if (sum % 2 == presses % 2 && sum <= presses)
            {
                int status = pressArray[0] ^ pressArray[1] ^ pressArray[3];
                if (n >= 2)
                {
                    status |= (pressArray[0] ^ pressArray[1]) << 1;
                }
                if (n >= 3)
                {
                    status |= (pressArray[0] ^ pressArray[2]) << 2;
                }
                if (n >= 4)
                {
                    status |= status << 3;
                }
                seen.Add(status);
            }
        }

        return seen.Count;
    }
}
```

### Typescript

```typescript
function flipLights(n: number, presses: number): number {
    let seen = new Set<number>();
    for (let i = 0; i < 16; i++) {
        let pressArray = new Array<number>(4).fill(0);
        for (let j = 0; j < 4; j++) {
            pressArray[j] = (i >> j) & 1;
        }
        let sum = pressArray.reduce((accumulator, currentValue) => accumulator + currentValue);
        if (sum % 2 === presses % 2 && sum <= presses) {
            let status = pressArray[0] ^ pressArray[1] ^ pressArray[3];
            if (n >= 2) {
                status |= (pressArray[0] ^ pressArray[1]) << 1;
            }
            if (n >= 3) {
                status |= (pressArray[0] ^ pressArray[2]) << 2;
            }
            if (n >= 4) {
                status |= (pressArray[0] ^ pressArray[1] ^ pressArray[3]) << 3;
            }
            seen.add(status);
        }
    }
    return seen.size;
};
```

### Python

```python
class Solution:
    def flipLights(self, n: int, presses: int) -> int:
        seen = set()
        for i in range(2**4):
            press_arr = [(i >> j) & 1 for j in range(4)]
            if sum(press_arr) % 2 == presses % 2 and sum(press_arr) <= presses:
                status = press_arr[0] ^ press_arr[1] ^ press_arr[3]
                if n >= 2:
                    status |= (press_arr[0] ^ press_arr[1]) << 1
                if n >= 3:
                    status |= (press_arr[0] ^ press_arr[2]) << 2
                if n >= 4:
                    status |= (press_arr[0] ^ press_arr[1] ^ press_arr[3]) << 3
                seen.add(status)
        return len(seen)
```


### C++

```cpp
class Solution {
public:
    int flipLights(int n, int presses) {
        std::unordered_set<int> seen;
        for (int i = 0; i < 1 << 4; i++) {
            std::vector<int> pressArray(4);
            for (int j = 0; j < 4; j++) {
            pressArray[j] = (i >> j) & 1;
            }
            int sum = std::accumulate(pressArray.begin(), pressArray.end(), 0);
            if (sum % 2 == presses % 2 && sum <= presses) {
            int status = pressArray[0] ^ pressArray[1] ^ pressArray[3];
            if (n >= 2) {
                status |= (pressArray[0] ^ pressArray[1]) << 1;
            }
            if (n >= 3) {
                status |= (pressArray[0] ^ pressArray[2]) << 2;
            }
            if (n >= 4) {
                status |= status << 3;
            }
            seen.emplace(status);
            }
        }
        return seen.size();
    }
};
```


### Java

```java
class Solution {
    public int flipLights(int n, int presses) {
        Set<Integer> seen = new HashSet<>();
        for (int i = 0; i < 1 << 4; i++) {
            int[] pressArray = new int[4];
            for (int j = 0; j < 4; j++) {
                pressArray[j] = i >> j & 1;
            }
            int sum = Arrays.stream(pressArray).sum();
            if (sum % 2 == presses % 2 && sum <= presses) {
                int status = pressArray[0] ^ pressArray[1] ^ pressArray[3];
                if (n >= 2) {
                    status |= (pressArray[0] ^ pressArray[1]) << 1;
                }
                if (n >= 3) {
                    status |= (pressArray[0] ^ pressArray[2]) << 2;
                }
                if (n >= 4) {
                    status |= status << 3;
                }
                seen.add(status);
            }
        }
        return seen.size();
    }
}
```

### Rust

```rust
use std::collections::HashSet;
impl Solution {

    pub fn flip_lights(n: i32, presses: i32) -> i32 {
        let mut seen: HashSet<i32> = HashSet::new();
        for i in 0..16 {
            let mut press_array: Vec<i32> = vec![0;4];
            for j in 0..4 {
                press_array[j] = (i >> j) & 1;
            }
            let sum: i32 = press_array.iter().sum();
            if sum % 2 == presses % 2 && sum <= presses {
                let mut status = press_array[0] ^ press_array[1] ^ press_array[3];
                if n >= 2 {
                    status |= (press_array[0] ^ press_array[1]) << 1;
                }
                if n >= 3 {
                    status |= (press_array[0] ^ press_array[2]) << 2;
                }
                if n >= 4 {
                    status |= (press_array[0] ^ press_array[1] ^ press_array[3]) << 3;
                }
                seen.insert(status);
            }
        }
        seen.len() as i32
    }
}
```


### Go

```go
func flipLights(n int, presses int) int {
	seen := map[int]struct{}{}
	for i := 0; i < 1<<4; i++ {
		pressArray := [4]int{}
		sum := 0
		for j := 0; j < 4; j++ {
			pressArray[j] = i >> j & 1
			sum += pressArray[j]
		}
		if sum%2 == presses%2 && sum <= presses {
			status := pressArray[0] ^ pressArray[1] ^ pressArray[3]
			if n >= 2 {
				status |= (pressArray[0] ^ pressArray[1]) << 1
			}
			if n >= 3 {
				status |= (pressArray[0] ^ pressArray[2]) << 2
			}
			if n >= 4 {
				status |= (pressArray[0] ^ pressArray[1] ^ pressArray[3]) << 3
			}
			seen[status] = struct{}{}
		}
	}
	return len(seen)
}
```

## References

[LeetCode-672. Bulb Switcher II](https://leetcode.cn/problems/bulb-switcher-ii/)
