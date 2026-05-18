# DSA: Two Sum (Hash Map)

Given an array of integers `nums` and an integer `target`, return the **indices** of two numbers that add up to `target`. Exactly one solution exists; you may not use the same element twice.

**Example:** `nums = [2, 7, 11, 15]`, `target = 9` → `[0, 1]`.

**LeetCode:** [Two Sum](https://leetcode.com/problems/two-sum/)

---

## Why it matters for FE

The “store what you’ve seen, look up the complement” pattern shows up everywhere: pairing IDs, matching tags, deduping keys in UI state.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Brute force (nested loops) | O(n²) | O(1) | Skip in interviews |
| **Hash map (one pass)** | **O(n)** | **O(n)** | **Default** |
| Sort + two pointers | O(n log n) | O(1) or O(n) | When array is already sorted |

---

## Algorithm

1. Create an empty `Map`: `value → index`.
2. Loop `i` from `0` to `numbers.length - 1`.
3. Compute `complement = target - numbers[i]`.
4. If the map already has `complement`, return `[map.get(complement), i]`.
5. Otherwise store `numbers[i]` → `i` in the map.
6. If no pair is found, return `[]` (problem usually guarantees one solution).

---

## JavaScript solution

```javascript
function twoSum(numbers, target) {
  // Map: value we've seen -> its index
  const valueToIndex = new Map();

  for (let i = 0; i < numbers.length; i++) {
    const current = numbers[i];
    const complement = target - current;

    // Found the pair before inserting current (avoids same index)
    if (valueToIndex.has(complement)) {
      return [valueToIndex.get(complement), i];
    }

    valueToIndex.set(current, i);
  }

  return [];
}
```

---

## Big-O analysis

- **Time complexity:** O(n) — single pass through the array.
- **Space complexity:** O(n) — map stores up to n entries.

---

## Gotchas

- Check for `complement` **before** inserting the current value (same-index trap).
- Follow-up “all pairs” or “sorted indices” — same map idea, different return shape.

---

## Quick Revise Summary

- One pass + `Map` of `value → index`.
- `complement = target - current`; lookup then insert.
