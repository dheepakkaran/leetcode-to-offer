# 1004. Max Consecutive Ones III

| | |
|---|---|
| **Difficulty** | Medium |
| **Topic** | Array, Sliding Window |
| **LeetCode Link** | [Problem](https://leetcode.com/problems/max-consecutive-ones-iii/) |
| **Study Plan** | LeetCode 75 |
| **Time Complexity** | O(n) |
| **Space Complexity** | O(1) |

---

## Problem

Given a binary array `nums` and an integer `k`, return the **maximum number of consecutive `1`s** if you can flip **at most `k` zeros** to `1`.

**Example:**
```
Input:  nums = [1,1,1,0,0,0,1,1,1,1,0],  k = 2
Output: 6

After flipping 2 zeros:  [1,1,1,0,0,1,1,1,1,1,1]
                                    ↑           ↑
                               flipped         flipped
Longest streak = 6
```

---

## Approach — Sliding Window (Never Shrink Trick)

**Intuition:**  
We want the longest window `[l, r]` that contains **at most `k` zeros**.  

Standard sliding window shrinks the window when violated — but we don't need every valid window.  
We only want the **maximum** size ever reached.

> Key insight: The window size never needs to decrease.  
> When the zero budget is busted, **slide** (shift both l and r) instead of **shrink** (move only l).  
> The window size stays the same or grows — it never gets smaller.

```
nums = [1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0]   k = 2
        0  1  2  3  4  5  6  7  8  9  10

r=0 : num=1  k=2        l=0  window=[0..0]  size=1
r=1 : num=1  k=2        l=0  window=[0..1]  size=2
r=2 : num=1  k=2        l=0  window=[0..2]  size=3
r=3 : num=0  k=1        l=0  window=[0..3]  size=4
r=4 : num=0  k=0        l=0  window=[0..4]  size=5
r=5 : num=0  k=-1 BUST! nums[0]=1→ no refund, l=1  window=[1..5]  size=5
r=6 : num=1  k=-1 BUST! nums[1]=1→ no refund, l=2  window=[2..6]  size=5
r=7 : num=1  k=-1 BUST! nums[2]=1→ no refund, l=3  window=[3..7]  size=5
r=8 : num=1  k=-1 BUST! nums[3]=0→ refund +1=0, l=4  window=[4..8]  size=5
r=9 : num=1  k=0        l=4  window=[4..9]  size=6  ✅ best!
r=10: num=0  k=-1 BUST! nums[4]=0→ refund +1=0, l=5  window=[5..10] size=6 ✅
```

---

## Solution

```python
from typing import List

class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        l = 0

        for r in range(len(nums)):
            k -= nums[r] == 0        # 0 enters window → spend 1 budget
            if k < 0:                # budget exceeded → slide the window
                k += nums[l] == 0    # 0 exits window  → refund 1 budget
                l += 1               # shift left forward (never shrink!)

        return r - l + 1             # window size at end = max size ever reached
```

---

## Never Shrink vs Standard Shrink — what's the difference?

```
Standard sliding window:
    When zeros > k → move l forward until zeros == k
    → Window can shrink → tracks ALL valid windows
    → Need a separate max tracker

Never Shrink trick:
    When zeros > k → move l exactly ONE step (slide, not shrink)
    → Window size never decreases
    → No need for max tracker — final window IS the answer
    → Saves one variable and one inner loop
```

---

## Why `k -= nums[r] == 0` works

| Expression | Value when `nums[r] == 1` | Value when `nums[r] == 0` |
|---|---|---|
| `nums[r] == 0` | `False` → `0` | `True` → `1` |
| `k -= (expression)` | k unchanged | k decremented by 1 |

Python booleans are integers — `True == 1`, `False == 0`.  
This avoids an explicit `if nums[r] == 0: k -= 1` branch — arithmetic does the job directly.

---

## Edge Cases

```
k = 0  →  [1,0,1,1,0,1]  → no flips allowed → max streak of natural 1s = 2
k = n  →  all zeros allowed to flip → entire array is the answer
All 1s →  k never decrements → window grows to full length
All 0s →  k=2, [0,0,0,0] → flip first 2 → window size = k = 2
```

---

## Complexity

| | |
|---|---|
| **Time** | O(n) — `l` and `r` each move at most `n` steps total |
| **Space** | O(1) — only `l` and in-place `k` used; no extra array or zero counter |

---

## Key Takeaway

> When you only need the **maximum** window size (not all valid windows),  
> the window never needs to shrink — it only slides.  
> Using `k` itself as the live zero-budget counter eliminates a separate variable entirely.  
> Boolean-to-int arithmetic (`nums[r] == 0` → `0` or `1`) keeps the update a clean one-liner.
