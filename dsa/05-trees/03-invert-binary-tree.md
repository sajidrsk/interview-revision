# DSA: Invert Binary Tree (DFS)

Swap every node’s left and right children; return root.

**LeetCode:** [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

---

## Why it matters for FE

Tree transforms = updating nested component trees.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **DFS swap** | **O(n)** | **O(h)** | **Default** |

---

## Algorithm

1. If `root` is null, return null.
2. Swap `root.left` and `root.right`.
3. Recurse on left and right subtrees.
4. Return `root`.

---

## JavaScript solution

```javascript
function invertTree(root) {
  if (!root) return null;

  [root.left, root.right] = [root.right, root.left];
  invertTree(root.left);
  invertTree(root.right);
  return root;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(h)

---

## Gotchas

- In-place mutation is OK unless clone required.

---

## Quick Revise Summary

- Swap children; DFS both subtrees.
