# DSA: Validate Binary Search Tree (DFS Bounds)

Return `true` if tree is a valid BST.

**LeetCode:** [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

---

## Why it matters for FE

Ordered trees for search UI and valid nested sorted structures.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Inorder → strictly increasing | O(n) | O(n) | Works |
| **DFS min/max bounds** | **O(n)** | **O(h)** | **Default** |
| Parent-only compare | Wrong | — | Fails on skewed violations |

---

## Algorithm

1. `validate(node, min, max)`:
   - Null → `true`.
   - If `node.val <= min` or `node.val >= max` → `false`.
   - Left: `(min, node.val)`; right: `(node.val, max)`.
2. Start with `(-Infinity, Infinity)`.

---

## JavaScript solution

```javascript
function isValidBST(root) {
  function validate(node, min, max) {
    if (!node) return true;
    if (node.val <= min || node.val >= max) return false;
    return (
      validate(node.left, min, node.val) &&
      validate(node.right, node.val, max)
    );
  }
  return validate(root, -Infinity, Infinity);
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(h)

---

## Gotchas

- Global bounds, not just parent check.
- No duplicate values allowed.

---

## Quick Revise Summary

- DFS with `(min, max)` window per node.
