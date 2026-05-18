# DSA: Merge Intervals (Sort + Sweep)

Merge all overlapping intervals.

**LeetCode:** [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

---

## Why it matters for FE

Calendar blocks, availability, scheduling UI.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **Sort by start + merge** | **O(n log n)** | **O(n)** | **Default** |
| All pairs | O(n²) | O(1) | Skip |

---

## Algorithm

1. Sort intervals by `start`.
2. `merged = [intervals[0]]`.
3. For each next `[start, end]`:
   - `last = merged[merged.length - 1]`.
   - If `start <= last[1]`, extend `last[1] = max(last[1], end)`.
   - Else push new interval.
4. Return `merged`.

---

## JavaScript solution

```javascript
function merge(intervals) {
  if (!intervals.length) return [];

  intervals.sort((a, b) => a[0] - b[0]);
  const merged = [intervals[0]];

  for (let i = 1; i < intervals.length; i++) {
    const [start, end] = intervals[i];
    const last = merged[merged.length - 1];

    if (start <= last[1]) {
      last[1] = Math.max(last[1], end);
    } else {
      merged.push([start, end]);
    }
  }

  return merged;
}
```

---

## Big-O analysis

- **Time complexity:** O(n log n) — sort dominates
- **Space complexity:** O(n) — output

---

## Gotchas

- Overlap: `start <= last.end` (touching merges).
- Empty input → `[]`.

---

## Quick Revise Summary

- Sort by start; merge into last or push new.
