# 1470. Shuffle the Array

| | |
|---|---|
| **Difficulty** | Easy |
| **Topic** | Array |
| **LeetCode Link** | [Problem](https://leetcode.com/problems/shuffle-the-array/) |
| **Study Plan** | LeetCode 75 |
| **Time Complexity** | O(n) |
| **Space Complexity** | O(n) |

---

## Problem

Given an array `nums` of `2n` elements in the form `[x1, x2, ..., xn, y1, y2, ..., yn]`,  
return the array interleaved as `[x1, y1, x2, y2, ..., xn, yn]`.

**Example:**
```
Input:  nums = [2, 5, 1, 3, 4, 7],  n = 3
Output: [2, 3, 5, 4, 1, 7]

x1=2, x2=5, x3=1  →  first half:  nums[0..n-1]
y1=3, y2=4, y3=7  →  second half: nums[n..2n-1]

Interleaved: [x1,y1, x2,y2, x3,y3] = [2,3, 5,4, 1,7]
```

---

## Approach — Two-Pointer Interleave

**Intuition:**  
The array is already split into two halves. Use index `i` to walk both halves simultaneously — pick `nums[i]` from the left half and `nums[i + n]` from the right half, appending them alternately.

```
nums = [2, 5, 1, | 3, 4, 7]   n = 3
        ─────────  ─────────
        x-half      y-half
        i=0,1,2     i+n=3,4,5

i=0 → nums[0]=2,  nums[3]=3  →  append 2, 3
i=1 → nums[1]=5,  nums[4]=4  →  append 5, 4
i=2 → nums[2]=1,  nums[5]=7  →  append 1, 7
                               ─────────────────
Result →                       [2, 3, 5, 4, 1, 7] ✅
```

---

## Solution

```python
from typing import List

class Solution:
    def shuffle(self, nums: List[int], n: int) -> List[int]:
        ans = []
        for i in range(n):
            ans.append(nums[i])        # xi  ← from first half
            ans.append(nums[i + n])    # yi  ← from second half (offset by n)
        return ans
```

**Alternative — zip one-liner (same logic, more Pythonic):**

```python
from typing import List

class Solution:
    def shuffle(self, nums: List[int], n: int) -> List[int]:
        return [val for pair in zip(nums[:n], nums[n:]) for val in pair]
```

---

## zip approach breakdown

```
nums[:n]  = [2, 5, 1]
nums[n:]  = [3, 4, 7]

zip(...)  = [(2,3), (5,4), (1,7)]    ← pairs each xi with yi

for pair in zip → for val in pair:
    (2,3) → 2, 3
    (5,4) → 5, 4
    (1,7) → 1, 7

Final list → [2, 3, 5, 4, 1, 7]  ✅
```

---

## Loop vs zip — when to use which

| Approach | Readability | Explicitness | Best for |
|---|---|---|---|
| `for i in range(n)` | Clear, beginner-friendly | Index arithmetic visible | Interviews — easy to explain |
| `zip` one-liner | Concise, Pythonic | Hides the offset logic | Clean production code |

Both produce identical output and have the same complexity.

---

## Complexity

| | |
|---|---|
| **Time** | O(n) — iterate through `n` pairs exactly once |
| **Space** | O(n) — output array of size `2n` (not counting input) |

---

## Key Takeaway

> Split the array mentally into two halves at index `n`.  
> Walk both halves with a single index `i` — left half at `nums[i]`, right half at `nums[i + n]`.  
> `zip(nums[:n], nums[n:])` is the Pythonic shortcut that makes the pairing implicit.
