# 1679. Max Number of K-Sum Pairs

| | |
|---|---|
| **Difficulty** | Medium |
| **Topic** | Two Pointers, Sorting |
| **LeetCode Link** | [Problem](https://leetcode.com/problems/max-number-of-k-sum-pairs/) |
| **Study Plan** | LeetCode 75 |
| **Time Complexity** | O(n log n) |
| **Space Complexity** | O(1) |

---

## Problem

Given an integer array `nums` and an integer `k`, find the **maximum number of operations** where you pick two numbers that sum to `k` and remove them from the array.

**Example:**
```
Input:  nums = [1, 2, 3, 4],  k = 5
Output: 2

Pairs removed: (1,4) and (2,3) → 2 operations
```

---

## Approach — Two Pointers

**Intuition:**  
Sort the array first. Then use two pointers — one from the left (smallest) and one from the right (largest).

- Sum == k → pair found, move both pointers inward
- Sum < k  → need a bigger number, move left pointer right
- Sum > k  → need a smaller number, move right pointer left

This works because the array is sorted — we know exactly which direction to move to increase or decrease the sum.

```
Sorted: [1, 2, 3, 4]   k = 5
         L           R   → 1+4=5 ✅  count=1
            L     R      → 2+3=5 ✅  count=2
              L≮R        → stop
```

---

## Solution

```python
from typing import List

class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        nums.sort()                          # sort the array
        left, right = 0, len(nums) - 1      # two pointers from both ends
        count = 0

        while left < right:
            s = nums[left] + nums[right]
            if s == k:
                count += 1
                left += 1
                right -= 1
            elif s < k:
                left += 1                   # need bigger value
            else:
                right -= 1                  # need smaller value

        return count
```

---

## Complexity

| | |
|---|---|
| **Time** | O(n log n) — dominated by sort |
| **Space** | O(1) — no extra array used |

---

## Key Takeaway

> Sort + Two Pointers is a classic pattern for pair-sum problems.  
> Every element is visited at most once after sorting → linear scan = O(n).
