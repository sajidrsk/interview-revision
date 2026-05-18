# DSA: Two Sum II (Two Pointers — Sorted)

Given **1-indexed** sorted array `numbers` and `target`, return indices of two numbers that sum to `target`. O(1) extra space.

**LeetCode:** [Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

---

## Why it matters for FE

Sorted data + two pointers avoids a hash map when order is guaranteed.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Hash map | O(n) | O(n) | Ignores sorted property |
| **Two pointers** | **O(n)** | **O(1)** | **Default** |

---

## Algorithm

1. `left = 0`, `right = numbers.length - 1`.
2. While `left < right`:
   - `sum = numbers[left] + numbers[right]`.
   - If `sum === target`, return `[left + 1, right + 1]`.
   - If `sum < target`, `left++`; else `right--`.
3. Return `[]` if not found.

---

## JavaScript solution

```javascript
function twoSum(numbers, target) {
  let left = 0;
  let right = numbers.length - 1;

  while (left < right) {
    const sum = numbers[left] + numbers[right];

    if (sum === target) {
      return [left + 1, right + 1]; // 1-indexed
    }
    if (sum < target) {
      left++;
    } else {
      right--;
    }
  }

  return [];
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(1)

---

## Gotchas

- Return **1-based** indices.
- Template extends to 3Sum (fix one index, two-pointer the rest).

---

## Quick Revise Summary

- Sorted → move `left` or `right` by comparing sum to target.
