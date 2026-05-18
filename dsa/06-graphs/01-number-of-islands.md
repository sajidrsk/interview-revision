# DSA: Number of Islands (Grid DFS / BFS)

Count connected `'1'` regions in a 2D grid (4-directional).

**LeetCode:** [Number of Islands](https://leetcode.com/problems/number-of-islands/)

---

## Why it matters for FE

Flood-fill for region selection, connected cells, image tools.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **DFS flood fill** | **O(mn)** | **O(mn)** | **Default** |
| BFS | O(mn) | O(mn) | Same |

---

## Algorithm

1. `islandCount = 0`.
2. For each cell `(row, col)`:
   - If `grid[row][col] === '1'`:
     - `islandCount++`.
     - `sinkIsland(row, col)` — DFS mark as `'0'` and visit 4 neighbors.
3. Return `islandCount`.

---

## JavaScript solution

```javascript
function numIslands(grid) {
  const rows = grid.length;
  const cols = grid[0].length;
  let islandCount = 0;

  function sinkIsland(row, col) {
    if (row < 0 || col < 0 || row >= rows || col >= cols) return;
    if (grid[row][col] === "0") return;

    grid[row][col] = "0";
    sinkIsland(row + 1, col);
    sinkIsland(row - 1, col);
    sinkIsland(row, col + 1);
    sinkIsland(row, col - 1);
  }

  for (let row = 0; row < rows; row++) {
    for (let col = 0; col < cols; col++) {
      if (grid[row][col] === "1") {
        islandCount++;
        sinkIsland(row, col);
      }
    }
  }

  return islandCount;
}
```

---

## Big-O analysis

- **Time complexity:** O(m × n)
- **Space complexity:** O(m × n) worst-case recursion stack

---

## Gotchas

- Mutating grid avoids separate `visited` set.
- 4 directions unless problem says 8.

---

## Quick Revise Summary

- Scan grid; on `'1'`, count++ and DFS sink.
