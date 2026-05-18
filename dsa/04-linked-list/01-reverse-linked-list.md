# DSA: Reverse Linked List (Pointers)

Reverse a singly linked list; return new head.

**LeetCode:** [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

---

## Why it matters for FE

Base pattern for in-place list transforms.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **Iterative three pointers** | **O(n)** | **O(1)** | **Default** |
| Recursive | O(n) | O(n) stack | If asked |

---

## Algorithm

1. `previous = null`, `current = head`.
2. While `current`:
   - Save `next = current.next`.
   - `current.next = previous`.
   - Move `previous = current`, `current = next`.
3. Return `previous` (new head).

---

## JavaScript solution

```javascript
function reverseList(head) {
  let previous = null;
  let current = head;

  while (current) {
    const next = current.next;
    current.next = previous;
    previous = current;
    current = next;
  }

  return previous;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(1)

---

## Gotchas

- Save `next` before flipping `current.next`.

---

## Quick Revise Summary

- `prev / curr / next` walk; return `prev`.
