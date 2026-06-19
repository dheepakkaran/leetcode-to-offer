# 485. Max Consecutive Ones

| | |
|---|---|
| **Difficulty** | Easy |
| **Topic** | Array |
| **LeetCode Link** | [Problem](https://leetcode.com/problems/max-consecutive-ones/) |
| **Study Plan** | LeetCode 75 |
| **Time Complexity** | O(n) |
| **Space Complexity** | O(1) |

---

## Problem

Given a binary array `nums`, return the **maximum number of consecutive `1`s** in the array.

**Example:**
```
Input:  nums = [1, 1, 0, 1, 1, 1]
Output: 3

The last three elements form the longest streak of consecutive 1s.
```

---

## Approach — Linear Scan with Reset

**Intuition:**  
Walk through the array once. Keep a running count `curr` of the current streak of `1`s.  
The moment a `0` appears, the streak is broken — reset `curr` to `0`.  
At every step, update `best` if `curr` beats it.

- No extra space needed — two variables only
- One pass is enough → O(n)

```
nums = [1, 1, 0, 1, 1, 1]

num=1 → curr=1   best=1
num=1 → curr=2   best=2
num=0 → curr=0   best=2   ← streak broken, reset
num=1 → curr=1   best=2
num=1 → curr=2   best=2
num=1 → curr=3   best=3 ✅
```

---

## Solution

```python
from typing import List

class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        curr = 0
        best = 0

        for num in nums:
            if num == 1:
                curr += 1
                best = max(best, curr)    # update max at every step
            else:
                curr = 0                  # 0 encountered → streak broken

        return best
```

---

## Why update `best` inside the loop (not after)?

```
nums = [1, 1, 1]    ← no zeros at all

If we updated best only on reset (at 0s), we'd never update it here.
Updating inside the loop on every 1 guarantees the final streak is captured.

num=1 → curr=1  best=1
num=1 → curr=2  best=2
num=1 → curr=3  best=3  ✅  (no 0 ever came — best still captured)
```

---

## Edge Cases

```
All zeros:   [0, 0, 0]    →  curr never increments  →  best = 0  ✅
All ones:    [1, 1, 1]    →  no reset ever happens  →  best = 3  ✅
Single one:  [0, 1, 0]    →  one streak of length 1 →  best = 1  ✅
Single zero: [0]          →  best stays 0           →  best = 0  ✅
```

---

## Complexity

| | |
|---|---|
| **Time** | O(n) — single pass through the array |
| **Space** | O(1) — only `curr` and `best` variables used |

---

## Key Takeaway

> Track the current streak length and reset it to `0` whenever a `0` is seen.  
> Update the global max on every `1` — not just on resets — to handle trailing streaks with no closing `0`.
