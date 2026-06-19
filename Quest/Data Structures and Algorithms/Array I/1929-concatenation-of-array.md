# 1929. Concatenation of Array

| | |
|---|---|
| **Difficulty** | Easy |
| **Topic** | Array |
| **LeetCode Link** | [Problem](https://leetcode.com/problems/concatenation-of-array/) |
| **Study Plan** | LeetCode 75 |
| **Time Complexity** | O(n) |
| **Space Complexity** | O(n) |

---

## Problem

Given an integer array `nums` of length `n`, return an array `ans` of length `2n` which is the **concatenation of two copies** of `nums`.

Formally: `ans[i] == nums[i]` and `ans[i + n] == nums[i]` for `0 <= i < n`.

**Example:**
```
Input:  nums = [1, 2, 1]
Output: [1, 2, 1, 1, 2, 1]

ans[0] = nums[0] = 1
ans[1] = nums[1] = 2
ans[2] = nums[2] = 1
ans[3] = nums[0] = 1   ← second copy starts at index n
ans[4] = nums[1] = 2
ans[5] = nums[2] = 1
```

---

## Approach — Array Concatenation

**Intuition:**  
The result is just `nums` written twice back-to-back. No reordering, no filtering — straightforward copy.

Two ways to think about it:

```
Option 1 — Python operator:
  nums + nums  →  creates a new list with both copies joined

Option 2 — Manual build:
  Loop i from 0 to n:
      ans[i]     = nums[i]
      ans[i + n] = nums[i]
```

Both are O(n). Python's `+` operator on lists does exactly this internally.

```
nums = [1, 3, 2, 1]    n = 4

First half  →  ans[0..3] = [1, 3, 2, 1]
Second half →  ans[4..7] = [1, 3, 2, 1]
                           ─────────────
Result      →             [1, 3, 2, 1, 1, 3, 2, 1]  ✅
```

---

## Solution

```python
from typing import List

class Solution:
    def getConcatenation(self, nums: List[int]) -> List[int]:
        return nums + nums          # Python list + creates a new concatenated list
```

**Alternative — explicit loop (same logic, more verbose):**

```python
from typing import List

class Solution:
    def getConcatenation(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [0] * (2 * n)

        for i in range(n):
            ans[i]     = nums[i]    # first copy
            ans[i + n] = nums[i]    # second copy at offset n

        return ans
```

---

## Why `nums + nums` works in Python

| Expression | What happens under the hood |
|---|---|
| `nums + nums` | Allocates a new list of size `2n`, copies elements from both |
| Does NOT modify `nums` | `+` on lists always returns a **new** list |
| Same as `nums * 2` | `nums * 2` is also valid — repeats the list twice |

---

## Complexity

| | |
|---|---|
| **Time** | O(n) — every element copied exactly twice |
| **Space** | O(n) — output array of size 2n (not counting input) |

---

## Key Takeaway

> When the problem asks to repeat or mirror an array, Python's `nums + nums` or `nums * 2`  
> handles it cleanly in one line — no manual indexing needed.  
> The output size doubling means O(n) space is unavoidable regardless of approach.
