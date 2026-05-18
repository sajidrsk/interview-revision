# DSA: Binary Tree Right Side View (BFS / DFS)

Return node values visible from the right, top to bottom.

**LeetCode:** [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

---

## Why it matters for FE

Last node per BFS level = rightmost in each row.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **BFS — last per level** | **O(n)** | **O(w)** | **Easiest** |
| DFS right-first | O(n) | O(h) | Elegant |

---

## Algorithm (BFS)

1. If no root, return `[]`.
2. BFS with frozen `levelSize` per round.
3. When processing index `i === levelSize - 1`, append `node.val` to `result`.
4. Return `result`.

---

## JavaScript solution

```javascript
function rightSideView(root) {
  if (!root) return [];

  const result = [];
  const queue = [root];

  while (queue.length) {
    const levelSize = queue.length;

    for (let i = 0; i < levelSize; i++) {
      const node = queue.shift();
      if (i === levelSize - 1) {
        result.push(node.val);
      }
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
  }

  return result;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(w)

---

## Gotchas

- Append only on last index of each level.

---

## Quick Revise Summary

- Level BFS; take last value each level.
