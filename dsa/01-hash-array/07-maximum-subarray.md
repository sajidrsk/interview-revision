# DSA: Maximum Subarray (Kadane)

Given an integer array `nums`, find the **contiguous** subarray with the largest **sum** and return that sum.

**Example:** `[-2, 1, -3, 4, -1, 2, 1, -5, 4]` → `6` (subarray `[4, -1, 2, 1]`).

**LeetCode:** [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

> **Not the same as Maximum Product Subarray** — that problem needs `minSoFar` / `maxSoFar` because negatives flip signs. This problem is **sum** only (Kadane).

---

## Why it matters for FE

Kadane is the “running best” pattern — useful for metrics over time windows, scroll performance aggregates, etc.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Brute force all subarrays | O(n²) or O(n³) | O(1) | Skip |
| Prefix + min prefix | O(n) | O(n) | Works, heavier |
| **Kadane (one pass)** | **O(n)** | **O(1)** | **Memorize** |

---

## Algorithm

1. If `numbers` is empty, return `0` (or handle per problem constraints).
2. Initialize `maxEndingHere` and `maxSoFar` to the first element.
3. Iterate from the second element to the end.
4. For each `current`, set `maxEndingHere` to the larger of:
   - starting a new subarray at `current`, or
   - extending the previous subarray: `maxEndingHere + current`.
5. Update `maxSoFar` if `maxEndingHere` is greater.
6. Return `maxSoFar`.

---

## JavaScript solution

```javascript
function maxSubArray(numbers) {
  // Edge case: empty array
  if (numbers.length === 0) return 0;

  // Best sum ending at current index; best sum seen anywhere
  let maxEndingHere = numbers[0];
  let maxSoFar = numbers[0];

  for (let i = 1; i < numbers.length; i++) {
    const current = numbers[i];

    // Extend previous subarray or start fresh at current
    maxEndingHere = Math.max(current, maxEndingHere + current);

    // Track global maximum
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
  }

  return maxSoFar;
}
```

---

## Big-O analysis

- **Time complexity:** O(n) — each element is processed once.
- **Space complexity:** O(1) — only a few variables; no extra data structures.

---

## Gotchas

- All negative? Kadane still works — `maxSoFar` becomes the largest single element.
- Follow-up “return indices” — track `start` / `end` when you reset `maxEndingHere` to `current`.
- **Maximum Product Subarray** uses `minSoFar` and `maxSoFar` with `tempMax` — different problem.

---

## Quick Revise Summary

- `maxEndingHere = max(current, maxEndingHere + current)` each step.
- `maxSoFar` = best subarray sum anywhere so far.
