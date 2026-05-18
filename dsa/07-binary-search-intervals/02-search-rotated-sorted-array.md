# DSA: Search in Rotated Sorted Array (Binary Search)

Find `target` in rotated sorted array (no duplicates), O(log n).

**LeetCode:** [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

---

## Why it matters for FE

Circular sorted buffers, rotated timelines — modified binary search.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Find pivot + two BS | O(log n) | O(1) | Two-step |
| **One BS — find sorted half** | **O(log n)** | **O(1)** | **Default** |

---

## Algorithm

1. `left = 0`, `right = n - 1`.
2. While `left <= right`:
   - `mid = left + floor((right-left)/2)`.
   - If `numbers[mid] === target`, return `mid`.
   - If left half sorted (`numbers[left] <= numbers[mid]`):
     - If `target` in `[numbers[left], numbers[mid])` → `right = mid - 1`; else `left = mid + 1`.
   - Else right half sorted:
     - If `target` in `(numbers[mid], numbers[right]]` → `left = mid + 1`; else `right = mid - 1`.
3. Return `-1`.

---

## JavaScript solution

```javascript
function search(numbers, target) {
  let left = 0;
  let right = numbers.length - 1;

  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);

    if (numbers[mid] === target) return mid;

    if (numbers[left] <= numbers[mid]) {
      if (target >= numbers[left] && target < numbers[mid]) {
        right = mid - 1;
      } else {
        left = mid + 1;
      }
    } else {
      if (target > numbers[mid] && target <= numbers[right]) {
        left = mid + 1;
      } else {
        right = mid - 1;
      }
    }
  }

  return -1;
}
```

---

## Big-O analysis

- **Time complexity:** O(log n)
- **Space complexity:** O(1)

---

## Gotchas

- With duplicates: `left++` when `numbers[left] === numbers[mid]`.

---

## Quick Revise Summary

- BS; identify sorted half; check if target lies there.
