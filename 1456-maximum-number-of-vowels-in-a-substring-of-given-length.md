# 1456. Maximum Number of Vowels in a Substring of Given Length

| | |
|---|---|
| **Difficulty** | Medium |
| **Topic** | Sliding Window, String |
| **LeetCode Link** | [Problem](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/) |
| **Study Plan** | LeetCode 75 |
| **Time Complexity** | O(n) |
| **Space Complexity** | O(1) |

---

## Problem

Given a string `s` and an integer `k`, return the **maximum number of vowel letters** in any substring of `s` with length `k`.

Vowel letters in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Examples:**
```
Input:  s = "abciiidef",  k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.

Input:  s = "leetcode",   k = 3
Output: 2
Explanation: "lee", "eet" and "ode" each contain 2 vowels.
```

---

## Approach — Fixed-Size Sliding Window

**Intuition:**  
Count vowels in the first `k` characters. Then slide the window one step at a time — **add 1 if the incoming character is a vowel**, **subtract 1 if the outgoing character was a vowel**.

- No need to recount `k` characters every step
- Just one lookup + one arithmetic operation per slide → O(n)

```
s = "abciiidef",  k = 3

[a b c] i i i d e f  →  curr = 1  (only 'a')
 a[b c i] i i d e f  →  curr = 0  (drop 'a' -1, add 'i' +1 → wait: drop vowel, add vowel → curr=1... recalc)

Let's trace carefully:

Step 0  →  window = "abc"  →  curr = 1   ('a')        best = 1
Step 1  →  drop 'a'(-1), add 'i'(+1)  →  curr = 1    best = 1
Step 2  →  drop 'b'( 0), add 'i'(+1)  →  curr = 2    best = 2
Step 3  →  drop 'c'( 0), add 'i'(+1)  →  curr = 3  ✅ best = 3
Step 4  →  drop 'i'(-1), add 'd'( 0)  →  curr = 2    best = 3
Step 5  →  drop 'i'(-1), add 'e'(+1)  →  curr = 2    best = 3
Step 6  →  drop 'i'(-1), add 'f'( 0)  →  curr = 1    best = 3
```

---

## Solution

```python
class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        vowels = set('aeiou')

        # Build the first window
        curr = sum(c in vowels for c in s[:k])
        best = curr

        # Slide the window from index k to end
        for i in range(k, len(s)):
            curr += (s[i] in vowels) - (s[i - k] in vowels)  # add right, drop left
            if curr > best:
                best = curr
            if best == k:        # full window is all vowels — can't do better
                return best

        return best
```

---

## Step-by-Step Breakdown

| Expression | What it does |
|---|---|
| `s[i] in vowels` | `True`/`False` → evaluates to `1`/`0` in arithmetic |
| `s[i - k] in vowels` | Same check for the character exiting the window |
| `curr += (1 or 0) - (1 or 0)` | Net change without rescanning `k` chars |
| `best == k` | Early exit — a window full of vowels is the theoretical ceiling |

---

## Naive vs Sliding Window

```
Naive approach  →  O(n * k)
  For every starting position, count vowels in k chars from scratch.

  s = "abciiidef", k = 3
  "abc" → count 3 chars → 1
  "bci" → count 3 chars → 1
  "cii" → count 3 chars → 2
  ... recount every time — wasteful!

Sliding Window  →  O(n)
  Reuse previous count. Only care about the one char coming in
  and the one char going out.
```

---

## Complexity

| | |
|---|---|
| **Time** | O(n) — one pass after building the initial window |
| **Space** | O(1) — vowel set is fixed size (5 characters), no extra storage |

---

## Key Takeaway

> Fixed-size Sliding Window avoids recounting from scratch at every position.  
> Track only the **delta** — what enters on the right, what leaves on the left.  
> Boolean-to-int coercion (`True`/`False` → `1`/`0`) makes the update a clean one-liner.
