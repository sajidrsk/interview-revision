# DSA: Linked List Cycle (Fast / Slow)

Return `true` if linked list has a cycle.

**LeetCode:** [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

---

## Why it matters for FE

Floyd’s algorithm — circular references, infinite loop detection.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Hash set of nodes | O(n) | O(n) | Easy |
| **Floyd slow/fast** | **O(n)** | **O(1)** | **Default** |

---

## Algorithm

1. `slow = head`, `fast = head`.
2. While `fast` and `fast.next` exist:
   - `slow = slow.next`, `fast = fast.next.next`.
   - If `slow === fast`, return `true`.
3. Return `false`.

**Find cycle start (follow-up):** after meet, reset one pointer to head; move both one step until they meet.

---

## JavaScript solution

```javascript
function hasCycle(head) {
  let slow = head;
  let fast = head;

  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true;
  }

  return false;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(1)

---

## Gotchas

- Guard `fast && fast.next` before advancing.

---

## Quick Revise Summary

- Tortoise + hare; meet → cycle.
