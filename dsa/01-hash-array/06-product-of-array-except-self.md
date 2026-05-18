# DSA: Product of Array Except Self (Prefix / Suffix)

Given integer array `nums`, return `answer` where `answer[i]` is the product of all elements **except** `nums[i]`. No division; O(n) time.

**Example:** `[1, 2, 3, 4]` → `[24, 12, 8, 6]`.

**LeetCode:** [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

---

## Why it matters for FE

Prefix/suffix aggregation appears in charts, running totals, and “everything except current” calculations.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Division | O(n) | O(1) | Forbidden; zeros break |
| Prefix + suffix arrays | O(n) | O(n) | Clear two-pass |
| **Prefix in output + suffix pass** | **O(n)** | **O(1)** extra | **Optimal** |

---

## Algorithm

1. Create `answer` array length `n`, fill with `1`.
2. **Left pass:** `prefixProduct = 1`; for each `i`, set `answer[i] = prefixProduct`, then `prefixProduct *= numbers[i]`.
3. **Right pass:** `suffixProduct = 1`; from `n-1` down to `0`, multiply `answer[i] *= suffixProduct`, then `suffixProduct *= numbers[i]`.
4. Return `answer`.

---

## JavaScript solution

```javascript
function productExceptSelf(numbers) {
  const n = numbers.length;
  const answer = new Array(n).fill(1);

  let prefixProduct = 1;
  for (let i = 0; i < n; i++) {
    answer[i] = prefixProduct;
    prefixProduct *= numbers[i];
  }

  let suffixProduct = 1;
  for (let i = n - 1; i >= 0; i--) {
    answer[i] *= suffixProduct;
    suffixProduct *= numbers[i];
  }

  return answer;
}
```

---

## Big-O analysis

- **Time complexity:** O(n) — two linear passes.
- **Space complexity:** O(1) extra — output array not counted as extra space.

---

## Gotchas

- Zeros handled naturally (many zeros in answer).
- Don’t use division even if allowed — shows the right pattern.

---

## Quick Revise Summary

- Left pass: prefix into `answer[i]`; right pass: multiply suffix.
