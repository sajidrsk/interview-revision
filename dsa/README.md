# DSA — frontend interview revision

Short, **frontend-scoped** coding-round list (~32 problems). Read each note once, then implement on LeetCode in JavaScript.

**15-min cram:** [QUICK-REVISION.md](QUICK-REVISION.md) — all 32 problems, patterns, approaches, and code snippets in one file.

Each problem note includes: **Algorithm** (numbered steps), **commented JavaScript**, and **Big-O analysis**.

**Included:** arrays, hash maps/sets, two pointers, sliding window, stack, linked list basics, trees (DFS/BFS), simple graphs, binary search, intervals.

**Excluded for now:** dynamic programming, greedy, hard backtracking, advanced graphs.

---

## Study order

Work through in order, or jump by folder. Mark done by changing `[ ]` to `[x]`.

| # | Done | Pattern | Difficulty | Problem | Note |
|---|------|---------|------------|---------|------|
| 01 | [ ] | Hash map | Easy | [Two Sum](https://leetcode.com/problems/two-sum/) | [01-two-sum](01-hash-array/01-two-sum.md) |
| 02 | [ ] | Set | Easy | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) | [02-contains-duplicate](01-hash-array/02-contains-duplicate.md) |
| 03 | [ ] | Hash map | Easy | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) | [03-valid-anagram](01-hash-array/03-valid-anagram.md) |
| 04 | [ ] | Hash map | Medium | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | [04-group-anagrams](01-hash-array/04-group-anagrams.md) |
| 05 | [ ] | Hash + bucket/heap | Medium | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | [05-top-k-frequent](01-hash-array/05-top-k-frequent-elements.md) |
| 06 | [ ] | Prefix product | Medium | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) | [06-product-except-self](01-hash-array/06-product-of-array-except-self.md) |
| 07 | [ ] | Kadane | Medium | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | [07-maximum-subarray](01-hash-array/07-maximum-subarray.md) |
| 08 | [ ] | Two pointers | Easy | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | [01-valid-palindrome](02-two-pointers-window/01-valid-palindrome.md) |
| 09 | [ ] | Two pointers | Medium | [Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | [02-two-sum-ii](02-two-pointers-window/02-two-sum-ii.md) |
| 10 | [ ] | Two pointers | Medium | [3Sum](https://leetcode.com/problems/3sum/) | [03-3sum](02-two-pointers-window/03-3sum.md) |
| 11 | [ ] | Two pointers | Medium | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | [04-container-water](02-two-pointers-window/04-container-with-most-water.md) |
| 12 | [ ] | Sliding window | Medium | [Longest Substring Without Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | [05-longest-substring](02-two-pointers-window/05-longest-substring-without-repeating.md) |
| 13 | [ ] | Stack | Easy | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | [01-valid-parentheses](03-stack/01-valid-parentheses.md) |
| 14 | [ ] | Stack design | Medium | [Min Stack](https://leetcode.com/problems/min-stack/) | [02-min-stack](03-stack/02-min-stack.md) |
| 15 | [ ] | Stack | Medium | [Evaluate RPN](https://leetcode.com/problems/evaluate-reverse-polish-notation/) | [03-evaluate-rpn](03-stack/03-evaluate-reverse-polish-notation.md) |
| 16 | [ ] | Monotonic stack | Medium | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) | [04-daily-temperatures](03-stack/04-daily-temperatures.md) |
| 17 | [ ] | Linked list | Easy | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | [01-reverse](04-linked-list/01-reverse-linked-list.md) |
| 18 | [ ] | Linked list | Easy | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | [02-merge-two-lists](04-linked-list/02-merge-two-sorted-lists.md) |
| 19 | [ ] | Fast/slow | Easy | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | [03-cycle](04-linked-list/03-linked-list-cycle.md) |
| 20 | [ ] | Tree DFS | Easy | [Maximum Depth](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | [01-max-depth](05-trees/01-maximum-depth-binary-tree.md) |
| 21 | [ ] | Tree DFS | Easy | [Same Tree](https://leetcode.com/problems/same-tree/) | [02-same-tree](05-trees/02-same-tree.md) |
| 22 | [ ] | Tree DFS | Easy | [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) | [03-invert-tree](05-trees/03-invert-binary-tree.md) |
| 23 | [ ] | Tree BFS | Medium | [Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | [04-level-order](05-trees/04-binary-tree-level-order.md) |
| 24 | [ ] | Tree DFS | Medium | [Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) | [05-validate-bst](05-trees/05-validate-bst.md) |
| 25 | [ ] | Tree walk | Medium | [LCA of BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | [06-lca-bst](05-trees/06-lowest-common-ancestor-bst.md) |
| 26 | [ ] | Tree BFS/DFS | Medium | [Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) | [07-right-side-view](05-trees/07-binary-tree-right-side-view.md) |
| 27 | [ ] | Grid DFS/BFS | Medium | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | [01-islands](06-graphs/01-number-of-islands.md) |
| 28 | [ ] | Graph BFS/DFS | Medium | [Clone Graph](https://leetcode.com/problems/clone-graph/) | [02-clone-graph](06-graphs/02-clone-graph.md) |
| 29 | [ ] | Binary search | Easy | [Binary Search](https://leetcode.com/problems/binary-search/) | [01-binary-search](07-binary-search-intervals/01-binary-search.md) |
| 30 | [ ] | Binary search | Medium | [Search Rotated Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | [02-rotated-search](07-binary-search-intervals/02-search-rotated-sorted-array.md) |
| 31 | [ ] | Intervals | Medium | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | [03-merge-intervals](07-binary-search-intervals/03-merge-intervals.md) |
| 32 | [ ] | Intervals + heap | Medium | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | [04-meeting-rooms-ii](07-binary-search-intervals/04-meeting-rooms-ii.md) |

---

## 3-week schedule

| Week | Folders | Problems |
|------|---------|----------|
| 1 | `01-hash-array`, `02-two-pointers-window`, `03-stack` | 01–16 |
| 2 | `04-linked-list`, `05-trees` | 17–26 |
| 3 | `06-graphs`, `07-binary-search-intervals` + redo 5 weak | 27–32 |

---

## Related notes

- [JavaScript fundamentals](../javascript/README.md)
- [React UI interviews](../react/01-file-explorer.md) — tree DFS ties to file explorer
