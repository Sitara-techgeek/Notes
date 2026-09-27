# Bit Manipulation - Application

> **📅 Written:** 24 Sep 2026

### Definition

**Beyond basic bit tricks, bit manipulation powers several advanced techniques — representing sets as bitmasks, encoding state for dynamic programming, and generating subsets — used when the problem's state space is small enough to fit in the bits of an integer.**

**In one sentence:**
> Once you notice a problem's "state" is really just "which of these ≤30 things are included/true", you can pack that entire state into a single integer and manipulate it with O(1) bitwise operations instead of a slower Set or boolean array.

*(This note assumes familiarity with the basic operators and tricks from Bit Manipulation - Easy.)*

---

## 1. Representing a Set as a Bitmask

Each bit position represents whether an element is present. For n elements, a bitmask needs n bits — this only scales up to roughly **n ≤ 20-25** for practical use (since 2ⁿ states must be enumerable).

```
Elements: {0, 1, 2, 3}
Subset {0, 2} -> bitmask: 0101  (bit 0 set, bit 2 set)
Subset {1, 3} -> bitmask: 1010
Full set      -> bitmask: 1111
Empty set     -> bitmask: 0000
```

```java
int mask = 0;
mask |= (1 << 2);        // add element 2 to the set
boolean has2 = (mask & (1 << 2)) != 0;   // check membership
mask &= ~(1 << 2);       // remove element 2
```

---

## 2. Generating All Subsets Using Bitmasks

Every number from `0` to `2^n - 1`, in binary, represents exactly one subset — iterate through all of them.

```java
List<List<Integer>> subsets(int[] nums) {
    int n = nums.length;
    List<List<Integer>> result = new ArrayList<>();

    for (int mask = 0; mask < (1 << n); mask++) {
        List<Integer> subset = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) != 0) {
                subset.add(nums[i]);
            }
        }
        result.add(subset);
    }
    return result;
}
```

```
n = 3, nums = [1, 2, 3]

mask=000 -> {}          mask=100 -> {3}
mask=001 -> {1}          mask=101 -> {1,3}
mask=010 -> {2}          mask=110 -> {2,3}
mask=011 -> {1,2}        mask=111 -> {1,2,3}
```
→ **O(2ⁿ × n)** — generates all 2ⁿ subsets, each taking O(n) to build.

> **Interview point ⭐:** This is a clean iterative alternative to the usual backtracking-based subset generation — useful when the interviewer specifically wants to see the bitmask approach, or when subsets need to be processed in a specific numeric mask order.

---

## 3. Bitmask Dynamic Programming (DP on Subsets)

Used when the DP **state** includes "which subset of items have been used/visited so far" — the classic example is the **Traveling Salesman Problem (TSP)**.

```
dp[mask][i] = minimum cost to visit all cities in `mask`, ending at city i

Transition:
dp[mask][i] = min over all j in mask (j != i) of:
              dp[mask without i][j] + cost(j, i)
```

```java
int[][] dp = new int[1 << n][n];
for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE);
dp[1][0] = 0;   // start at city 0, only city 0 visited

for (int mask = 1; mask < (1 << n); mask++) {
    for (int i = 0; i < n; i++) {
        if ((mask & (1 << i)) == 0 || dp[mask][i] == Integer.MAX_VALUE) continue;
        for (int j = 0; j < n; j++) {
            if ((mask & (1 << j)) != 0) continue;   // j already visited
            int newMask = mask | (1 << j);
            dp[newMask][j] = Math.min(dp[newMask][j], dp[mask][i] + cost[i][j]);
        }
    }
}
```
→ **O(2ⁿ × n²)** — dramatically better than the O(n!) brute force of trying every permutation of cities, though still only practical for small n (roughly ≤ 15-20).

> **Interview point ⭐:** Bitmask DP is the go-to whenever a problem screams "try all subsets/permutations" but n is small (≤ 20ish) — it's a strong signal in "assign tasks to workers" or "visit all X" style problems.

---

## 4. Useful Advanced Bit Tricks

### Isolate the lowest set bit

```java
int lowestSetBit = n & (-n);
```
Works because `-n` is the two's complement of `n` (flip all bits, add 1) — this flips every bit **above** the lowest set bit, so ANDing with the original isolates exactly that bit.

### Get all subsets of a given bitmask (submask enumeration)

```java
for (int sub = mask; sub > 0; sub = (sub - 1) & mask) {
    // process submask `sub`
}
// don't forget to also process sub == 0 separately if needed
```
→ Enumerates every submask of `mask` in O(3ⁿ) total across all masks (a known amortized bound) — appears in advanced bitmask DP optimizations.

### Check if two numbers have opposite signs

```java
boolean oppositeSigns(int a, int b) {
    return (a ^ b) < 0;   // XOR of sign bits is 1 only if signs differ
}
```

### Multiply/Divide by powers of 2

```java
int mul4 = n << 2;   // n * 4
int div4 = n >> 2;    // n / 4 (floor, for non-negative n)
```
> **Watch out ⭐:** `>>` on a negative number does **floor** division toward negative infinity, not truncation toward zero — `-7 >> 1` gives `-4`, not `-3`. This differs from `-7 / 2` which gives `-3` in Java (truncates toward zero). Mixing these up is a subtle bug source.

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Generate all subsets via bitmask | O(2ⁿ × n) |
| Bitmask DP (TSP-style) | O(2ⁿ × n²) |
| Submask enumeration (all masks combined) | O(3ⁿ) |
| Isolate lowest set bit (`n & -n`) | O(1) |

---

## Quick Revision

- **Bitmask** → represents a set/state using an integer's bits; practical for n ≤ ~20-25.
- **Subset generation** → iterate mask from 0 to 2ⁿ-1, each mask is one subset.
- **Bitmask DP** → state includes "which items used so far" as a mask; classic for TSP-style problems, O(2ⁿ × n²) vs O(n!) brute force.
- **`n & -n`** → isolates the lowest set bit.
- **Submask enumeration trick** → `for (sub = mask; sub > 0; sub = (sub-1) & mask)`.
- Watch the sign behavior of `>>` on negative numbers — floors toward negative infinity, unlike integer division.

---

## Interview Definition ⭐

> **Advanced bit manipulation applications use an integer's bits to represent an entire set or state compactly, enabling O(2ⁿ) subset enumeration and bitmask dynamic programming — where the DP state tracks "which elements have been used" as a mask — turning otherwise-factorial brute-force problems like the Traveling Salesman Problem into a more tractable O(2ⁿ × n²).**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Subsets](https://leetcode.com/problems/subsets/) | Medium | Can be solved via bitmask iteration instead of backtracking |
| 2 | [Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/) | Medium | Bitmask used to track which elements are already placed |
| 3 | [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) | Hard | Bitmask + BFS, a TSP-flavored problem |
| 4 | [Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/) | Medium | Bitmask DP over task-assignment states |
| 5 | [Maximum Students Taking Exam](https://leetcode.com/problems/maximum-students-taking-exam/) | Hard | Bitmask DP over row states in a grid |
| 6 | [Single Number II](https://leetcode.com/problems/single-number-ii/) | Medium | Bit-counting trick extended beyond simple XOR cancellation |
