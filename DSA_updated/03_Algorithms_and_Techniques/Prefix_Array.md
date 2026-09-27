# Prefix Array

> **📅 Written:** 24 Sep 2026

### Definition

**A Prefix Array (Prefix Sum Array) is a precomputed array where each index holds the cumulative sum of all elements up to that point, allowing any range-sum query to be answered in O(1) instead of O(n).**

**In one sentence:**
> Instead of re-adding the same numbers over and over for every range query, you add them up once upfront, then answer any "sum from i to j" question with a single subtraction.

**Example:**

```
Array:   [3, 1, 4, 1, 5]
Prefix:  [3, 4, 8, 9, 14]
          ↑  ↑  ↑  ↑  ↑
       prefix[i] = sum of arr[0..i]
```

---

## Building a Prefix Array

```java
int[] arr = {3, 1, 4, 1, 5};
int[] prefix = new int[arr.length];
prefix[0] = arr[0];
for (int i = 1; i < arr.length; i++) {
    prefix[i] = prefix[i - 1] + arr[i];
}
// prefix = [3, 4, 8, 9, 14]
```

→ **O(n)** to build.

### A Common Convention: 1-Indexed Prefix Array

Many implementations use a prefix array of size `n + 1`, with `prefix[0] = 0`, to make range queries cleaner (no special-casing for range starting at index 0):

```java
int[] prefix = new int[arr.length + 1];
for (int i = 0; i < arr.length; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}
// prefix = [0, 3, 4, 8, 9, 14]
```

---

## Range Sum Query — The Whole Point

Once built, the sum of any range `[l, r]` (inclusive) is:

```
0-indexed prefix:  sum(l, r) = prefix[r] - prefix[l-1]   (handle l == 0 separately)

1-indexed prefix:  sum(l, r) = prefix[r+1] - prefix[l]   (no special case needed)
```

```java
// Using the 1-indexed convention
int rangeSum(int[] prefix, int l, int r) {
    return prefix[r + 1] - prefix[l];
}
```

→ **O(1)** per query, after the one-time O(n) build.

```
Array:    [3, 1, 4, 1, 5]
Prefix:  [0, 3, 4, 8, 9, 14]

sum(1, 3) = prefix[4] - prefix[1] = 9 - 3 = 6
Check: arr[1] + arr[2] + arr[3] = 1 + 4 + 1 = 6 ✅
```

> **Interview point ⭐:** Whenever a problem asks for **many repeated range-sum queries** on a static (unchanging) array, prefix sums turn an O(n) per-query brute force into O(1) per query — a huge win when there are many queries.

---

## 2D Prefix Sum (Prefix Sum of a Matrix)

Extends the same idea to a matrix, using **inclusion-exclusion** to avoid double-counting the overlapping region.

```
prefix[i][j] = sum of all cells in the rectangle from (0,0) to (i,j)

prefix[i][j] = matrix[i][j]
             + prefix[i-1][j]      (sum above)
             + prefix[i][j-1]      (sum to the left)
             - prefix[i-1][j-1]    (subtract double-counted overlap)
```

```
      +---+---+          Overlap region counted twice
      | A | B |          if we just add "above" + "left" —
      +---+---+          must subtract it back out once.
      | C | X |
      +---+---+
```

```java
int[][] prefix = new int[rows + 1][cols + 1];
for (int i = 1; i <= rows; i++) {
    for (int j = 1; j <= cols; j++) {
        prefix[i][j] = matrix[i-1][j-1]
                      + prefix[i-1][j]
                      + prefix[i][j-1]
                      - prefix[i-1][j-1];
    }
}
```

**Querying a sub-rectangle sum** `(r1, c1)` to `(r2, c2)`, using the same inclusion-exclusion idea:

```java
int rectSum(int[][] prefix, int r1, int c1, int r2, int c2) {
    return prefix[r2+1][c2+1] - prefix[r1][c2+1] - prefix[r2+1][c1] + prefix[r1][c1];
}
```

→ **O(rows × cols)** to build, **O(1)** per rectangle-sum query.

---

## Prefix Products / Prefix Max / Prefix Min

The same "precompute cumulative X upfront" idea generalizes to any associative operation, not just sum:

```java
// Prefix Product
int[] prefixProduct = new int[arr.length];
prefixProduct[0] = arr[0];
for (int i = 1; i < arr.length; i++) {
    prefixProduct[i] = prefixProduct[i - 1] * arr[i];
}

// Prefix Max
int[] prefixMax = new int[arr.length];
prefixMax[0] = arr[0];
for (int i = 1; i < arr.length; i++) {
    prefixMax[i] = Math.max(prefixMax[i - 1], arr[i]);
}
```

> **Watch out ⭐:** Prefix **sum** supports the subtraction trick for O(1) range queries because addition has an inverse (subtraction). Prefix **max/min** does **not** support this trick directly — you can't "un-max" a range — so range max/min queries typically need a different structure (like a Sparse Table or Segment Tree) for O(1)/O(log n) queries.

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Build 1D prefix array | O(n) |
| 1D range sum query | O(1) |
| Build 2D prefix array | O(rows × cols) |
| 2D rectangle sum query | O(1) |
| Naive range sum (no prefix array) | O(n) per query |

---

## Quick Revision

- **Prefix Array** → precomputed cumulative sums, O(n) build, O(1) range-sum queries.
- `sum(l, r) = prefix[r] - prefix[l-1]` (0-indexed) — or use the 1-indexed convention to avoid edge cases.
- **2D Prefix Sum** → inclusion-exclusion to avoid double-counting the overlap.
- The prefix trick generalizes to product/count, but **not directly to max/min** (no inverse operation).
- Use whenever a static array faces **many repeated range queries**.

---

## Interview Definition ⭐

> **A prefix array precomputes cumulative values (typically sums) across an array in O(n), enabling any range query to be answered in O(1) via subtraction — the same idea extends to 2D matrices using inclusion-exclusion, and generalizes to any operation with an inverse, though not to max/min, which lack one.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) | Easy | The textbook 1D prefix sum application |
| 2 | [Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/) | Medium | The 2D prefix sum / inclusion-exclusion application |
| 3 | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | Medium | Prefix sum combined with a HashMap for O(n) counting |
| 4 | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) | Medium | Prefix product + suffix product technique |
| 5 | [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/) | Medium | Prefix sum + modulo + HashMap |
| 6 | [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/) | Easy | Prefix sum used to compare left-sum vs right-sum |
| 7 | [Maximum Size Subarray Sum Equals k](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/) | Medium | Prefix sum + HashMap, tracking earliest index |
