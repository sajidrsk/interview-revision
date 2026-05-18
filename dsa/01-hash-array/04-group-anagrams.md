# DSA: Group Anagrams (Hash Map)

Given an array of strings, group **anagrams** together. Return groups in any order.

**Example:** `["eat","tea","tan","ate","nat","bat"]` → `[["bat"],["nat","tan"],["ate","eat","tea"]]`.

**LeetCode:** [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

---

## Why it matters for FE

Grouping by a canonical key (sorted string) clusters tags, labels, or search terms.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| **Sort each word as key** | O(n · k log k) | O(n · k) | **Easy default** |
| Frequency signature key | O(n · k) | O(n · k) | Long words |
| Pairwise compare | O(n²) | O(1) | Skip |

---

## Algorithm

1. Create `Map<canonicalKey, string[]>`.
2. For each `word` in `strs`:
   - `key = word.split('').sort().join('')`.
   - If key missing, set `groups.set(key, [])`.
   - Push `word` into `groups.get(key)`.
3. Return `Array.from(groups.values())`.

---

## JavaScript solution

```javascript
function groupAnagrams(strs) {
  const groups = new Map();

  for (const word of strs) {
    const canonicalKey = word.split("").sort().join("");

    if (!groups.has(canonicalKey)) {
      groups.set(canonicalKey, []);
    }
    groups.get(canonicalKey).push(word);
  }

  return Array.from(groups.values());
}
```

---

## Big-O analysis

- **Time complexity:** O(n · k log k) — n words, k = max word length (sort per word).
- **Space complexity:** O(n · k) — store all strings in groups.

---

## Gotchas

- Key must be identical for all anagrams (sorted letters).
- Don’t mutate the original `word` if interviewer cares — sort a copy.

---

## Quick Revise Summary

- `Map<sortedWord, string[]>` → return `.values()`.
