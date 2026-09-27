# Recently Asked OA Round Questions

> **📅 Written:** 24 Sep 2026

### Purpose

This file is a **curated reference table** of problem styles that commonly show up in **Online Assessment (OA) rounds** — the timed, auto-graded coding rounds companies use before human interviews. OAs tend to favor certain flavors of problems: clean array/string manipulation, simulation-heavy logic, and problems with a "twist" on a well-known pattern.

**How to use this file:** OAs reward **speed and correctness on the first try** more than interviews do (no interviewer to nudge you toward the right approach) — so the goal with this list is pattern-recognition speed, not just solvability.

---

## Array & String Manipulation (Very Common in OAs)

| # | Problem | Difficulty | Pattern to Recognize |
|---|---------|-----------|------------------------|
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) | Easy | HashMap lookup |
| 2 | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) | Medium | Prefix/Suffix product |
| 3 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Medium | Sliding Window |
| 4 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | Medium | HashMap with a computed key |
| 5 | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | Easy | Stack matching |
| 6 | [Rotate Array](https://leetcode.com/problems/rotate-array/) | Medium | In-place reversal trick |
| 7 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Medium | Sort + greedy scan |
| 8 | [Move Zeroes](https://leetcode.com/problems/move-zeroes/) | Easy | Two Pointers, in-place |
| 9 | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | Medium | Prefix Sum + HashMap |

---

## Simulation-Style Problems

*(OAs love these — no fancy algorithm needed, just careful, bug-free implementation of a described process.)*

| # | Problem | Difficulty | Why OAs Like It |
|---|---------|-----------|--------------------|
| 1 | [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) | Medium | Tests careful boundary/index handling under pressure |
| 2 | [Game of Life](https://leetcode.com/problems/game-of-life/) | Medium | Simulating a rule-based grid update, in-place |
| 3 | [Robot Return to Origin](https://leetcode.com/problems/robot-return-to-origin/) | Easy | Simple simulation, fast warm-up style question |
| 4 | [Design Parking System](https://leetcode.com/problems/design-parking-system/) | Easy | Straightforward state-tracking simulation |
| 5 | [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) | Medium | Multi-source BFS simulation over time steps |

---

## Sliding Window / Two Pointers (Frequent OA Favorites)

| # | Problem | Difficulty | Pattern |
|---|---------|-----------|---------|
| 1 | [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/) | Easy | Fixed-size sliding window |
| 2 | [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/) | Medium | Variable-size sliding window |
| 3 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | Medium | Two pointers, greedy shrink |
| 4 | [3Sum](https://leetcode.com/problems/3sum/) | Medium | Sort + two pointers |

---

## Graph/Grid Problems (Common in Mid-to-Senior OAs)

| # | Problem | Difficulty | Pattern |
|---|---------|-----------|---------|
| 1 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | Medium | DFS/BFS on a grid |
| 2 | [Course Schedule](https://leetcode.com/problems/course-schedule/) | Medium | Topological sort / cycle detection |
| 3 | [Clone Graph](https://leetcode.com/problems/clone-graph/) | Medium | DFS/BFS + HashMap for node mapping |
| 4 | [Word Ladder](https://leetcode.com/problems/word-ladder/) | Hard | BFS shortest path over transformations |

---

## Binary Search / Greedy (Common "Optimize This" OA Questions)

| # | Problem | Difficulty | Pattern |
|---|---------|-----------|---------|
| 1 | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | Medium | Binary Search on Answer |
| 2 | [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | Medium | Binary Search on Answer |
| 3 | [Jump Game](https://leetcode.com/problems/jump-game/) | Medium | Greedy reachability |
| 4 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | Medium | Greedy + heap/math |

---

## General OA Strategy Notes

- **Read constraints first** (see the Verdicts, Complexity and Constraints note) — OAs are auto-graded on hidden test cases with real time limits, so a wrong-complexity approach fails silently until submission.
- **Edge cases matter more than in interviews** — no interviewer to catch a missed edge case; the hidden test suite will.
- **Simulation problems reward careful reading** over clever algorithms — re-read the problem statement once fully before coding.
- **Time-box yourself** — OAs are timed across multiple questions; a stuck-for-20-minutes problem is often better abandoned and revisited than to risk the whole round.

---

## Quick Revision

- OAs favor **array/string manipulation**, **simulation**, **sliding window/two pointers**, and **binary search on answer / greedy** far more than obscure or highly theoretical topics.
- Speed and first-try correctness matter more here than in a live interview — pattern recognition should be near-instant by the time you're doing OAs for real.
- This file is a **practice roundup**, not a new concept — refer back to the dedicated topic files for the full explanation of any pattern listed here.
