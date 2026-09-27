# Sorting and Custom Sorting - Java

> **📅 Written:** 24 Sep 2026

### Definition

**Sorting is the process of arranging elements in a specific order — ascending, descending, or by a custom rule — and is one of the most fundamental building blocks for solving array/string problems efficiently.**

**In one sentence:**
> Sorting first often turns an ugly O(n²) brute-force problem into a clean O(n log n) one, because sorted data lets you use two pointers, binary search, or greedy scanning instead of comparing everything to everything.

---

## Built-in Sorting in Java

```java
int[] arr = {5, 2, 8, 1};
Arrays.sort(arr);                          // ascending, in-place -> [1, 2, 5, 8]

Integer[] boxed = {5, 2, 8, 1};
Arrays.sort(boxed, Collections.reverseOrder()); // descending -> [8, 5, 2, 1]

List<Integer> list = new ArrayList<>(List.of(5, 2, 8, 1));
Collections.sort(list);                    // ascending, in-place
```

> **Watch out ⭐:** `Arrays.sort()` on a **primitive** array (`int[]`) has no comparator overload — it only sorts ascending. To sort descending, you must box to `Integer[]` first, since comparators only work on objects.

### What Algorithm Does Java Actually Use?

| Data Type | Algorithm | Time Complexity |
|---|---|---|
| Primitives (`int[]`, `double[]`, …) | **Dual-Pivot Quicksort** | O(n log n) avg, O(n²) worst (rare) |
| Objects (`Integer[]`, `List<T>`, …) | **TimSort** (hybrid Merge Sort + Insertion Sort) | O(n log n) worst case, **stable** |

> **Interview point ⭐:** Object sorting in Java is **stable** (equal elements keep their relative order) — primitive sorting is **not stable**, but since primitives have no identity beyond their value, stability doesn't matter for them anyway.

---

## Custom Sorting with Comparator

```java
List<String> words = new ArrayList<>(List.of("banana", "apple", "fig"));

// Sort by length, ascending
words.sort((a, b) -> a.length() - b.length());

// Same thing, using Comparator.comparing (more readable)
words.sort(Comparator.comparing(String::length));

// Descending
words.sort(Comparator.comparing(String::length).reversed());

// Multi-level sort: by length, then alphabetically for ties
words.sort(Comparator.comparing(String::length).thenComparing(Comparator.naturalOrder()));
```

### Sorting Custom Objects

```java
class Person {
    String name;
    int age;
    Person(String name, int age) { this.name = name; this.age = age; }
}

List<Person> people = new ArrayList<>();
// Sort by age ascending
people.sort((p1, p2) -> p1.age - p2.age);
// Sort by age, then name
people.sort(Comparator.comparingInt((Person p) -> p.age).thenComparing(p -> p.name));
```

> **Watch out ⭐:** For custom `Comparator` lambdas comparing numeric fields directly (`p1.age - p2.age`), watch for **integer overflow** if the values could be very large/negative — prefer `Integer.compare(a, b)` for safety.

---

## `Comparable` vs `Comparator` ⭐

| `Comparable` | `Comparator` |
|---|---|
| Implemented **inside** the class itself (`compareTo`) | Defined **externally**, as a separate object/lambda |
| Defines the class's **natural/default** ordering | Defines **custom, situational** orderings |
| One implementation per class | Many different comparators can exist for the same class |
| `class Person implements Comparable<Person>` | `Comparator<Person> byAge = ...` |

```java
class Person implements Comparable<Person> {
    int age;
    @Override
    public int compareTo(Person other) {
        return this.age - other.age;   // defines natural ordering
    }
}
// Now Collections.sort(people) works directly, using this natural order
```

---

## Sorting Algorithms — What to Know Conceptually

| Algorithm | Time (avg) | Time (worst) | Space | Stable? |
|---|---|---|---|---|
| Bubble Sort | O(n²) | O(n²) | O(1) | ✅ |
| Selection Sort | O(n²) | O(n²) | O(1) | ❌ |
| Insertion Sort | O(n²) | O(n²) | O(1) | ✅ — fast for near-sorted data |
| Merge Sort | O(n log n) | O(n log n) | O(n) | ✅ |
| Quick Sort | O(n log n) | O(n²) | O(log n) | ❌ |
| Heap Sort | O(n log n) | O(n log n) | O(1) | ❌ |
| Counting Sort | O(n + k) | O(n + k) | O(k) | ✅ — only for small integer ranges |

```java
// Merge Sort - core merge step
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

> **Interview point ⭐:** You rarely hand-implement sorting in interviews (use `Arrays.sort`/`Collections.sort`), but **Merge Sort** and **Quick Sort** internals get asked directly fairly often, and the *merge step* of Merge Sort specifically reappears in problems like "count inversions" and "merge k sorted lists".

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| `Arrays.sort(int[])` | O(n log n) avg, O(n²) rare worst case |
| `Arrays.sort(Object[])` / `Collections.sort` | O(n log n) worst case (TimSort) |
| Custom comparator sort | O(n log n) (same algorithm, extra comparator overhead) |
| Counting Sort (bounded range k) | O(n + k) |

---

## Quick Revision

- **Primitives** → Dual-Pivot Quicksort, no comparator support (box to sort descending).
- **Objects** → TimSort, O(n log n) worst case, **stable**.
- **`Comparable`** → natural order, defined inside the class (`compareTo`).
- **`Comparator`** → custom order, defined externally, supports `.reversed()` / `.thenComparing()`.
- Sorting first is a common first move to unlock two-pointer / greedy / binary-search techniques.
- Know Merge Sort's merge step — it reappears in several classic problems.

---

## Interview Definition ⭐

> **Sorting arranges elements by a defined order — natural (via `Comparable`) or custom (via `Comparator`) — and in Java, primitive arrays use Dual-Pivot Quicksort while objects use a stable TimSort, both averaging O(n log n); sorting is frequently the first step that transforms a brute-force problem into one solvable with two pointers, binary search, or greedy logic.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Sort Colors](https://leetcode.com/problems/sort-colors/) | Medium | Dutch National Flag — single-pass sort for 3 distinct values |
| 2 | [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) | Easy | In-place merge step, foundational to Merge Sort |
| 3 | [Largest Number](https://leetcode.com/problems/largest-number/) | Medium | Custom comparator with a non-obvious ordering rule |
| 4 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Medium | Sort-then-scan pattern |
| 5 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | Medium | Sorting vs Quickselect vs Heap — great comparator/complexity discussion |
| 6 | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | Medium | Custom sorting + greedy scheduling |
| 7 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | Hard | Merge Sort's merge step used for inversion counting |
