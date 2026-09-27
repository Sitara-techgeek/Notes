# Verdicts, Complexity and Constraints

> **📅 Written:** 24 Sep 2026

### Definition

**Verdicts are the feedback an online judge gives after evaluating a submission (Accepted, Wrong Answer, Time Limit Exceeded, etc.), while Complexity and Constraints refer to reading a problem's input limits to reverse-engineer what time complexity your solution needs to have.**

**In one sentence:**
> Before writing a single line of code, the problem's constraints already tell you which algorithms are even allowed to work — learning to read that signal saves you from designing a solution that was doomed from the start.

---

## Common Judge Verdicts

| Verdict | Abbreviation | Meaning |
|---|---|---|
| Accepted | AC | Correct output, within time & memory limits |
| Wrong Answer | WA | Output doesn't match the expected result |
| Time Limit Exceeded | TLE | Solution is too slow — usually a complexity problem, not a bug |
| Memory Limit Exceeded | MLE | Uses too much memory — often from oversized arrays/recursion depth |
| Runtime Error | RE | Crashes during execution — e.g. `ArrayIndexOutOfBoundsException`, `NullPointerException`, division by zero |
| Compilation Error | CE | Code doesn't compile |
| Presentation Error | PE | Correct values, but wrong formatting (extra spaces, missing newline) — rare on modern judges |
| Wrong Answer on Test X | WA on test X | Tells you *which* test case failed — useful for finding edge cases your logic misses |

> **Interview point ⭐:** TLE is a **complexity** signal, not a "make the code faster" signal — micro-optimizing an O(n²) solution rarely turns it into a passing O(n log n) one. If you're hitting TLE, the fix is almost always a better algorithm, not tighter code.

---

## Reading Constraints to Estimate Required Complexity

Competitive judges typically allow roughly **10⁸ operations per second** as a rule of thumb (varies by judge, but a solid mental anchor). Given the input size `n`, you can back-calculate the time complexity your algorithm needs.

| Constraint on n | Required Complexity (roughly) | Typical Algorithms |
|---|---|---|
| n ≤ 10 | O(n!) or O(2ⁿ · n) | Brute force permutations, exhaustive search |
| n ≤ 20-25 | O(2ⁿ) | Bitmask DP, subset enumeration |
| n ≤ 500 | O(n³) | Triple nested loops, Floyd-Warshall |
| n ≤ 5,000 | O(n²) | Double nested loops, simple DP |
| n ≤ 10⁶ | O(n log n) | Sorting-based solutions, efficient DP, heaps |
| n ≤ 10⁸ | O(n) | Single-pass algorithms, prefix sums, two pointers |
| n very large (10⁹+) | O(log n) or O(1) | Binary search, math formulas, matrix exponentiation |

```
If n ≤ 10⁵ and time limit is 1-2 seconds:
   O(n²) = 10¹⁰ operations -> WAY too slow, will TLE
   O(n log n) = ~1.7 × 10⁶ operations -> comfortably fast
```

> **Interview point ⭐:** This is one of the most practically useful competitive-programming habits — glance at the constraints **before** designing a solution, and let them tell you the target complexity. If `n ≤ 20`, the problem is basically *telling you* it wants an exponential/bitmask approach.

---

## Time Limit and What It Implies

```
Time Limit: 1 second,  n ≤ 10⁵
   -> Need roughly O(n log n) or better

Time Limit: 2 seconds, n ≤ 10⁹
   -> Need roughly O(log n) or O(√n) — even O(n) would likely TLE at this scale
```

- A **generous** time limit (3-5 seconds) on a small n can hint that an O(n²) or even O(n³) brute force is intended
- A **tight** time limit on a large n almost always rules out anything above O(n log n)

---

## Space Constraints

Memory limits matter too — a common pitfall is allocating an array/2D matrix sized by the constraint without checking if it actually fits.

```
Memory Limit: 256 MB
int[10^9] would need ~4 GB -> instant MLE, even before considering time

Rule of thumb: 1 million ints ≈ 4 MB
So an int array of size 10^7 ≈ 40 MB — generally safe;
size 10^8 ≈ 400 MB — likely MLE on a 256 MB limit.
```

> **Watch out ⭐:** A 2D array sized `n × n` for `n = 10⁵` would need `10¹⁰` cells — this is an instant giveaway that an O(n²) space (or time) approach is **not** what the problem intends, even before running anything.

---

## Edge Cases Worth Checking (to Avoid WA)

- **Empty input** — empty array, empty string, n = 0
- **Single element** — n = 1
- **All identical elements** — e.g. all zeros, all same character
- **Already sorted / reverse sorted** input
- **Negative numbers**, if the problem allows them (easy to assume non-negative by accident)
- **Integer overflow** — does the expected answer exceed `int` range? Should you use `long`?
- **Duplicate values**, if uniqueness isn't guaranteed
- **Maximum constraint values** — does your solution handle the absolute largest allowed n without TLE/MLE?

---

## Quick Revision

- **AC** → correct; **WA** → wrong output; **TLE** → wrong complexity, not a speed bug; **MLE** → oversized memory usage; **RE** → crash.
- **~10⁸ ops/sec** is the standard mental anchor for estimating what complexity a time limit allows.
- **Read constraints FIRST** — they tell you the required complexity before you write any code.
- `n ≤ 20` → exponential/bitmask; `n ≤ 5000` → O(n²); `n ≤ 10⁶` → O(n log n); `n ≤ 10⁸` → O(n); very large → O(log n).
- Watch memory too — a naive `n × n` array can blow the memory limit even if time wouldn't.
- Always sanity-check edge cases: empty input, single element, negatives, overflow, max constraints.

---

## Interview Definition ⭐

> **A problem's stated constraints (input size, time limit, memory limit) implicitly define the required algorithmic complexity — using the rough anchor of 10⁸ operations per second, an input size of n ≤ 20 signals exponential/bitmask solutions, n ≤ 5000 signals O(n²), and n ≤ 10⁶ or beyond signals O(n log n) or better — reading this signal before designing a solution avoids wasted effort on an approach that was mathematically doomed to time out.**

---

## Must-Solve LeetCode Problems

*(This topic is a meta-skill rather than a specific algorithm — the practice is reading the constraints on ANY problem before solving it. A few problems where constraints particularly drive the intended approach:)*

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Subsets](https://leetcode.com/problems/subsets/) | Medium | Small n (≤10) constraint directly signals an exponential/bitmask approach |
| 2 | [Two Sum](https://leetcode.com/problems/two-sum/) | Easy | Large n constraint rules out the O(n²) brute force, signals HashMap |
| 3 | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) | Hard | The explicit O(log(m+n)) requirement rules out simple merging entirely |
| 4 | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) | Medium | Constraints push toward the O(n log n) patience-sorting solution over O(n²) DP |
