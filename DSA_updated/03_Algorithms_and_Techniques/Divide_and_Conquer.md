# Divide and Conquer

> **📅 Written:** 24 Sep 2026

### Definition

**Divide and Conquer is an algorithmic paradigm that solves a problem by breaking it into smaller subproblems of the same type, solving each recursively, and combining their results into the final answer.**

**In one sentence:**
> Split the problem into pieces small enough to solve trivially, solve each piece, then figure out how to stitch the pieces' answers back together — the "combine" step is usually where all the cleverness lives.

---

## The Three Steps

```
        Divide          Conquer            Combine
   [split problem]  [solve subproblems]  [merge results]

Problem
   |
   ├── Subproblem A ── (recursively divide further...)
   └── Subproblem B ── (recursively divide further...)
           |
      Combine A's and B's results into the final answer
```

1. **Divide** — split the problem into smaller subproblems (usually roughly equal halves)
2. **Conquer** — solve each subproblem recursively (base case: trivially small, solve directly)
3. **Combine** — merge the subproblem solutions into the overall answer

---

## Worked Example: Merge Sort

```
Divide:                          Combine (merge):
[5, 2, 8, 1]                     [2, 5] + [1, 8]  -> merge -> [1, 2, 5, 8]
   /        \
[5, 2]    [8, 1]
  / \       / \
[5] [2]  [8] [1]     <- base case: single element is trivially sorted
```

```java
void mergeSort(int[] arr, int left, int right) {
    if (left >= right) return;              // base case
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);              // conquer left half
    mergeSort(arr, mid + 1, right);         // conquer right half
    merge(arr, left, mid, right);           // combine
}

void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        temp[k++] = (arr[i] <= arr[j]) ? arr[i++] : arr[j++];
    }
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    System.arraycopy(temp, 0, arr, left, temp.length);
}
```
→ **O(n log n)** — log n levels of recursion, O(n) work to merge at each level.

---

## Worked Example: Quick Sort

Unlike Merge Sort, Quick Sort does the hard work in the **divide** step (partitioning around a pivot), and the **combine** step is trivial (nothing to do — the array is already sorted once both halves are).

```java
void quickSort(int[] arr, int low, int high) {
    if (low >= high) return;
    int pivotIndex = partition(arr, low, high);
    quickSort(arr, low, pivotIndex - 1);
    quickSort(arr, pivotIndex + 1, high);
}

int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr, i, j);
        }
    }
    swap(arr, i + 1, high);
    return i + 1;
}
```
→ **O(n log n) average**, **O(n²) worst case** (already-sorted input with a poor pivot choice — mitigated in practice with random/median pivots).

---

## Worked Example: Binary Search (as Divide and Conquer)

Binary search is technically Divide and Conquer with a twist — it only recurses into **one** half, not both, which is why it's O(log n) instead of O(n log n).

```java
int binarySearch(int[] arr, int target, int left, int right) {
    if (left > right) return -1;
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) return mid;
    if (arr[mid] < target) return binarySearch(arr, target, mid + 1, right);
    return binarySearch(arr, target, left, mid - 1);
}
```

---

## Analyzing Divide and Conquer — The Master Theorem (Conceptual)

The recurrence `T(n) = a·T(n/b) + O(n^d)` describes most D&C algorithms, where:
- `a` = number of subproblems
- `n/b` = size of each subproblem
- `O(n^d)` = cost of the divide + combine steps

| Algorithm | a | b | d | Result |
|---|---|---|---|---|
| Merge Sort | 2 | 2 | 1 | O(n log n) |
| Binary Search | 1 | 2 | 0 | O(log n) |
| Quick Sort (avg case) | 2 | 2 | 1 | O(n log n) |
| Naive matrix multiplication (D&C) | 8 | 2 | 2 | O(n³) |
| Strassen's matrix multiplication | 7 | 2 | 2 | O(n^2.81) |

> **Interview point ⭐:** You're rarely expected to derive the Master Theorem formally in an interview, but recognizing "this problem splits into `a` subproblems of size `n/b`, with `O(n^d)` combine cost" is a fast way to reason about a D&C algorithm's complexity on the spot.

---

## Divide and Conquer vs Dynamic Programming ⭐

| Divide and Conquer | Dynamic Programming |
|---|---|
| Subproblems are **independent** (don't overlap) | Subproblems **overlap** — same subproblem solved multiple times |
| No need to cache/memoize results | Caching (memoization) is essential to avoid recomputation |
| e.g. Merge Sort, Quick Sort, Binary Search | e.g. Fibonacci, Knapsack, Longest Common Subsequence |

> **Interview point ⭐:** The key question that separates the two: *"If I solved this exact subproblem before, would I ever need that answer again?"* If yes (overlapping subproblems) → DP. If no (independent subproblems) → plain Divide and Conquer, no caching needed.

---

## Worked Example: Maximum Subarray (D&C Approach)

While Kadane's Algorithm solves this in O(n), the D&C approach is a good exercise in combining results from two halves.

```
The max subarray either:
  1. Lies entirely in the left half
  2. Lies entirely in the right half
  3. Crosses the midpoint (combine step must check this case explicitly)
```

```java
int maxCrossingSum(int[] arr, int left, int mid, int right) {
    int leftSum = Integer.MIN_VALUE, sum = 0;
    for (int i = mid; i >= left; i--) {
        sum += arr[i];
        leftSum = Math.max(leftSum, sum);
    }
    int rightSum = Integer.MIN_VALUE;
    sum = 0;
    for (int i = mid + 1; i <= right; i++) {
        sum += arr[i];
        rightSum = Math.max(rightSum, sum);
    }
    return leftSum + rightSum;
}
```
→ **O(n log n)** — slower than Kadane's O(n), but a great illustration of the "combine step handles the case that crosses the split" idea common to many D&C problems.

---

## Time Complexity — Quick Table

| Algorithm | Time Complexity |
|---|---|
| Merge Sort | O(n log n) |
| Quick Sort (average / worst) | O(n log n) / O(n²) |
| Binary Search | O(log n) |
| Max Subarray (D&C) | O(n log n) |
| Strassen's Matrix Multiplication | O(n^2.81) |

---

## Quick Revision

- **Divide and Conquer** → Divide into subproblems, Conquer recursively, Combine results.
- Subproblems are **independent** — no overlap, no need to cache (unlike DP).
- **Merge Sort** — hard work in combine (merge step); **Quick Sort** — hard work in divide (partition step).
- **Binary Search** recurses into only **one** half — that's why it's O(log n), not O(n log n).
- The recurrence `T(n) = a·T(n/b) + O(n^d)` is the general shape to reason about D&C complexity.

---

## Interview Definition ⭐

> **Divide and Conquer solves a problem by splitting it into smaller, independent subproblems of the same type, solving each recursively, and combining their results — distinguished from Dynamic Programming by the absence of overlapping subproblems, meaning no memoization is needed; Merge Sort, Quick Sort, and Binary Search are the canonical examples, each varying in where the algorithmic work is concentrated (divide, conquer, or combine).**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Sort an Array](https://leetcode.com/problems/sort-an-array/) | Medium | Implement Merge Sort or Quick Sort from scratch |
| 2 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | Medium | Quickselect — a D&C variant of Quick Sort's partition |
| 3 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | Medium | D&C approach as an alternative to Kadane's |
| 4 | [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | Hard | D&C merging strategy (merge pairs of lists, halve repeatedly) |
| 5 | [Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/) | Medium | Classic "divide on operator, combine results" D&C problem |
| 6 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | Hard | Merge Sort's combine step adapted to count inversions |
| 7 | [Pow(x, n)](https://leetcode.com/problems/powx-n/) | Medium | Fast exponentiation via divide and conquer, O(log n) |
