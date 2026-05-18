# DSA: Meeting Rooms II (Intervals — Sweep)

Minimum conference rooms for all meetings.

**LeetCode:** [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) (Premium)

---

## Why it matters for FE

Max overlapping meetings = concurrent resources needed.

---

## Approaches & trade-offs

| Approach | Time | Space | When to use / skip |
|----------|------|-------|---------------------|
| Sort starts + min-heap of ends | O(n log n) | O(n) | Good |
| **Sort starts & ends — two pointers** | **O(n log n)** | **O(n)** | **Interview-friendly** |

---

## Algorithm

1. `starts = sorted start times`, `ends = sorted end times`.
2. `startPtr = 0`, `endPtr = 0`, `roomsInUse = 0`, `maxRooms = 0`.
3. While `startPtr < starts.length`:
   - If `starts[startPtr] < ends[endPtr]`: need a room → `roomsInUse++`, update `maxRooms`, `startPtr++`.
   - Else: meeting ended → `roomsInUse--`, `endPtr++`.
4. Return `maxRooms`.

---

## JavaScript solution

```javascript
function minMeetingRooms(intervals) {
  if (!intervals.length) return 0;

  const starts = intervals.map((i) => i[0]).sort((a, b) => a - b);
  const ends = intervals.map((i) => i[1]).sort((a, b) => a - b);

  let startPtr = 0;
  let endPtr = 0;
  let roomsInUse = 0;
  let maxRooms = 0;

  while (startPtr < starts.length) {
    if (starts[startPtr] < ends[endPtr]) {
      roomsInUse++;
      maxRooms = Math.max(maxRooms, roomsInUse);
      startPtr++;
    } else {
      roomsInUse--;
      endPtr++;
    }
  }

  return maxRooms;
}
```

---

## Big-O analysis

- **Time complexity:** O(n log n) — sorting
- **Space complexity:** O(n) — start/end arrays

---

## Gotchas

- `starts[i] < ends[j]` means new meeting before one ends → need another room.
- Same as maximum overlapping intervals.

---

## Quick Revise Summary

- Sort starts and ends; sweep with two pointers; track peak rooms.
