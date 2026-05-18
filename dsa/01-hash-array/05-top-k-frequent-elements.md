# DSA: Top K Frequent Elements (Hash Map + Bucket)

Given integer array `nums` and integer `k`, return the `k` most frequent elements (any order).

**Example:** `nums = [1,1,1,2,2,3]`, `k = 2` → `[1, 2]`.

**LeetCode:** [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

---

## Why it matters for FE

“Top N” lists (trending tags, popular items) start with counting, then picking top k.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Map + sort by frequency | O(n log n) | O(n) | Simple fallback |
| **Bucket sort by frequency** | **O(n)** | **O(n)** | **Default** |
| Min-heap of size k | O(n log k) | O(n) | Huge n, small k |

---

## Algorithm

1. Count frequency of each number with a `Map`.
2. Create `bucket` array of length `numbers.length + 1`; `bucket[f]` = numbers with frequency `f`.
3. Place each `(num, count)` into `bucket[count]`.
4. Walk `bucket` from highest index downward, collect values until `k` elements.
5. Return the collected array.

---

## JavaScript solution

```javascript
function topKFrequent(numbers, k) {
  const frequency = new Map();
  for (const num of numbers) {
    frequency.set(num, (frequency.get(num) || 0) + 1);
  }

  const bucket = Array.from({ length: numbers.length + 1 }, () => []);

  for (const [num, count] of frequency) {
    bucket[count].push(num);
  }

  const result = [];
  for (let freq = bucket.length - 1; freq >= 0 && result.length < k; freq--) {
    for (const num of bucket[freq]) {
      result.push(num);
      if (result.length === k) return result;
    }
  }

  return result;
}
```

---

## Big-O analysis

- **Time complexity:** O(n) — counting + bucket fill + scan buckets.
- **Space complexity:** O(n) — map and buckets.

---

## Gotchas

- Max frequency ≤ `numbers.length`, so bucket size is safe.
- Sort-by-frequency one-liner is fine if interviewer accepts O(n log n).

---

## Quick Revise Summary

- Count → bucket by freq → scan buckets from end for top k.
