# DSA: Min Stack (Stack Design)

Stack with `push`, `pop`, `top`, `getMin` in O(1).

**LeetCode:** [Min Stack](https://leetcode.com/problems/min-stack/)

---

## Why it matters for FE

Track running min alongside state (metrics, undo with metadata).

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Scan on getMin | O(1) push, O(n) getMin | O(n) | Skip |
| **Stack of [value, minSoFar]** | **O(1) all** | **O(n)** | **Default** |

---

## Algorithm

1. On `push(val)`: `minSoFar = stack empty ? val : min(val, top.minSoFar)`; push `[val, minSoFar]`.
2. On `pop()`: remove top pair.
3. On `top()`: return top value.
4. On `getMin()`: return top `minSoFar`.

---

## JavaScript solution

```javascript
class MinStack {
  constructor() {
    this.stack = []; // each entry: [value, minSoFar]
  }

  push(val) {
    const minSoFar =
      this.stack.length === 0
        ? val
        : Math.min(val, this.stack[this.stack.length - 1][1]);
    this.stack.push([val, minSoFar]);
  }

  pop() {
    this.stack.pop();
  }

  top() {
    return this.stack[this.stack.length - 1][0];
  }

  getMin() {
    return this.stack[this.stack.length - 1][1];
  }
}
```

---

## Big-O analysis

- **Time complexity:** O(1) per operation
- **Space complexity:** O(n)

---

## Gotchas

- Store min **at push time** so pop restores previous min.

---

## Quick Revise Summary

- Each entry: `[value, minIncludingThis]`.
