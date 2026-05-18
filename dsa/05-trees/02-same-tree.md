# DSA: Same Tree (DFS)

Return `true` if two binary trees are identical.

**LeetCode:** [Same Tree](https://leetcode.com/problems/same-tree/)

---

## Why it matters for FE

Structural equality for nested UI / JSON trees.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **DFS compare** | **O(n)** | **O(h)** | **Default** |

---

## Algorithm

1. If both null → `true`.
2. If one null or values differ → `false`.
3. Return `same(left)` AND `same(right)`.

---

## JavaScript solution

```javascript
function isSameTree(p, q) {
  if (!p && !q) return true;
  if (!p || !q) return false;
  if (p.val !== q.val) return false;
  return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(h)

---

## Gotchas

- Check nulls before `.val`.

---

## Quick Revise Summary

- Mirror DFS on both trees.
