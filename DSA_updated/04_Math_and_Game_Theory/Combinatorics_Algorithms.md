# Combinatorics - Algorithms

> **📅 Written:** 24 Sep 2026

### Definition

**While Combinatorics Basics covers the counting formulas, this note covers the algorithms used to actually *generate* permutations, combinations, and subsets in code — almost universally done via Backtracking.**

**In one sentence:**
> Backtracking is a systematic "try, recurse, undo" way of exploring every branch of possibilities — build a partial answer, recurse deeper, and when a branch is exhausted, undo the last choice and try the next one.

*(Builds directly on the formulas from Combinatorics Basics.)*

---

## The Backtracking Template

```java
void backtrack(/* state */, List<Integer> current, List<List<Integer>> result) {
    if (/* base case: current is a complete valid answer */) {
        result.add(new ArrayList<>(current));   // must COPY - current keeps changing
        return;
    }
    for (/* each choice available at this point */) {
        current.add(choice);         // make a choice
        backtrack(/* updated state */, current, result);  // recurse deeper
        current.remove(current.size() - 1);   // undo the choice (backtrack!)
    }
}
```

> **Watch out ⭐:** Forgetting `new ArrayList<>(current)` when adding to `result` is one of the most common backtracking bugs — without the copy, every entry in `result` ends up pointing to the **same** mutable list, which gets emptied by the later `remove()` calls, silently corrupting all previously "saved" answers.

---

## Generating All Subsets

Each element has two choices at every step: **include it** or **don't**.

```java
List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

void backtrack(int[] nums, int index, List<Integer> current, List<List<Integer>> result) {
    if (index == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    // Choice 1: exclude nums[index]
    backtrack(nums, index + 1, current, result);

    // Choice 2: include nums[index]
    current.add(nums[index]);
    backtrack(nums, index + 1, current, result);
    current.remove(current.size() - 1);   // undo
}
```
→ **O(2ⁿ · n)** — 2ⁿ subsets, each taking O(n) to copy into the result.

```
        {}
       /  \
    (skip1)(take1)
     {}    {1}
    / \    / \
  {} {2} {1} {1,2}
  ...continues for each element...
```

---

## Generating All Permutations

Every element must be used exactly once, in every possible order — track which elements are already used.

```java
List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, new ArrayList<>(), new boolean[nums.length], result);
    return result;
}

void backtrack(int[] nums, List<Integer> current, boolean[] used, List<List<Integer>> result) {
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;         // skip already-used elements
        used[i] = true;
        current.add(nums[i]);
        backtrack(nums, current, used, result);
        current.remove(current.size() - 1);   // undo
        used[i] = false;                        // undo
    }
}
```
→ **O(n! · n)** — n! permutations, each taking O(n) to build.

### Handling Duplicates in Permutations

Sort first, then skip a duplicate value **at the same recursion depth** if its identical predecessor wasn't used (prevents generating the same permutation twice).

```java
Arrays.sort(nums);
// inside the loop:
if (used[i]) continue;
if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;   // skip duplicate at this level
```

---

## Generating All Combinations (Size k)

Similar to subsets, but fixed size `k`, and typically enforce an increasing index order to avoid duplicate combinations (e.g. `[1,2]` and `[2,1]` should count as the same combination, so only generate one of them).

```java
List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(1, n, k, new ArrayList<>(), result);
    return result;
}

void backtrack(int start, int n, int k, List<Integer> current, List<List<Integer>> result) {
    if (current.size() == k) {
        result.add(new ArrayList<>(current));
        return;
    }
    for (int i = start; i <= n; i++) {
        current.add(i);
        backtrack(i + 1, n, k, current, result);   // start from i+1, never go backward
        current.remove(current.size() - 1);
    }
}
```
→ **O(C(n, k) · k)**.

> **Interview point ⭐:** The `start` parameter (only ever moving forward, never revisiting earlier indices) is precisely what enforces "order doesn't matter" for combinations — it's the single-line difference between generating combinations vs. generating permutations of the same size.

---

## Combination Sum (Elements Can Repeat)

A variant where the **same element can be reused** — don't advance past the current index when recursing.

```java
List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(candidates, target, 0, new ArrayList<>(), result);
    return result;
}

void backtrack(int[] candidates, int remaining, int start, List<Integer> current, List<List<Integer>> result) {
    if (remaining == 0) {
        result.add(new ArrayList<>(current));
        return;
    }
    if (remaining < 0) return;   // prune - this branch can't work
    for (int i = start; i < candidates.length; i++) {
        current.add(candidates[i]);
        backtrack(candidates, remaining - candidates[i], i, current, result);  // NOT i+1 - reuse allowed
        current.remove(current.size() - 1);
    }
}
```

---

## Pruning — Making Backtracking Practical

Raw backtracking explores every branch; **pruning** cuts off branches early once they're provably invalid, which is often what makes an exponential-looking solution actually run in time on real constraints.

```java
if (remaining < 0) return;   // sum-based pruning — no point continuing
if (current.size() > k) return;   // size-based pruning
```

> **Interview point ⭐:** The formulas from Combinatorics Basics tell you the **theoretical count** of outcomes; pruning is what keeps the **actual runtime** manageable by cutting invalid branches before fully exploring them — both matter together in practice.

---

## Time Complexity — Quick Table

| Generation Task | Time Complexity |
|---|---|
| All subsets | O(2ⁿ · n) |
| All permutations | O(n! · n) |
| All combinations of size k | O(C(n,k) · k) |
| Combination Sum (with repetition, pruned) | Depends on target/candidates, generally exponential but well-pruned in practice |

---

## Quick Revision

- **Backtracking** → choose, recurse, undo — the standard engine behind all combinatorial generation.
- **Always copy** (`new ArrayList<>(current)`) when saving a result — a classic and very common bug otherwise.
- **Subsets** → include/exclude choice at each index.
- **Permutations** → track `used[]`, every element must appear exactly once.
- **Combinations** → `start` parameter moves only forward, enforcing "order doesn't matter".
- **Combination Sum (repetition allowed)** → recurse with `i`, not `i+1`.
- **Pruning** → cut invalid branches early (`if (remaining < 0) return;`) to keep runtime practical.

---

## Interview Definition ⭐

> **Combinatorial generation algorithms use backtracking — a systematic choose/recurse/undo exploration of every branch — to enumerate subsets (include/exclude choices), permutations (tracking used elements), and combinations (a forward-only start index preventing reordering duplicates), with pruning used to cut off invalid branches early and keep otherwise-exponential runtimes practical.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Subsets](https://leetcode.com/problems/subsets/) | Medium | The foundational include/exclude backtracking template |
| 2 | [Subsets II](https://leetcode.com/problems/subsets-ii/) | Medium | Subset generation with duplicate handling |
| 3 | [Permutations](https://leetcode.com/problems/permutations/) | Medium | The foundational used[] backtracking template |
| 4 | [Permutations II](https://leetcode.com/problems/permutations-ii/) | Medium | Permutation generation with duplicate handling |
| 5 | [Combinations](https://leetcode.com/problems/combinations/) | Medium | The forward-only `start` index template |
| 6 | [Combination Sum](https://leetcode.com/problems/combination-sum/) | Medium | Combinations with element reuse allowed |
| 7 | [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/) | Medium | No reuse, but with duplicate candidates to skip |
| 8 | [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) | Medium | Backtracking over a different kind of choice space (character sets per digit) |
| 9 | [N-Queens](https://leetcode.com/problems/n-queens/) | Hard | Backtracking with heavy pruning — a great synthesis problem |
