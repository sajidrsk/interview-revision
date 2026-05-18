# DSA: Clone Graph (DFS + Hash Map)

Deep copy an undirected graph (`val` + `neighbors`).

**LeetCode:** [Clone Graph](https://leetcode.com/problems/clone-graph/)

---

## Why it matters for FE

`Map<old, new>` for deep-cloning trees, state snapshots, relations.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **DFS + map** | **O(V+E)** | **O(V)** | **Default** |
| BFS + map | O(V+E) | O(V) | Same |

---

## Algorithm

1. If `node` is null, return null.
2. `Map` from original node → clone.
3. `dfs(original)`:
   - If in map, return clone.
   - Create clone, store in map **before** recursing.
   - For each neighbor, push `dfs(neighbor)` onto clone.neighbors.
4. Return `dfs(node)`.

---

## JavaScript solution

```javascript
function cloneGraph(node) {
  if (!node) return null;

  const originalToClone = new Map();

  function dfs(original) {
    if (originalToClone.has(original)) {
      return originalToClone.get(original);
    }

    const clone = { val: original.val, neighbors: [] };
    originalToClone.set(original, clone);

    for (const neighbor of original.neighbors) {
      clone.neighbors.push(dfs(neighbor));
    }

    return clone;
  }

  return dfs(node);
}
```

---

## Big-O analysis

- **Time complexity:** O(V + E)
- **Space complexity:** O(V)

---

## Gotchas

- Map **before** neighbor recursion (cycles).

---

## Quick Revise Summary

- `Map` old→new; DFS; register clone first.
