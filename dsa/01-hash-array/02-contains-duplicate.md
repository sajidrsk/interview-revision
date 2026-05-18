# DSA: Contains Duplicate (Set)

Given an integer array `nums`, return `true` if any value appears **at least twice**, else `false`.

**Example:** `[1, 2, 3, 1]` → `true`; `[1, 2, 3, 4]` → `false`.

**LeetCode:** [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

---

## Why it matters for FE

Sets detect duplicate keys, selected IDs, or repeated items before rendering.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Sort + scan neighbors | O(n log n) | O(1) | OK if you must sort anyway |
| **Hash set one pass** | **O(n)** | **O(n)** | **Default** |
| Brute force | O(n²) | O(1) | Skip |

---

## Algorithm

1. Create an empty `Set`.
2. For each `current` in `numbers`:
   - If `seen.has(current)`, return `true`.
   - Add `current` to `seen`.
3. If the loop finishes, return `false`.

---

## JavaScript solution

```javascript
function containsDuplicate(numbers) {
  const seen = new Set();

  for (const current of numbers) {
    if (seen.has(current)) {
      return true;
    }
    seen.add(current);
  }

  return false;
}
```

---

## Big-O analysis

- **Time complexity:** O(n) — each element checked once.
- **Space complexity:** O(n) — set stores up to n values.

---

## Gotchas

- Early return on first duplicate saves time.
- Follow-up “find all duplicates” needs a frequency map, not just a set.

---

## Quick Revise Summary

- `Set` + single pass: seen before → `true`.
