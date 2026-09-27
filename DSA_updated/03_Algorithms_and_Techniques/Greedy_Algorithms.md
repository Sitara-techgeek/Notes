# Greedy Algorithms

> **📅 Written:** 24 Sep 2026

### Definition

**A Greedy Algorithm builds a solution step by step, always choosing the option that looks best *right now* — locally optimal — without reconsidering past choices, and trusts that this leads to a globally optimal solution.**

**In one sentence:**
> Greedy never looks back and never second-guesses — it commits fully to whatever seems best at each individual step, which only works when the problem's structure guarantees that local best choices actually add up to the global best.

---

## When Does Greedy Actually Work?

Greedy isn't always correct — it only works when the problem has these two properties:

### 1. Greedy Choice Property
A globally optimal solution can be reached by making a locally optimal choice at each step, without needing to reconsider it later.

### 2. Optimal Substructure
An optimal solution to the problem contains optimal solutions to its subproblems (same property DP relies on, but greedy doesn't need to explore all subproblems — it commits to one choice immediately).

> **Interview point ⭐:** The hardest part of a greedy problem is usually not writing the code — it's **proving** (or at least convincingly arguing) that the greedy choice property holds. Interviewers often want to hear *why* the greedy approach is correct, not just the implementation. A common proof technique is the **"exchange argument"**: show that any solution not following the greedy rule can be transformed into one that does, without making it worse.

---

## Classic Greedy Problems, Worked

### 1. Activity Selection (Interval Scheduling)

**Problem:** Given intervals, pick the maximum number of non-overlapping ones.

**Greedy rule:** Always pick the interval that **ends earliest** among the remaining valid options — leaves the most room for future intervals.

```
Intervals: [1,3], [2,4], [3,5], [6,8]
Sort by end time: [1,3], [2,4], [3,5], [6,8]

Pick [1,3]  (end=3)
Skip [2,4]  (starts before 3, overlaps)
Pick [3,5]  (starts at 3, no overlap)
Pick [6,8]  (no overlap)

Result: 3 intervals selected
```

```java
int maxActivities(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);   // sort by END time
    int count = 1;
    int lastEnd = intervals[0][1];
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= lastEnd) {
            count++;
            lastEnd = intervals[i][1];
        }
    }
    return count;
}
```
→ **O(n log n)**, dominated by the sort.

---

### 2. Fractional Knapsack

**Problem:** Maximize value in a knapsack of limited capacity, where items **can be split** (fractions allowed).

**Greedy rule:** Always take as much as possible of the item with the **highest value-per-weight ratio** first.

```java
double fractionalKnapsack(int[] weights, int[] values, int capacity) {
    int n = weights.length;
    Integer[] idx = new Integer[n];
    for (int i = 0; i < n; i++) idx[i] = i;
    Arrays.sort(idx, (a, b) -> Double.compare(
        (double) values[b] / weights[b], (double) values[a] / weights[a]));

    double totalValue = 0;
    for (int i : idx) {
        if (capacity <= 0) break;
        int take = Math.min(capacity, weights[i]);
        totalValue += take * ((double) values[i] / weights[i]);
        capacity -= take;
    }
    return totalValue;
}
```

> **Watch out ⭐:** Greedy works for **Fractional** Knapsack, but **fails** for **0/1 Knapsack** (items can't be split) — that version needs Dynamic Programming instead. This is one of the most instructive examples of "greedy works here, but not for the nearby-looking variant" in all of DSA.

---

### 3. Jump Game (Greedy Reachability)

**Problem:** Given an array of max-jump-lengths, determine if you can reach the last index.

**Greedy rule:** Track the **furthest reachable index** as you scan left to right; if the current index ever exceeds that, it's unreachable.

```java
boolean canJump(int[] nums) {
    int maxReach = 0;
    for (int i = 0; i < nums.length; i++) {
        if (i > maxReach) return false;        // can't even reach this index
        maxReach = Math.max(maxReach, i + nums[i]);
    }
    return true;
}
```
→ **O(n)** — a single greedy pass, tracking one running value.

---

### 4. Gas Station (Greedy with a Reset Trick)

**Problem:** Find the starting gas station from which a circular trip is possible.

**Greedy rule:** If the tank ever goes negative starting from candidate `start`, **no station between `start` and the failure point** could have worked either — so jump the candidate straight past the failure point.

```java
int canCompleteCircuit(int[] gas, int[] cost) {
    int total = 0, tank = 0, start = 0;
    for (int i = 0; i < gas.length; i++) {
        int diff = gas[i] - cost[i];
        total += diff;
        tank += diff;
        if (tank < 0) {
            start = i + 1;   // reset candidate — nothing before this could work either
            tank = 0;
        }
    }
    return total >= 0 ? start : -1;
}
```
→ **O(n)** — a subtle but powerful greedy insight, worth understanding the "why" deeply.

---

## Greedy vs Dynamic Programming ⭐

| Greedy | Dynamic Programming |
|---|---|
| One choice per step, **never reconsidered** | Explores/compares multiple choices, keeps the best |
| Works only when greedy-choice property holds | Works generally, whenever optimal substructure holds |
| Usually O(n) or O(n log n) | Usually higher — depends on state space |
| Fractional Knapsack ✅ | 0/1 Knapsack ✅ (greedy fails here) |

> **Interview point ⭐:** If you're not sure whether greedy will actually work, try to find a **counterexample** first — a small case where the "obviously best" local choice leads to a worse overall outcome. If you can't find one after genuinely trying, that's decent (informal) evidence greedy might actually hold.

---

## Time Complexity — Quick Table

| Problem | Time Complexity |
|---|---|
| Activity Selection | O(n log n) — sort dominates |
| Fractional Knapsack | O(n log n) — sort dominates |
| Jump Game | O(n) |
| Gas Station | O(n) |
| Huffman Coding (greedy + heap) | O(n log n) |

---

## Quick Revision

- **Greedy** → commits to the locally best choice at each step, never revisits it.
- Requires **greedy choice property** + **optimal substructure** to be provably correct.
- **Fractional Knapsack** → greedy works (sort by value/weight ratio). **0/1 Knapsack** → greedy fails, needs DP.
- **Exchange argument** → the standard proof technique for why a greedy rule is correct.
- Most greedy solutions are O(n) or O(n log n) — fast, but only correct when the problem structure actually supports it.

---

## Interview Definition ⭐

> **A greedy algorithm makes the locally optimal choice at every step without reconsideration, relying on the greedy choice property and optimal substructure to guarantee a globally optimal result — it's provably correct only for specific problem structures (like Fractional Knapsack or Activity Selection), and notably fails for close variants like 0/1 Knapsack, which require Dynamic Programming instead.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Jump Game](https://leetcode.com/problems/jump-game/) | Medium | Classic greedy reachability tracking |
| 2 | [Jump Game II](https://leetcode.com/problems/jump-game-ii/) | Medium | Greedy minimum-jumps variant |
| 3 | [Gas Station](https://leetcode.com/problems/gas-station/) | Medium | The reset-candidate greedy insight |
| 4 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | Medium | Activity selection, framed as "minimum removals" |
| 5 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Medium | Sort + greedy merge |
| 6 | [Candy](https://leetcode.com/problems/candy/) | Hard | Two-pass greedy, a great test of the "prove it's correct" instinct |
| 7 | [Partition Labels](https://leetcode.com/problems/partition-labels/) | Medium | Greedy interval-extension pattern |
| 8 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | Medium | Greedy scheduling combined with a max-heap |
