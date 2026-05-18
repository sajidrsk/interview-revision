# DSA: Evaluate Reverse Polish Notation (Stack)

Evaluate postfix expression with `+`, `-`, `*`, `/` (integer division toward zero).

**LeetCode:** [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

---

## Why it matters for FE

Stack evaluation for calculators and expression parsers.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **Stack** | **O(n)** | **O(n)** | **Default** |

---

## Algorithm

1. For each token:
   - If number → push `Number(token)`.
   - If operator → pop `right`, pop `left`, push `apply(left, right)`.
2. Return sole stack element.

---

## JavaScript solution

```javascript
function evalRPN(tokens) {
  const stack = [];
  const operators = {
    "+": (a, b) => a + b,
    "-": (a, b) => a - b,
    "*": (a, b) => a * b,
    "/": (a, b) => Math.trunc(a / b),
  };

  for (const token of tokens) {
    if (token in operators) {
      const right = stack.pop();
      const left = stack.pop();
      stack.push(operators[token](left, right));
    } else {
      stack.push(Number(token));
    }
  }

  return stack[0];
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(n)

---

## Gotchas

- Second pop = right operand; use `Math.trunc` for `/`.

---

## Quick Revise Summary

- Number push; operator pop two, push result.
