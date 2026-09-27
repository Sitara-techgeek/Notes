# Arrays - A Linear Data Structure

> **📅 Written:** 24 Sep 2026

### Definition

**An Array is a linear data structure that stores elements of the same type in contiguous memory locations, each accessible directly using an index.**

**In one sentence:**
> An array is a row of same-sized boxes sitting right next to each other in memory, numbered from 0, so you can jump straight to any box if you know its number.

**Example:**

```
Index:   0    1    2    3    4
       +----+----+----+----+----+
Value: | 10 | 20 | 30 | 40 | 50 |
       +----+----+----+----+----+
```

---

## Key Features of Arrays

- Elements are stored in **contiguous** memory locations
- **Fixed size** in Java — declared once, can't grow/shrink (use `ArrayList` for that)
- **Homogeneous** — all elements must be of the same type
- **Random access** — any element reachable in O(1) using its index
- Index-based, **0-indexed** in Java

---

## Declaration & Memory Model

```java
int[] arr = new int[5];              // declaration + allocation, default values (0 for int)
int[] arr2 = {10, 20, 30, 40, 50};   // declaration + initialization
```

Because the memory is contiguous, the address of `arr[i]` is calculated directly:

```
address(arr[i]) = base_address + i * size_of(element_type)
```

This is *why* random access is O(1) — no traversal needed, just arithmetic.

```
base -----> [10][20][30][40][50]
             i=0  i=1  i=2  i=3  i=4
```

---

## Array Operations

### 1. Access

```java
int x = arr[2];   // 30
```

→ **O(1)** — direct address calculation, no traversal.

---

### 2. Traversal

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

→ **O(n)** — must visit every element once.

---

### 3. Search (Linear)

```java
int target = 30;
for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) return i;
}
return -1;
```

→ **O(n)** for an unsorted array. Drops to **O(log n)** with Binary Search if the array is sorted.

---

### 4. Insertion

Since Java arrays are **fixed-size**, "insertion" really means shifting elements within existing bounds (or copying into a new, bigger array).

```
Before: [10, 20, 40, 50, _]
Insert 30 at index 2:
Shift [40, 50] one step right -> [10, 20, _, 40, 50]
Place 30 at index 2           -> [10, 20, 30, 40, 50]
```

```java
// Insert val at index idx, assuming arr has a free trailing slot
for (int i = arr.length - 1; i > idx; i--) {
    arr[i] = arr[i - 1];
}
arr[idx] = val;
```

→ **O(n)** worst case (insert at the beginning shifts everything). **O(1)** if inserting at the end with room to spare.

---

### 5. Deletion

```
Before: [10, 20, 30, 40, 50]
Delete index 2 (value 30):
Shift [40, 50] one step left -> [10, 20, 40, 50, _]
```

```java
for (int i = idx; i < arr.length - 1; i++) {
    arr[i] = arr[i + 1];
}
```

→ **O(n)** worst case, same shifting logic as insertion.

---

## Time Complexity — Quick Table

| Operation              | Time Complexity |
| ----------------------- | ---------------- |
| Access by index          | O(1)             |
| Traversal                | O(n)             |
| Search (unsorted)        | O(n)             |
| Search (sorted, binary)  | O(log n)         |
| Insert at end (space free) | O(1)           |
| Insert at beginning/middle | O(n)           |
| Delete at end            | O(1)             |
| Delete at beginning/middle | O(n)           |

---

## Arrays vs ArrayList ⭐

| Array                          | ArrayList                              |
| ------------------------------- | ---------------------------------------- |
| Fixed size                      | Resizable (grows dynamically)            |
| Can hold primitives (`int[]`)   | Only holds objects (`Integer`, autoboxed)|
| Slightly faster (no boxing)     | Extra overhead from boxing/unboxing      |
| No built-in utility methods     | Rich API (`add`, `remove`, `contains`…)  |

> **Interview point ⭐:** Arrays are the right choice when the size is known upfront and performance matters; `ArrayList` wins when the size is dynamic and convenience matters.

---

## Common Patterns Built on Arrays

- **Two Pointers / Sliding Window** — exploit contiguous indices to avoid nested loops
- **Prefix Sum** — precompute cumulative sums for O(1) range-sum queries
- **Kadane's Algorithm** — max subarray sum in O(n)
- **In-place reversal / rotation** — swap using two pointers without extra space

*(These get their own dedicated notes — this section is just to flag that Arrays are the foundation almost every array-based technique sits on.)*

---

## Built-in Methods / Functions

| Method | What it does | Example |
|---|---|---|
| `arr.length` | Field (not a method!) giving the array's size | `int n = arr.length;` |
| `Arrays.sort(arr)` | Sorts in ascending order, in-place — Dual-Pivot Quicksort for primitives → **O(n log n)** | `Arrays.sort(arr);` |
| `Arrays.sort(arr, l, r)` | Sorts only the range `[l, r)` | `Arrays.sort(arr, 1, 4);` |
| `Arrays.sort(arr, Collections.reverseOrder())` | Descending sort — **only works on object arrays** (`Integer[]`, not `int[]`) | `Arrays.sort(boxed, Collections.reverseOrder());` |
| `Arrays.fill(arr, val)` | Fills every slot with `val` | `Arrays.fill(arr, -1);` |
| `Arrays.equals(a, b)` | Checks if two arrays have identical elements in the same order | `Arrays.equals(a, b);` |
| `Arrays.copyOf(arr, newLen)` | Returns a resized copy (truncated or zero/null-padded) | `int[] b = Arrays.copyOf(arr, 10);` |
| `Arrays.copyOfRange(arr, from, to)` | Copies a sub-range `[from, to)` | `Arrays.copyOfRange(arr, 1, 4);` |
| `Arrays.toString(arr)` | Human-readable string, e.g. `[1, 2, 3]` — used constantly for debugging | `System.out.println(Arrays.toString(arr));` |
| `Arrays.asList(arr)` | View of the array as a `List` — **fixed-size**, backed by the array (no `add`/`remove`) | `List<Integer> l = Arrays.asList(boxedArr);` |
| `Arrays.binarySearch(arr, key)` | Binary search on a **sorted** array → **O(log n)**, returns index or negative insertion point | `Arrays.binarySearch(arr, 30);` |
| `Arrays.stream(arr)` | Converts to an `IntStream`/`Stream<T>` for functional-style ops (`.sum()`, `.max()`, `.map()`…) | `int sum = Arrays.stream(arr).sum();` |

> **Watch out ⭐:** `Arrays.sort()` on a primitive array (`int[]`) always sorts ascending — there's no comparator overload for primitives. To sort descending, either box to `Integer[]` first, or reverse the array manually after sorting.

---

## Quick Revision

- **Array** → Fixed-size, contiguous, same-type elements, index-based.
- **Access** → O(1) via direct address math.
- **Traversal/Search (unsorted)** → O(n).
- **Insert/Delete** → O(1) at the end (if space free), O(n) elsewhere due to shifting.
- **Array vs ArrayList** → fixed vs dynamic size, primitives vs objects.

---

## Interview Definition ⭐

> **An array is a fixed-size, linear collection of same-type elements stored in contiguous memory, offering O(1) random access via index arithmetic, but O(n) insertion/deletion since shifting elements is required to preserve contiguity.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) | Easy | The canonical array + hashing warm-up |
| 2 | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | Easy | Single-pass array scan, greedy tracking of min |
| 3 | [Move Zeroes](https://leetcode.com/problems/move-zeroes/) | Easy | In-place two-pointer rearrangement |
| 4 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | Medium | Kadane's Algorithm — a must-know pattern |
| 5 | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) | Medium | Prefix/suffix product technique, no division |
| 6 | [Rotate Array](https://leetcode.com/problems/rotate-array/) | Medium | In-place rotation using reversal trick |
| 7 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Medium | Sort + linear scan over an array of ranges |
| 8 | [3Sum](https://leetcode.com/problems/3sum/) | Medium | Sorting + two pointers, very common interview ask |
| 9 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | Medium | Two-pointer optimization over brute force |
| 10 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | Hard | Prefix max / suffix max, a classic hard-array problem |
