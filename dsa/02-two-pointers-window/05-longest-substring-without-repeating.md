# DSA: Longest Substring Without Repeating Characters (Sliding Window)

Return length of longest substring without repeating characters.

**LeetCode:** [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

---

## Why it matters for FE

Sliding window for “valid segment” problems: inputs, buffers, rate limits.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| All substrings | O(n²)+ | O(1) | Skip |
| **Sliding window + Set** | **O(n)** | **O(k)** | **Default** |
| Map last index | O(n) | O(k) | Jump `left` faster |

---

## Algorithm

1. `left = 0`, `seen = Set`, `maxLength = 0`.
2. For `right` from `0` to `s.length - 1`:
   - While `s[right]` is in `seen`, remove `s[left]` from set and `left++`.
   - Add `s[right]` to `seen`.
   - `maxLength = max(maxLength, right - left + 1)`.
3. Return `maxLength`.

---

## JavaScript solution

```javascript
function lengthOfLongestSubstring(s) {
  const seen = new Set();
  let left = 0;
  let maxLength = 0;

  for (let right = 0; right < s.length; right++) {
    while (seen.has(s[right])) {
      seen.delete(s[left]);
      left++;
    }
    seen.add(s[right]);
    maxLength = Math.max(maxLength, right - left + 1);
  }

  return maxLength;
}
```

---

## Big-O analysis

- **Time complexity:** O(n) — each char added/removed once.
- **Space complexity:** O(min(n, alphabet size))

---

## Gotchas

- Window `[left, right]` inclusive; length = `right - left + 1`.
- Map variant: `left = max(left, lastIndex[ch] + 1)`.

---

## Quick Revise Summary

- Expand `right`, shrink `left` while duplicate in window.
