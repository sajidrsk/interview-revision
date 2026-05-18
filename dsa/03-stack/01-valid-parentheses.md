# DSA: Valid Parentheses (Stack)

Return `true` if bracket string is valid and correctly nested.

**LeetCode:** [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

---

## Why it matters for FE

Stacks model nesting: JSX, menus, parsers, undo.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **Stack** | **O(n)** | **O(n)** | **Only approach** |

---

## Algorithm

1. Map each closing bracket to its opening partner.
2. For each character:
   - If opening → push onto stack.
   - If closing → pop; if pop doesn’t match, return `false`.
3. Return `stack.length === 0`.

---

## JavaScript solution

```javascript
function isValid(s) {
  const closingToOpening = { ")": "(", "]": "[", "}": "{" };
  const stack = [];

  for (const ch of s) {
    if (ch === "(" || ch === "[" || ch === "{") {
      stack.push(ch);
    } else {
      if (stack.pop() !== closingToOpening[ch]) {
        return false;
      }
    }
  }

  return stack.length === 0;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(n)

---

## Gotchas

- Pop on close catches empty stack (`undefined !== '('`).

---

## Quick Revise Summary

- Open push; close pop + match; empty stack at end.
