# DSA: Valid Anagram (Hash Map / Frequency)

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s` (same letters, same counts).

**Example:** `s = "anagram"`, `t = "nagaram"` → `true`.

**LeetCode:** [Valid Anagram](https://leetcode.com/problems/valid-anagram/)

---

## Why it matters for FE

Frequency maps power search normalization, grouping user input, and comparing tokens.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Sort both strings | O(n log n) | O(n) | Simple but slower |
| **Map frequency** | **O(n)** | **O(k)** | **Default** |
| 26-count array (a–z only) | O(n) | O(1) | Fixed alphabet |

---

## Algorithm

1. If `s.length !== t.length`, return `false`.
2. Build a `Map` of character counts from `s` (+1 per char).
3. For each character in `t`, decrement its count in the map.
4. If a char is missing or count goes below zero, return `false`.
5. If all decrements succeed, return `true`.

---

## JavaScript solution

```javascript
function isAnagram(s, t) {
  if (s.length !== t.length) return false;

  const charCount = new Map();

  for (const ch of s) {
    charCount.set(ch, (charCount.get(ch) || 0) + 1);
  }

  for (const ch of t) {
    if (!charCount.has(ch)) return false;
    const remaining = charCount.get(ch) - 1;
    if (remaining < 0) return false;
    charCount.set(ch, remaining);
  }

  return true;
}
```

---

## Big-O analysis

- **Time complexity:** O(n) — n = string length; two passes.
- **Space complexity:** O(k) — k = number of unique characters.

---

## Gotchas

- Compare lengths first for quick reject.
- Unicode: use `Map`, not a fixed 26-slot array.

---

## Quick Revise Summary

- Same length + frequency map: +1 on `s`, -1 on `t`.
