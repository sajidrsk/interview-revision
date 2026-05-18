# DSA: Valid Palindrome (Two Pointers)

Given string `s`, return `true` if it is a palindrome considering only alphanumeric characters and ignoring case.

**LeetCode:** [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

---

## Why it matters for FE

Two pointers on strings model symmetric head/tail checks in lists and parsers.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Clean string + reverse | O(n) | O(n) | Extra space |
| **Two pointers** | **O(n)** | **O(1)** | **Default** |

---

## Algorithm

1. Set `left = 0`, `right = s.length - 1`.
2. While `left < right`:
   - Advance `left` while non-alphanumeric.
   - Advance `right` while non-alphanumeric.
   - Compare `s[left].toLowerCase()` with `s[right].toLowerCase()`; mismatch → `false`.
   - Move `left++`, `right--`.
3. Return `true`.

---

## JavaScript solution

```javascript
function isPalindrome(s) {
  const isAlphanumeric = (ch) => /[a-z0-9]/i.test(ch);

  let left = 0;
  let right = s.length - 1;

  while (left < right) {
    while (left < right && !isAlphanumeric(s[left])) left++;
    while (left < right && !isAlphanumeric(s[right])) right--;

    if (s[left].toLowerCase() !== s[right].toLowerCase()) {
      return false;
    }
    left++;
    right--;
  }

  return true;
}
```

---

## Big-O analysis

- **Time complexity:** O(n)
- **Space complexity:** O(1)

---

## Gotchas

- Skip junk characters before each compare.
- Empty string → `true`.

---

## Quick Revise Summary

- `left` / `right`; skip non-alnum; compare lowercase.
