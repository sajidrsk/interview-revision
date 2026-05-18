# DSA: Maximum Depth of Binary Tree (DFS)

Return maximum depth (nodes on longest root-to-leaf path).

**LeetCode:** [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

See also: [file explorer](../../react/01-file-explorer.md) (tree nesting).

---

## Why it matters for FE

Depth = nesting levels in menus, folders, comment threads.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **DFS recursion** | **O(n)** | **O(h)** | **Default** |
| BFS level count | O(n) | O(w) | When levels needed |

---

## Algorithm

1. If `root` is null, return `0`.
2. Return `1 + max(depth(left), depth(right))`.

---

## JavaScript solution

```javascript
function maxDepth(root) {
  if (!root) return 0;
  return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(h) — recursion stack height

---

## Gotchas

- Empty tree → `0`.
- Skewed tree → O(n) stack depth.

---

## Quick Revise Summary

- `null → 0`; else `1 + max(left, right)`.
