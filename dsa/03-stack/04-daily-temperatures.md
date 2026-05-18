# DSA: Daily Temperatures (Monotonic Stack)

`answer[i]` = days until a warmer temperature after day `i` (0 if none).

**LeetCode:** [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

---

## Why it matters for FE

Monotonic stack = “next greater element” / next event after timestamp.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Brute | O(n²) | O(1) | Skip |
| **Monotonic stack (indices)** | **O(n)** | **O(n)** | **Default** |

---

## Algorithm

1. `answer` filled with 0; `stack` holds indices (decreasing temperatures).
2. For each day `i`:
   - While stack not empty and `temperatures[i] > temperatures[stack.top]`:
     - Pop index `j`, set `answer[j] = i - j`.
   - Push `i` onto stack.
3. Return `answer`.

---

## JavaScript solution

```javascript
function dailyTemperatures(temperatures) {
  const answer = new Array(temperatures.length).fill(0);
  const indexStack = []; // indices with decreasing temps

  for (let i = 0; i < temperatures.length; i++) {
    while (
      indexStack.length &&
      temperatures[i] > temperatures[indexStack[indexStack.length - 1]]
    ) {
      const previousDay = indexStack.pop();
      answer[previousDay] = i - previousDay;
    }
    indexStack.push(i);
  }

  return answer;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(n)

---

## Gotchas

- Stack stores **indices**, not temperatures.

---

## Quick Revise Summary

- Decreasing index stack; warmer day resolves waiting indices.
