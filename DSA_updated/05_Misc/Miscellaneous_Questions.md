# Miscellaneous Questions

> **📅 Written:** 24 Sep 2026

### Purpose

This file is a **curated cross-topic reference table**, not a single-concept note. It collects strong practice problems that don't belong cleanly to one specific topic file — either because they **combine multiple patterns**, or because they're common **"trick" questions** that test sharp thinking rather than a named technique.

**How to use this file:** If you're stuck placing a problem into one of the other topic notes, it probably belongs here. Each row links the problem to the pattern(s) it actually draws on, so you can jump back to the relevant topic file for a refresher if needed.

---

## Multi-Pattern / Combination Problems

| # | Problem | Difficulty | Patterns Combined |
|---|---------|-----------|---------------------|
| 1 | [LRU Cache](https://leetcode.com/problems/lru-cache/) | Medium | HashMap + Doubly Linked List + OOP design |
| 2 | [Reorder List](https://leetcode.com/problems/reorder-list/) | Medium | Linked List (find middle) + Reversal + Merge |
| 3 | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | Hard | Two Heaps (min + max) |
| 4 | [Word Search](https://leetcode.com/problems/word-search/) | Medium | Backtracking + Grid/Matrix Traversal |
| 5 | [Course Schedule](https://leetcode.com/problems/course-schedule/) | Medium | Graph + Topological Sort (cycle detection) |
| 6 | [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | Hard | Tree Traversal (pre-order) + String parsing |
| 7 | [Design Twitter](https://leetcode.com/problems/design-twitter/) | Medium | HashMap + Priority Queue + OOP design |
| 8 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | Hard | Prefix/Suffix Max + Two Pointers |
| 9 | [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/) | Hard | Sliding Window + Two Heaps |
| 10 | [Word Ladder](https://leetcode.com/problems/word-ladder/) | Hard | BFS + String manipulation |

---

## Classic "Trick" / Sharp-Thinking Problems

*(These are famous for having a clever O(1)-space or O(n)-time solution that isn't the "obvious" approach — worth knowing the trick by name.)*

| # | Problem | Difficulty | The Trick |
|---|---------|-----------|-----------|
| 1 | [Majority Element](https://leetcode.com/problems/majority-element/) | Easy | Boyer-Moore Voting Algorithm — O(1) space, no HashMap needed |
| 2 | [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) | Medium | Floyd's Cycle Detection applied to an array (treating values as pointers) |
| 3 | [Single Number](https://leetcode.com/problems/single-number/) | Easy | XOR cancellation |
| 4 | [Missing Number](https://leetcode.com/problems/missing-number/) | Easy | XOR or Gauss's sum formula, O(1) space |
| 5 | [Rotate Array](https://leetcode.com/problems/rotate-array/) | Medium | Triple-reversal trick for O(1) space rotation |
| 6 | [Gas Station](https://leetcode.com/problems/gas-station/) | Medium | Greedy reset trick (see Greedy Algorithms note) |
| 7 | [Next Permutation](https://leetcode.com/problems/next-permutation/) | Medium | In-place lexicographic-next-arrangement algorithm |
| 8 | [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) | Hard | Index-as-hash-table trick, O(1) space |
| 9 | [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) | Medium | Binary search on value range, not index |

---

## Design-Style Problems (System-in-Miniature)

*(Frequently asked to test how cleanly you structure a class/API, not just algorithmic correctness.)*

| # | Problem | Difficulty | What It Tests |
|---|---------|-----------|----------------|
| 1 | [Design HashMap](https://leetcode.com/problems/design-hashmap/) | Easy | Implementing hashing + collision handling from scratch |
| 2 | [Design a Stack With Increment Operation](https://leetcode.com/problems/design-a-stack-with-increment-operation/) | Medium | Extending a base structure cleanly |
| 3 | [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/) | Medium | Combining ArrayList + HashMap for O(1) everything |
| 4 | [Design Circular Queue](https://leetcode.com/problems/design-circular-queue/) | Medium | Array-based circular buffer mechanics |
| 5 | [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) | Medium | Custom tree-like node structure |
| 6 | [Design Underground System](https://leetcode.com/problems/design-underground-system/) | Medium | Multiple collaborating classes/state |

---

## Interval / Scheduling Problems

*(A small but very interview-common family — usually a sort + greedy scan.)*

| # | Problem | Difficulty | Core Idea |
|---|---------|-----------|-----------|
| 1 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Medium | Sort by start, merge overlapping |
| 2 | [Insert Interval](https://leetcode.com/problems/insert-interval/) | Medium | Merge-intervals logic applied to a single insertion |
| 3 | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | Medium | Min-heap tracking concurrent meeting end times |
| 4 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | Medium | Greedy activity selection, framed as minimum removals |

---

## Quick Revision

- This file is a **navigation aid** — when a problem doesn't fit one topic cleanly, check here for which combination of patterns it's actually testing.
- **Multi-pattern problems** → strong signal you've mastered the individual topics if you can combine them smoothly.
- **"Trick" problems** → worth memorizing by name (Boyer-Moore, Floyd's on arrays, XOR tricks) since the "obvious" solution is rarely the optimal one.
- **Design problems** → test clean class structure and encapsulation as much as algorithmic correctness.
- **Interval problems** → almost always sort + greedy scan.
