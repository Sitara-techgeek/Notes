# Combinatorics - Basics

> **📅 Written:** 24 Sep 2026

### Definition

**Combinatorics is the branch of math concerned with counting — how many ways can something be arranged, selected, or distributed — forming the foundation for probability-flavored and "how many ways" style interview questions.**

**In one sentence:**
> Before writing any code, combinatorics answers "how many possibilities are we even dealing with," which is often the number itself the problem wants, or a huge hint about what time complexity is feasible.

---

## The Two Fundamental Counting Principles

### Rule of Sum (OR)

If a task can be done in `m` ways **or** `n` ways (mutually exclusive), the total is `m + n`.

```
Choosing a fruit OR a vegetable: 3 fruits + 4 vegetables = 7 total choices
```

### Rule of Product (AND)

If a task consists of steps that can be done in `m` ways **and then** `n` ways (independently), the total is `m × n`.

```
Choosing a shirt AND pants: 4 shirts × 3 pants = 12 outfit combinations
```

> **Interview point ⭐:** Almost every combinatorics problem is really "is this an OR situation (add the counts) or an AND situation (multiply the counts)?" — nailing this distinction first prevents the wrong formula from ever being reached for.

---

## Permutations — Order Matters

### Definition

The number of ways to **arrange** `r` items chosen from `n` distinct items, where **order matters**.

```
P(n, r) = n! / (n - r)!
```

```
Arranging 3 people out of 5 in a line:
P(5, 3) = 5! / 2! = 5 × 4 × 3 = 60
```

```java
long permutations(int n, int r) {
    long result = 1;
    for (int i = 0; i < r; i++) {
        result *= (n - i);
    }
    return result;
}
```

### Special Case: Arranging All n Items

```
P(n, n) = n!  (simply n factorial)
```

### Permutations with Repetition Allowed

If items **can repeat** and order matters (e.g. a 4-digit PIN):
```
n^r  possibilities  (n choices, made independently r times)
```

---

## Combinations — Order Doesn't Matter

### Definition

The number of ways to **select** `r` items from `n` distinct items, where **order does NOT matter** — this is the famous "nCr".

```
C(n, r) = n! / (r! × (n - r)!)
```

```
Choosing 3 people out of 5 for a committee (order doesn't matter):
C(5, 3) = 5! / (3! × 2!) = 10
```

```java
long combinations(int n, int r) {
    if (r > n - r) r = n - r;   // exploit symmetry: C(n, r) == C(n, n-r), fewer multiplications
    long result = 1;
    for (int i = 0; i < r; i++) {
        result = result * (n - i) / (i + 1);   // interleave multiply/divide to avoid huge intermediates
    }
    return result;
}
```

> **Interview point ⭐:** The single question that separates permutations from combinations: *"if I picked the same items in a different order, would that count as a different outcome?"* Yes → permutation. No → combination. This is the most common source of confusion in combinatorics problems.

### Relationship Between Permutations and Combinations

```
P(n, r) = C(n, r) × r!

(Choosing r items, THEN arranging them, gives all possible orderings)
```

---

## Pascal's Triangle — Building nCr Without Factorials

Each entry is the sum of the two entries above it — directly encodes `C(n, r)` at row `n`, position `r`.

```
Row 0:         1
Row 1:        1 1
Row 2:       1 2 1
Row 3:      1 3 3 1
Row 4:     1 4 6 4 1
```

```java
int[][] pascalsTriangle(int numRows) {
    int[][] triangle = new int[numRows][];
    for (int i = 0; i < numRows; i++) {
        triangle[i] = new int[i + 1];
        triangle[i][0] = triangle[i][i] = 1;
        for (int j = 1; j < i; j++) {
            triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j];
        }
    }
    return triangle;
}
```
→ **O(n²)** to build the full triangle — useful when many `C(n, r)` values are needed and factorial overflow is a concern.

---

## Pigeonhole Principle

### Definition

If `n` items are placed into `m` containers, and `n > m`, then **at least one container must hold more than one item**.

```
10 pigeons, 9 holes -> at least one hole has ≥ 2 pigeons, guaranteed, no matter the arrangement.
```

> **Interview point ⭐:** This shows up disguised as "prove that two elements must share a property" style problems — e.g. "prove that among any 13 people, at least two share a birth month" (13 people, 12 months → pigeonhole guarantees a collision). It's a proof technique, not a formula, and is easy to miss because it doesn't look like "counting" at first glance.

---

## Combinations With Repetition Allowed ("Stars and Bars")

Choosing `r` items from `n` types **with repetition allowed**, order doesn't matter (e.g. "how many ways to choose 5 fruits from 3 types, with repeats allowed"):

```
C(n + r - 1, r)
```

```
Choosing 5 fruits from {apple, banana, cherry} type, repeats OK:
C(3 + 5 - 1, 5) = C(7, 5) = 21
```

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Compute P(n, r) or C(n, r) directly | O(r) |
| Build Pascal's Triangle up to row n | O(n²) |
| Precompute factorials up to n (for repeated queries) | O(n) build, O(1) lookup per query |

---

## Quick Revision

- **Rule of Sum** → OR situations, add counts. **Rule of Product** → AND situations, multiply counts.
- **Permutation** `P(n,r) = n!/(n-r)!` → order **matters**.
- **Combination** `C(n,r) = n!/(r!(n-r)!)` → order **doesn't matter**.
- **P(n,r) = C(n,r) × r!** — pick, then arrange.
- **Pascal's Triangle** → builds nCr values via addition, avoiding factorial overflow.
- **Pigeonhole Principle** → n items into m containers, n > m guarantees a collision — a proof tool, not a formula.
- **Stars and Bars** → `C(n+r-1, r)` for combinations allowing repetition.

---

## Interview Definition ⭐

> **Combinatorics counts arrangements and selections using two core principles — sum for OR (mutually exclusive) situations and product for AND (independent, sequential) situations — with permutations counting ordered arrangements as n!/(n-r)! and combinations counting unordered selections as n!/(r!(n-r)!), related by P(n,r) = C(n,r)×r!, while the pigeonhole principle serves as a separate proof technique guaranteeing collisions whenever more items exist than containers.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/) | Easy | Direct construction of the nCr triangle |
| 2 | [Permutations](https://leetcode.com/problems/permutations/) | Medium | Generating actual permutations (backtracking), grounding the P(n,r) formula |
| 3 | [Combinations](https://leetcode.com/problems/combinations/) | Medium | Generating actual combinations (backtracking), grounding the C(n,r) formula |
| 4 | [Unique Paths](https://leetcode.com/problems/unique-paths/) | Medium | A grid-path counting problem solvable directly via nCr |
| 5 | [Count Vowel Strings](https://leetcode.com/problems/count-vowel-strings/) | Medium | Stars-and-bars combinations with repetition |
