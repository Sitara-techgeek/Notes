# Searching

> **📅 Written:** 24 Sep 2026

### Definition

**Searching is the process of finding whether a target value exists in a collection, and if so, where — the two fundamental approaches are Linear Search (check everything) and Binary Search (repeatedly halve the search space on sorted data).**

**In one sentence:**
> If the data is unsorted, you have no choice but to check every element one by one — but the moment it's sorted, you can throw away half the remaining possibilities with every single comparison.

---

## 1. Linear Search

### Definition

Check every element one at a time until the target is found or the collection ends.

```java
int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

→ **O(n)** — no assumptions needed about ordering. Works on any data.

---

## 2. Binary Search

### Definition

Requires **sorted** data. Repeatedly checks the middle element, and discards the half of the search space that can't contain the target.

```
Search for 23 in: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]

Step 1: mid = 16, target > mid -> search right half
Step 2: mid = 56 (of remaining), target < mid -> search left half
Step 3: mid = 23 -> found!
```

```java
int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;   // avoids overflow vs (left+right)/2
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

→ **O(log n)** — the search space halves with every comparison.

> **Watch out ⭐:** Always write `mid = left + (right - left) / 2`, not `(left + right) / 2` — the latter can **overflow** for very large index values (a classic bug, famously found in real binary search implementations for years).

---

## Binary Search Variants

### Find First/Last Occurrence (in an array with duplicates)

```java
int findFirst(int[] arr, int target) {
    int left = 0, right = arr.length - 1, result = -1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            result = mid;
            right = mid - 1;      // keep searching LEFT for an earlier occurrence
        } else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return result;
}
```

→ Same idea for **last occurrence**, but move `left = mid + 1` on a match instead, to keep searching right.

### Lower Bound / Upper Bound

- **Lower Bound** — first index where `arr[i] >= target`
- **Upper Bound** — first index where `arr[i] > target`

```java
int lowerBound(int[] arr, int target) {
    int left = 0, right = arr.length;   // note: right = length, not length - 1
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] < target) left = mid + 1;
        else right = mid;
    }
    return left;
}
```

---

## Binary Search on the Answer ⭐

A powerful pattern: even when the input array isn't sorted, if the **answer space is monotonic** (e.g. "is X feasible?" flips from false to true past some threshold), you can binary search on the answer itself instead of the array.

```
Feasibility:  F F F F T T T T T
                       ↑
              binary search finds this boundary
```

```java
// Template: find the smallest X for which check(X) is true
int binarySearchOnAnswer(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (check(mid)) hi = mid;       // mid works, try smaller
        else lo = mid + 1;              // mid doesn't work, need bigger
    }
    return lo;
}
```

> **Interview point ⭐:** Whenever a problem asks for the "minimum X such that condition holds" or "maximum X such that condition holds", and increasing X monotonically flips the condition from false→true (or true→false), that's a strong signal to binary search on the answer instead of brute-forcing every X. *(This gets its own dedicated note with worked examples.)*

---

## Java's Built-in Binary Search

| Method | What it does |
|---|---|
| `Arrays.binarySearch(arr, key)` | Binary search on a **sorted** array; returns index, or `-(insertion point) - 1` if not found |
| `Collections.binarySearch(list, key)` | Same, for a sorted `List` |

```java
int[] arr = {2, 5, 8, 12, 16};
int idx = Arrays.binarySearch(arr, 8);    // 2
int notFound = Arrays.binarySearch(arr, 9); // negative value encoding insertion point
```

> **Watch out ⭐:** If the target isn't found, `Arrays.binarySearch` doesn't return `-1` — it returns `-(insertionPoint) - 1`, so you must check `if (idx < 0)` rather than `if (idx == -1)`.

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Linear Search | O(n) |
| Binary Search | O(log n) |
| Binary Search (first/last occurrence) | O(log n) |
| Binary Search on Answer | O(log(range) × cost of `check()`) |
| `Arrays.binarySearch` | O(log n) |

---

## Linear vs Binary Search ⭐

| Linear Search | Binary Search |
|---|---|
| Works on unsorted data | Requires sorted data |
| O(n) | O(log n) |
| Simple, no preprocessing | May need an O(n log n) sort first if data isn't already sorted |
| Better for very small or one-off searches | Better when searching repeatedly on the same sorted data |

---

## Quick Revision

- **Linear Search** → O(n), works on any data, no sorting needed.
- **Binary Search** → O(log n), requires sorted data, halves the search space each step.
- Use `left + (right - left) / 2` to avoid overflow.
- **First/last occurrence** and **lower/upper bound** are binary search variants worth memorizing directly.
- **Binary Search on Answer** — applies when the answer space is monotonic, even without a sorted array.
- `Arrays.binarySearch` returns a **negative encoded value** (not `-1`) when not found.

---

## Interview Definition ⭐

> **Searching finds whether and where a target exists in a collection — Linear Search checks every element in O(n) with no ordering requirement, while Binary Search exploits sorted order to eliminate half the remaining search space each step, achieving O(log n); the same halving idea generalizes to "Binary Search on the Answer" whenever a problem's feasibility condition is monotonic.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Binary Search](https://leetcode.com/problems/binary-search/) | Easy | The template, memorize cold |
| 2 | [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | Medium | First/last occurrence variant |
| 3 | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | Medium | Modified binary search on a "broken" sorted array — very common ask |
| 4 | [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | Medium | Another rotated-array binary search variant |
| 5 | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | Medium | Classic Binary Search on Answer problem |
| 6 | [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | Medium | Another strong Binary Search on Answer example |
| 7 | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) | Hard | Advanced binary search across two arrays — frequent "hard" interview pick |
