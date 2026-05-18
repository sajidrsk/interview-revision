# DSA: Binary Search (Classic)

Find `target` index in sorted array, or `-1`.

**LeetCode:** [Binary Search](https://leetcode.com/problems/binary-search/)

---

## Why it matters for FE

Fast lookup in sorted tables, version lists, indexes.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Linear | O(n) | O(1) | Skip on sorted data |
| **Binary search** | **O(log n)** | **O(1)** | **Default** |

---

## Algorithm

1. `left = 0`, `right = numbers.length - 1`.
2. While `left <= right`:
   - `mid = left + floor((right - left) / 2)`.
   - If `numbers[mid] === target`, return `mid`.
   - If `numbers[mid] < target`, `left = mid + 1`.
   - Else `right = mid - 1`.
3. Return `-1`.

---

## JavaScript solution

```javascript
function search(numbers, target) {
  let left = 0;
  let right = numbers.length - 1;

  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);

    if (numbers[mid] === target) {
      return mid;
    }
    if (numbers[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
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

- Inclusive bounds: `left <= right`.
- `mid = left + (right - left) / 2` avoids overflow in other languages.

---

## Quick Revise Summary

- Halve search space each step on sorted array.
