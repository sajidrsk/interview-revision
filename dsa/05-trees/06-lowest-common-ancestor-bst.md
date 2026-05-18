# DSA: Lowest Common Ancestor of BST (Tree Walk)

Return LCA of nodes `p` and `q` in a BST.

**LeetCode:** [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

---

## Why it matters for FE

Shared ancestor in folder/tree navigation.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Store paths from root | O(h) | O(h) | Works |
| **BST walk** | **O(h)** | **O(1)** | **Default** |

---

## Algorithm

1. Start at `root`.
2. If both `p.val` and `q.val` are less than `root.val` → go left.
3. If both greater → go right.
4. Else `root` is the split point → return `root`.

---

## JavaScript solution

```javascript
function lowestCommonAncestor(root, p, q) {
  let current = root;

  while (current) {
    if (p.val < current.val && q.val < current.val) {
      current = current.left;
    } else if (p.val > current.val && q.val > current.val) {
      current = current.right;
    } else {
      return current;
    }
  }

  return null;
}
```

---

## Big-O analysis

- **Time complexity:** O(h)
- **Space complexity:** O(1) iterative

---

## Gotchas

- General binary tree LCA needs different postorder logic.

---

## Quick Revise Summary

- Both left → left; both right → right; else current is LCA.
