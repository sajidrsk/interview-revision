# Frontend DSA — 15-Minute Quick Revision

One-page-style cram sheet for all **32** problems in this track. For full explanations, see the linked notes in [README.md](README.md).

**How to use:** Skim pattern map → scan each block → close and code 2–3 weak ones on LeetCode.

---

## Pattern map (recognize → pick)

| You see… | Pattern | Time | Space |
|----------|---------|------|-------|
| Pair / complement / count frequency | Hash map or Set | O(n) | O(n) |
| Sorted array + pair sum | Two pointers | O(n) | O(1) |
| Substring / subarray “without X” | Sliding window + Set/Map | O(n) | O(k) |
| Nested brackets / undo / evaluate | Stack | O(n) | O(n) |
| Next greater / warmer day | Monotonic stack (indices) | O(n) | O(n) |
| Linked list transform | Dummy head + pointers | O(n) | O(1) |
| Cycle in list | Fast/slow (Floyd) | O(n) | O(1) |
| Tree depth / mirror / compare | DFS recursion | O(n) | O(h) |
| Level-by-level tree | BFS + frozen level size | O(n) | O(w) |
| BST order / LCA in BST | Bounds or compare values | O(h) | O(1)–O(h) |
| 2D grid connected components | DFS/BFS flood fill | O(mn) | O(mn) |
| Clone / visit graph | DFS/BFS + Map old→new | O(V+E) | O(V) |
| Sorted search | Binary search | O(log n) | O(1) |
| Overlapping ranges / meetings | Sort + merge or sweep | O(n log n) | O(n) |

**Skip in FE rounds (for now):** DP tables, greedy proofs, backtracking, Dijkstra, union-find.

---

## 1. Hash map & array (7)

### 01 — Two Sum
**Ask:** Indices of two numbers summing to `target`.  
**Pick:** Map `value → index`, one pass.  
**Logic:** `need = target - nums[i]`; if `map.has(need)` return; else `map.set(nums[i], i)`.  
```javascript
for (let i = 0; i < nums.length; i++) {
  const need = target - nums[i];
  if (seen.has(need)) return [seen.get(need), i];
  seen.set(nums[i], i);
}
```
**O(n) / O(n)** · Check complement **before** insert (same index trap).

---

### 02 — Contains Duplicate
**Ask:** Any value appears twice?  
**Pick:** Set one pass.  
**Logic:** `seen.has(n)` → true; else `seen.add(n)`.  
**O(n) / O(n)** · Sort+scan neighbors works if you can’t use extra space.

---

### 03 — Valid Anagram
**Ask:** Same letters, same counts?  
**Pick:** Map frequency (+1 on `s`, -1 on `t`) or 26-count array for a–z.  
**Logic:** Length mismatch → false; any count negative → false.  
**O(n) / O(k)** · Sort both strings is O(n log n) — slower.

---

### 04 — Group Anagrams
**Ask:** Group strings that are anagrams.  
**Pick:** Map key → list of words; key = sorted word.  
```javascript
const key = word.split("").sort().join("");
groups.get(key)?.push(word) ?? groups.set(key, [word]);
```
**O(n · k log k) / O(nk)** · Frequency string key is O(n·k) if words are long.

---

### 05 — Top K Frequent Elements
**Ask:** `k` most frequent nums.  
**Pick:** Count with Map → **bucket** by frequency (index = count) → scan buckets high→low.  
**Alt:** Sort entries by freq O(n log n); min-heap size k O(n log k).  
**O(n) / O(n)** with buckets.

---

### 06 — Product of Array Except Self
**Ask:** `answer[i]` = product of all except `nums[i]`, no division.  
**Pick:** Prefix pass left→right in `answer`, suffix pass right→left multiply in place.  
```javascript
for (i) answer[i] = prefix, prefix *= nums[i];
for (i = n-1) answer[i] *= suffix, suffix *= nums[i];
```
**O(n) / O(1)** extra · Zeros handled naturally.

---

### 07 — Maximum Subarray (Kadane) — **sum**, not product
**Ask:** Max **sum** of contiguous subarray.  
**Pick:** `maxEndingHere = max(current, maxEndingHere + current)`; `maxSoFar = max(maxSoFar, maxEndingHere)`.  
```javascript
maxEndingHere = Math.max(current, maxEndingHere + current);
maxSoFar = Math.max(maxSoFar, maxEndingHere);
```
**O(n) / O(1)** · Not **Product Subarray** (that uses `minSoFar` + `maxSoFar` + `tempMax`).

---

## 2. Two pointers & sliding window (5)

### 08 — Valid Palindrome
**Ask:** Palindrome ignoring non-alphanumeric, case-insensitive.  
**Pick:** `left`/`right`, skip junk, compare `toLowerCase()`.  
**O(n) / O(1)** · Inner while skips before compare.

---

### 09 — Two Sum II (sorted, 1-indexed)
**Ask:** Two numbers in sorted array sum to target.  
**Pick:** `left=0`, `right=n-1`; sum too small → `left++`, too big → `right--`.  
**O(n) / O(1)** · Return `[left+1, right+1]`.

---

### 10 — 3Sum
**Ask:** All unique triplets summing to 0.  
**Pick:** Sort → for each `i`, two-pointer on `i+1..n-1` for `-nums[i]`; skip duplicate `i`, `left`, `right`.  
**O(n²) / O(1)** · Break early if `nums[i] > 0`.

---

### 11 — Container With Most Water
**Ask:** Max area between two lines.  
**Pick:** Ends `left`/`right`; area = `min(h[l],h[r]) * (r-l)`; move **shorter** pointer inward.  
**O(n) / O(1)** · Moving taller side can’t improve min height.

---

### 12 — Longest Substring Without Repeating
**Ask:** Length of longest substring with unique chars.  
**Pick:** Sliding window: `while (set.has(s[r]))` delete `s[l++]`; add `s[r]`; `max = max(max, r-l+1)`.  
**Alt:** Map char → last index; `left = max(left, last+1)`.  
**O(n) / O(k)** · Map version: `left` never moves backward.

---

## 3. Stack (4)

### 13 — Valid Parentheses
**Ask:** Brackets balanced and correctly nested.  
**Pick:** Open → push; close → pop and match `pairs[ch]`.  
**O(n) / O(n)** · Final `stack.length === 0`.

---

### 14 — Min Stack
**Ask:** Stack with O(1) `getMin`.  
**Pick:** Store `[value, minSoFar]` per push; `minSoFar = min(val, top.min)`.  
**O(1)** all ops · Pop restores previous min automatically.

---

### 15 — Evaluate Reverse Polish Notation
**Ask:** Value of postfix expression.  
**Pick:** Number → push; operator → pop `b`, pop `a`, push `op(a,b)`.  
**O(n) / O(n)** · Order: second pop = right operand; `/` → `Math.trunc(a/b)`.

---

### 16 — Daily Temperatures
**Ask:** Days until warmer temperature (0 if none).  
**Pick:** Monotonic **decreasing** stack of **indices**; when warmer, pop and set `ans[j]=i-j`.  
**O(n) / O(n)** · Each index pushed/popped once.

---

## 4. Linked list (3)

### 17 — Reverse Linked List
**Ask:** Reverse singly linked list.  
**Pick:** Iterative: `prev=null`; `next=curr.next`; `curr.next=prev`; advance.  
```javascript
while (curr) { next = curr.next; curr.next = prev; prev = curr; curr = next; }
return prev;
```
**O(n) / O(1)** · New head is `prev`.

---

### 18 — Merge Two Sorted Lists
**Ask:** Merge two sorted lists.  
**Pick:** Dummy node, `tail`; attach smaller head; `tail.next = l1 || l2`.  
**O(n+m) / O(1)** · Dummy avoids special-casing first node.

---

### 19 — Linked List Cycle
**Ask:** Has cycle?  
**Pick:** Floyd: slow +1, fast +2; meet → cycle.  
**Follow-up — cycle start:** after meet, reset one pointer to head; move both +1 until they meet again (= start of cycle).  
**O(n) / O(1)** · `while (fast && fast.next)`.

---

## 5. Trees — DFS & BFS (7)

**Tree node:** `{ val, left, right }` · **Graph node:** `{ val, neighbors: [] }`

### 20 — Maximum Depth
**Ask:** Longest root-to-leaf path (node count).  
**Pick:** `if (!root) return 0; return 1 + max(dfs(left), dfs(right))`.  
**O(n) / O(h)**

---

### 21 — Same Tree
**Ask:** Identical structure and values?  
**Pick:** Both null → true; one null or vals differ → false; recurse left & right.  
**O(n) / O(h)**

---

### 22 — Invert Binary Tree
**Ask:** Swap every left/right child.  
**Pick:** `[root.left, root.right] = [root.right, root.left]` then recurse.  
**O(n) / O(h)**

---

### 23 — Level Order Traversal
**Ask:** Values grouped by level `[[3],[9,20],...]`.  
**Pick:** BFS queue; each round `count = queue.length`, process exactly `count` nodes.  
```javascript
while (queue.length) {
  const count = queue.length, level = [];
  for (let i = 0; i < count; i++) { /* shift, push children */ }
  result.push(level);
}
```
**O(n) / O(w)** · Freeze `count` before inner loop.

---

### 24 — Validate BST
**Ask:** Valid binary search tree?  
**Pick:** DFS `(node, min, max)` — not just parent check.  
**Logic:** `node.val <= min || node.val >= max` → false; left `(min, node.val)`, right `(node.val, max)`.  
**O(n) / O(h)** · No duplicates allowed (`<=` / `>=` fail).

---

### 25 — LCA of BST
**Ask:** Lowest common ancestor of `p`, `q` in BST.  
**Pick:** If both smaller than root → left; both larger → right; else root is split → LCA.  
**O(h) / O(1)** iterative · General tree needs postorder, not just BST compare.

---

### 26 — Binary Tree Right Side View
**Ask:** Values seen from the right (top to bottom).  
**Pick:** BFS — append `node.val` when `i === count - 1` per level.  
**Alt:** DFS right-first; if `depth === result.length`, push val.  
**O(n) / O(w) or O(h)**

---

## 6. Graphs (2)

### 27 — Number of Islands
**Ask:** Count connected `'1'` regions (4-directional).  
**Pick:** Scan grid; on `'1'`, `count++` and DFS sink (`'0'`) all 4 neighbors.  
**O(mn) / O(mn)** · Can use `visited` set if grid can’t mutate.

---

### 28 — Clone Graph
**Ask:** Deep copy undirected graph.  
**Pick:** `Map<old, clone>`; DFS: if mapped return; create clone, map it, recurse neighbors.  
**O(V+E) / O(V)** · Map **before** recursing neighbors (cycles).

---

## 7. Binary search & intervals (4)

### 29 — Binary Search
**Ask:** Index of `target` in sorted array or -1.  
**Pick:** `left=0`, `right=n-1`, `while (left <= right)`, `mid = left + ((right-left)>>1)`.  
**O(log n) / O(1)** · Inclusive bounds; `left=mid+1` / `right=mid-1`.

---

### 30 — Search Rotated Sorted Array
**Ask:** Find `target` in rotated sorted array (no dupes).  
**Pick:** One BS: find which half is sorted; check if `target` in sorted half range.  
```javascript
if (nums[left] <= nums[mid]) { /* left half sorted */ }
else { /* right half sorted */ }
```
**O(log n) / O(1)** · Dupes need `left++` when `nums[left]==nums[mid]`.

---

### 31 — Merge Intervals
**Ask:** Merge overlapping `[start,end]`.  
**Pick:** Sort by start; merge into last if `start <= last[1]` else push new.  
```javascript
if (start <= last[1]) last[1] = Math.max(last[1], end);
else merged.push([start, end]);
```
**O(n log n) / O(n)**

---

### 32 — Meeting Rooms II
**Ask:** Minimum rooms for all meetings.  
**Pick:** Sort `starts` and `ends`; two pointers — if `starts[s] < ends[e]` need room++ else room-- (meeting ended). Track max rooms.  
**Alt:** Sort by start + min-heap of end times; pop ends ≤ current start.  
**O(n log n) / O(n)** · Same as max overlapping intervals.

---

## Universal templates (copy mentally)

**Hash complement**
```javascript
if (map.has(need)) return answer;
map.set(current, meta);
```

**Sliding window**
```javascript
while (invalid) shrink left++;
expand right; update best;
```

**Binary search**
```javascript
while (left <= right) {
  const mid = left + Math.floor((right - left) / 2);
  if (found) return mid;
  if (goLeft) right = mid - 1; else left = mid + 1;
}
```

**Tree DFS**
```javascript
if (!node) return base;
// process node
return combine(dfs(node.left), dfs(node.right));
```

**Tree BFS level**
```javascript
const q = [root];
while (q.length) {
  const size = q.length;
  for (let i = 0; i < size; i++) { /* process, enqueue children */ }
}
```

**Grid DFS**
```javascript
function dfs(r, c) {
  if (outOfBounds || notLand) return;
  markVisited();
  dfs(r+1,c); dfs(r-1,c); dfs(r,c+1); dfs(r,c-1);
}
```

**Linked list reverse**
```javascript
let prev = null, curr = head;
while (curr) { const n = curr.next; curr.next = prev; prev = curr; curr = n; }
```

---

## Pre-interview checklist (60 seconds)

- [ ] Complement → **Map** before insert  
- [ ] Sorted + pair sum → **two pointers**  
- [ ] Contiguous “best sub…” → **Kadane** or **sliding window**  
- [ ] Nesting / matching → **stack**  
- [ ] Next greater → **monotonic stack**  
- [ ] List cycle → **slow/fast**  
- [ ] Tree levels → **BFS + size**  
- [ ] BST validity → **min/max bounds**  
- [ ] Grid regions → **flood fill DFS**  
- [ ] Clone graph → **Map + DFS**  
- [ ] Sorted lookup → **binary search**  
- [ ] Calendar overlap → **sort + merge/sweep**  

---

## Full notes index

| # | File |
|---|------|
| 01–07 | [01-hash-array/](01-hash-array/) |
| 08–12 | [02-two-pointers-window/](02-two-pointers-window/) |
| 13–16 | [03-stack/](03-stack/) |
| 17–19 | [04-linked-list/](04-linked-list/) |
| 20–26 | [05-trees/](05-trees/) |
| 27–28 | [06-graphs/](06-graphs/) |
| 29–32 | [07-binary-search-intervals/](07-binary-search-intervals/) |

**Excluded (study later):** DP, greedy, permutations/backtracking, shortest path, trie, bit tricks.
