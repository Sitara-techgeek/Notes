# Binary Search On Answer

> **📅 Written:** 24 Sep 2026

### Definition

**Binary Search on the Answer is a technique where, instead of searching for a target within a sorted array, you binary search over the space of *possible answers* to a problem — exploiting the fact that a feasibility check (`can(x)`) is monotonic across that space.**

**In one sentence:**
> If "can I achieve X?" flips cleanly from *no* to *yes* as X increases (or vice versa), you don't need to test every X one by one — binary search straight to the boundary.

---

## The Core Idea

Forget the input array being sorted — what matters here is that the **answer space** itself is monotonic:

```
Possible answers:  1  2  3  4  5  6  7  8  9  10
Feasibility:       F  F  F  F  T  T  T  T  T  T
                             ↑
                   binary search finds this exact boundary
```

Three ingredients needed to apply this pattern:
1. A **range of possible answers** `[lo, hi]` (often derived from problem constraints)
2. A **feasibility function** `check(x)` — "is x achievable / does x satisfy the condition?"
3. **Monotonicity** — once `check(x)` becomes true, it stays true for all larger (or smaller) x

> **Interview point ⭐:** Recognizing monotonicity is the hard part, not the binary search itself. Ask: *"If X works, does X+1 also work?"* — if yes, the answer space is monotonic and this pattern applies.

---

## The Template

### Finding the Minimum X for which `check(X)` is True

```java
int minimumFeasibleX(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (check(mid)) {
            hi = mid;          // mid works — try to find something smaller
        } else {
            lo = mid + 1;      // mid doesn't work — need to go bigger
        }
    }
    return lo;   // lo == hi, the smallest feasible answer
}
```

### Finding the Maximum X for which `check(X)` is True

```java
int maximumFeasibleX(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi - lo + 1) / 2;   // bias mid UPWARD to avoid infinite loop
        if (check(mid)) {
            lo = mid;           // mid works — try to find something bigger
        } else {
            hi = mid - 1;       // mid doesn't work — need to go smaller
        }
    }
    return lo;   // lo == hi, the largest feasible answer
}
```

> **Watch out ⭐:** For the "maximum X" version, you must bias `mid` **upward** (`+1` in the numerator) — otherwise, when `lo` and `hi` are adjacent (`hi = lo + 1`), `mid` always rounds down to `lo`, and if `check(lo)` is true, `lo` never updates → **infinite loop**. This is one of the most common bugs in this pattern.

---

## Worked Example: Koko Eating Bananas

**Problem gist:** Koko eats bananas from piles at a speed of `k` bananas/hour. Find the **minimum** `k` such that she finishes all piles within `h` hours.

**Why this fits the pattern:**
- Answer space: possible eating speeds `k`, from `1` to `max(piles)`
- `check(k)` = "can Koko finish all piles within `h` hours, eating at speed `k`?"
- Monotonic: if speed `k` works, any speed `> k` also works (faster is never worse)

```java
int minEatingSpeed(int[] piles, int h) {
    int lo = 1, hi = Arrays.stream(piles).max().getAsInt();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (canFinish(piles, h, mid)) {
            hi = mid;
        } else {
            lo = mid + 1;
        }
    }
    return lo;
}

boolean canFinish(int[] piles, int h, int speed) {
    long hoursNeeded = 0;
    for (int pile : piles) {
        hoursNeeded += Math.ceil((double) pile / speed);
    }
    return hoursNeeded <= h;
}
```

→ Search space is O(max(piles)), each `check()` call is O(n) → total **O(n log(max(piles)))**, vastly better than testing every speed from 1 upward.

---

## Worked Example: Capacity to Ship Packages Within D Days

**Problem gist:** Find the **minimum ship capacity** such that all packages (in order) can be shipped within `D` days.

- Answer space: capacity from `max(weights)` (must fit the heaviest package) to `sum(weights)` (ship everything in one day)
- `check(capacity)` = "can all packages ship within D days, given this capacity?"
- Monotonic: more capacity never increases the number of days needed

```java
int shipWithinDays(int[] weights, int days) {
    int lo = Arrays.stream(weights).max().getAsInt();
    int hi = Arrays.stream(weights).sum();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (daysNeeded(weights, mid) <= days) {
            hi = mid;
        } else {
            lo = mid + 1;
        }
    }
    return lo;
}

int daysNeeded(int[] weights, int capacity) {
    int days = 1, currentLoad = 0;
    for (int w : weights) {
        if (currentLoad + w > capacity) {
            days++;
            currentLoad = 0;
        }
        currentLoad += w;
    }
    return days;
}
```

---

## Spotting This Pattern in a New Problem

Look for phrasing like:
- "Find the **minimum/maximum** X such that..."
- "...within D days / within a budget / within a limit"
- A brute-force solution would be "try every possible X from lo to hi, check if it works" — that's your cue this pattern applies, just faster

```
Brute force:  for X in [lo, hi]: if check(X): return X    -> O((hi-lo) * cost(check))
Binary search on answer:                                   -> O(log(hi-lo) * cost(check))
```

---

## Time Complexity — Quick Table

| Component | Complexity |
|---|---|
| Binary search over answer range | O(log(hi - lo)) |
| Each `check(x)` call | Depends on problem — often O(n) |
| **Total** | O(n log(range)) typically |

---

## Quick Revision

- **Binary Search on Answer** → binary search over a range of *possible answers*, not over the input array.
- Requires: a bounded answer range, a `check(x)` feasibility function, and **monotonicity**.
- **Minimum feasible X** → `hi = mid` on success, `lo = mid + 1` on failure.
- **Maximum feasible X** → `lo = mid` on success (mid biased **upward**), `hi = mid - 1` on failure.
- Cue phrases: "minimum/maximum X such that...", "within D days/budget".
- Turns an O((range) × cost) brute force into O(log(range) × cost).

---

## Interview Definition ⭐

> **Binary Search on Answer applies binary search to the space of candidate answers rather than the input data itself, relying on a monotonic feasibility check to eliminate half the remaining candidates each step — it's recognizable whenever a problem asks for the minimum or maximum value satisfying some condition, and a brute-force scan over the answer range would otherwise be required.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | Medium | The canonical "minimum feasible X" template problem |
| 2 | [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | Medium | Another core "minimum feasible X" example |
| 3 | [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) | Hard | Same pattern, slightly trickier feasibility check |
| 4 | [Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/) | Medium | Very similar structure to Koko Eating Bananas — great for pattern recognition practice |
| 5 | [Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/) | Medium | Binary search over "day" as the answer, with a more complex feasibility check |
| 6 | [Magnetic Force Between Two Balls](https://leetcode.com/problems/magnetic-force-between-two-balls/) | Medium | "Maximum feasible X" variant of the pattern |
| 7 | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) | Hard | A different but related binary-search-on-partition flavor, good stretch problem |
