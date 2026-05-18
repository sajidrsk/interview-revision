# DSA: Merge Two Sorted Lists (Linked List)

Merge two sorted linked lists; return merged head.

**LeetCode:** [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

---

## Why it matters for FE

Merge pattern for sorted feeds and merge-sort chunks.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **Dummy head + tail** | **O(n+m)** | **O(1)** | **Default** |

---

## Algorithm

1. Create `dummy` node; `tail` points to dummy.
2. While both lists exist, attach smaller node to `tail`, advance that list.
3. Attach remaining list to `tail.next`.
4. Return `dummy.next`.

---

## JavaScript solution

```javascript
function mergeTwoLists(list1, list2) {
  const dummy = { val: 0, next: null };
  let tail = dummy;

  while (list1 && list2) {
    if (list1.val <= list2.val) {
      tail.next = list1;
      list1 = list1.next;
    } else {
      tail.next = list2;
      list2 = list2.next;
    }
    tail = tail.next;
  }

  tail.next = list1 || list2;
  return dummy.next;
}
```

---

## Big-O analysis

- **Time complexity:** O(n + m)
- **Space complexity:** O(1)

---

## Gotchas

- Dummy avoids special-casing first node.

---

## Quick Revise Summary

- Dummy + compare heads + attach remainder.
