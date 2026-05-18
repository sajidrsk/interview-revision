# DSA: Binary Tree Level Order Traversal (BFS)

Return values level by level: `[[3],[9,20],[15,7]]`.

**LeetCode:** [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

---

## Why it matters for FE

BFS by level = rendering UI row-by-row, lazy-loading tree depth.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| DFS with depth index | O(n) | O(h) | Easy to mis-order |
| **BFS + frozen level size** | **O(n)** | **O(w)** | **Default** |

---

## Algorithm

1. If no root, return `[]`.
2. Queue = `[root]`, `result = []`.
3. While queue not empty:
   - `levelSize = queue.length`, `currentLevel = []`.
   - Repeat `levelSize` times: dequeue node, push `val`, enqueue children.
   - Push `currentLevel` to `result`.
4. Return `result`.

---

## JavaScript solution

```javascript
function levelOrder(root) {
  if (!root) return [];

  const result = [];
  const queue = [root];

  while (queue.length) {
    const levelSize = queue.length;
    const currentLevel = [];

    for (let i = 0; i < levelSize; i++) {
      const node = queue.shift();
      currentLevel.push(node.val);
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    result.push(currentLevel);
  }

  return result;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(w) — max width

---

## Gotchas

- Freeze `levelSize` before inner loop.

---

## Quick Revise Summary

- BFS; process exactly `queue.length` nodes per level.
