# Pre-Computation

> **📅 Written:** 24 Sep 2026

### Definition

**Pre-computation is the general strategy of doing extra work upfront — before the "real" queries or operations begin — so that each individual query afterward becomes cheap, often O(1) or O(log n).**

**In one sentence:**
> If you're going to be asked the same *type* of question many times, do the expensive thinking once ahead of time, and just look up the answer every time after that.

---

## The Core Trade-off

Pre-computation trades **space and one-time setup cost** for **faster repeated queries**.

```
Without pre-computation:         With pre-computation:
Query 1: O(n) work                Setup: O(n) or O(n log n) work, once
Query 2: O(n) work                Query 1: O(1)
Query 3: O(n) work         -->    Query 2: O(1)
   ...  (q queries)                  ...  (q queries)
Total: O(n × q)                   Total: O(n + q)
```

> **Interview point ⭐:** Whenever a problem says "answer **multiple** queries" or "the array/graph is **static**" (doesn't change between queries), that's the signal to think: *"what can I precompute once to make each query cheap?"*

---

## Common Pre-Computation Techniques

| Technique | What it precomputes | Query Time |
|---|---|---|
| **Prefix Sum** | Cumulative sums | O(1) range sum |
| **Suffix Sum** | Cumulative sums from the end | O(1) suffix-based queries |
| **Frequency Array / Count Array** | Counts of each value | O(1) count lookup |
| **Sieve of Eratosthenes** | Primality of every number up to N | O(1) primality check |
| **Factorial Table (with modulo)** | n! for all n up to N | O(1) factorial lookup (used heavily in combinatorics) |
| **Power Table (Fast Exponentiation cache)** | powers of a base | O(1) power lookup |
| **Sparse Table** | Range min/max for all power-of-2-sized windows | O(1) range min/max (static array) |
| **Memoization tables (DP)** | Subproblem answers | O(1) lookup of previously solved subproblems |
| **Adjacency/Distance precompute (Floyd-Warshall)** | All-pairs shortest paths | O(1) distance lookup between any two nodes |

---

## Worked Example: Suffix Sum

The mirror image of a prefix sum — cumulative sum **from the end**.

```java
int[] arr = {3, 1, 4, 1, 5};
int[] suffix = new int[arr.length];
suffix[arr.length - 1] = arr[arr.length - 1];
for (int i = arr.length - 2; i >= 0; i--) {
    suffix[i] = suffix[i + 1] + arr[i];
}
// suffix = [14, 11, 10, 6, 5]
```

Useful whenever a problem needs both "everything before index i" **and** "everything after index i" — like Product of Array Except Self, or checking if a "split point" balances the array.

---

## Worked Example: Sieve of Eratosthenes

Precomputes primality for **every number up to N** in one pass, instead of checking each number individually with trial division.

```java
boolean[] isComposite = new boolean[N + 1];
for (int i = 2; (long) i * i <= N; i++) {
    if (!isComposite[i]) {
        for (int j = i * i; j <= N; j += i) {
            isComposite[j] = true;
        }
    }
}
// isComposite[x] == false  ->  x is prime
```

→ **O(N log log N)** to build the whole sieve, vs. **O(√x)** for each individual trial-division check — a massive win when checking primality of many numbers up to N.

---

## Worked Example: Factorial Table for Combinatorics

Precomputing factorials (usually with a modulo, since factorials grow huge fast) makes combinatorics formulas like nCr instant to compute.

```java
long MOD = 1_000_000_007L;
long[] fact = new long[N + 1];
fact[0] = 1;
for (int i = 1; i <= N; i++) {
    fact[i] = (fact[i - 1] * i) % MOD;
}
// nCr(n, r) can now be computed in O(log MOD) using fact[] + modular inverse,
// instead of recomputing factorials from scratch every time.
```

---

## When Pre-Computation Doesn't Help

- **Single query, static data** — if you only need the answer once, precomputing everything upfront is wasted work; just compute directly.
- **Data changes frequently (dynamic)** — a plain prefix array breaks the moment the underlying array is updated, since you'd need to rebuild it (O(n)) after every update. For frequently-updated data with range queries, look at **Fenwick Trees (BIT)** or **Segment Trees** instead, which support O(log n) updates *and* O(log n) queries.

> **Interview point ⭐:** "Static array + many range queries" → prefix sums / sparse table. "Array with updates + many range queries" → Fenwick Tree / Segment Tree. Recognizing which bucket a problem falls into is often the whole battle.

---

## Time Complexity — Quick Table

| Technique | Build Time | Query Time |
|---|---|---|
| Prefix/Suffix Sum | O(n) | O(1) |
| Sieve of Eratosthenes | O(N log log N) | O(1) per check |
| Factorial Table | O(N) | O(1) lookup, O(log MOD) for nCr |
| Sparse Table (range min/max) | O(n log n) | O(1) |
| DP Memoization Table | Depends on state space | O(1) per repeated subproblem |

---

## Quick Revision

- **Pre-computation** → do expensive work once, upfront, to make repeated queries cheap.
- Reach for it when a problem involves **many queries on static data**.
- Prefix/suffix sums, sieves, factorial tables, sparse tables, and memoization are all instances of the same idea.
- If the data **changes** between queries, plain pre-computation breaks — use Fenwick Tree / Segment Tree instead.

---

## Interview Definition ⭐

> **Pre-computation is the strategy of investing one-time upfront work to build a lookup structure — such as prefix sums, a sieve, or a memoization table — so that each subsequent query can be answered in O(1) or O(log n) instead of recomputing from scratch; it applies specifically to static data with repeated queries, and breaks down once the data becomes mutable, at which point Fenwick/Segment Trees take over.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) | Easy | Prefix sum as pre-computation |
| 2 | [Count Primes](https://leetcode.com/problems/count-primes/) | Medium | Sieve of Eratosthenes, the classic pre-computation algorithm |
| 3 | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) | Medium | Prefix + suffix pre-computation together |
| 4 | [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/) | Medium | Shows *why* plain pre-computation fails with updates — leads into Fenwick Tree |
| 5 | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | Easy | DP table as a pre-computation / memoization example |
| 6 | [Unique Paths](https://leetcode.com/problems/unique-paths/) | Medium | 2D DP table pre-computation |
