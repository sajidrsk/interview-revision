# DSA: Container With Most Water (Two Pointers)

Given `height`, find max area `min(height[i], height[j]) * (j - i)`.

**LeetCode:** [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

---

## Why it matters for FE

Move the pointer at the shorter line — greedy shrink from ends.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| All pairs | O(n²) | O(1) | Skip |
| **Two pointers** | **O(n)** | **O(1)** | **Default** |

---

## Algorithm

1. `left = 0`, `right = n - 1`, `maxArea = 0`.
2. While `left < right`:
   - `width = right - left`, `height = min(numbers[left], numbers[right])`.
   - `maxArea = max(maxArea, width * height)`.
   - Move the pointer at the **shorter** line inward.
3. Return `maxArea`.

---

## JavaScript solution

```javascript
function maxArea(numbers) {
  let left = 0;
  let right = numbers.length - 1;
  let maxArea = 0;

  while (left < right) {
    const width = right - left;
    const height = Math.min(numbers[left], numbers[right]);
    maxArea = Math.max(maxArea, width * height);

    if (numbers[left] < numbers[right]) {
      left++;
    } else {
      right--;
    }
  }

  return maxArea;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(1)

---

## Gotchas

- Only moving the shorter side can improve `min(height)`.

---

## Quick Revise Summary

- Wide window from ends; move shorter pointer.
