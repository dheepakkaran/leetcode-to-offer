# 643. Maximum Average Subarray I

| | |
|---|---|
| **Difficulty** | Easy |
| **Topic** | Sliding Window, Array |
| **LeetCode Link** | [Problem](https://leetcode.com/problems/maximum-average-subarray-i/) |
| **Study Plan** | LeetCode 75 |
| **Time Complexity** | O(n) |
| **Space Complexity** | O(1) |

---

## Problem

Given an integer array `nums` and an integer `k`, find the **contiguous subarray of length k** with the maximum average value and return it.

**Example:**
```
Input:  nums = [1, 12, -5, -6, 50, 3],  k = 4
Output: 12.75000

Window with max sum: (12 + -5 + -6 + 50) = 51 → 51 / 4 = 12.75
```

---

## Approach — Sliding Window

**Intuition:**  
Compute the sum of the first `k` elements. Then slide the window one step at a time — **add the incoming element** on the right and **drop the outgoing element** on the left.

- No need to re-sum `k` elements every step
- Just one addition + one subtraction per slide → O(n)

```
nums = [1, 12, -5, -6, 50, 3]   k = 4

Initial window:  [1, 12, -5, -6]       sum = 2
Slide right:     [12, -5, -6, 50]      sum = 2 + 50 - 1  = 51  ✅ max
Slide right:     [-5, -6, 50,  3]      sum = 51 + 3 - 12 = 42
                                                              ↑
                                                       max_sum = 51
```

---

## Solution

```python
from typing import List

class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        window_sum = sum(nums[:k])       # sum of first window
        max_sum = window_sum

        for i in range(k, len(nums)):
            window_sum += nums[i] - nums[i - k]   # slide: add right, drop left
            max_sum = max(max_sum, window_sum)

        return max_sum / k
```

---

## Complexity

| | |
|---|---|
| **Time** | O(n) — one pass after the initial window |
| **Space** | O(1) — only two variables used |

---

## Key Takeaway

> Fixed-size Sliding Window avoids recomputing the sum from scratch each step.  
> Add the new element, remove the old one — keeps it linear regardless of k.
