# List and ArrayList in Java

> **📅 Written:** 24 Sep 2026

### Definition

**`List` is an interface in Java representing an ordered, index-based collection that allows duplicates. `ArrayList` is the most common class that implements it — a dynamically resizable array.**

**In one sentence:**
> Think of `ArrayList` as a Java array that grows and shrinks on its own — you get all the array-like indexing, minus the fixed-size headache.

---

## `List` vs `ArrayList` ⭐

- **`List`** is an **interface**, not a class — you **cannot** do `new List()`.
- **`ArrayList`** (and `LinkedList`) is a **class** that **implements** the `List` interface — that's what you actually instantiate.

```java
List<Integer> list = new ArrayList<>();   // ✅ valid
List<Integer> list2 = new LinkedList<>(); // ✅ valid - both implement List
List<Integer> list3 = new List<>();       // ❌ invalid — List is an interface
```

> **One-liner to remember:** *You program to the interface (`List`), but you create the object from a class that implements it (`ArrayList`).*

---

## Why ArrayList Over a Plain Array?

| Array | ArrayList |
|---|---|
| Fixed size | Resizable — grows automatically |
| Holds primitives (`int[]`) | Only holds objects (`Integer`, autoboxed) |
| No built-in utility methods | Rich API: `add`, `remove`, `contains`, `indexOf`… |
| Slightly faster (no boxing overhead) | Small overhead from autoboxing/unboxing |

---

## How ArrayList Grows Internally

Under the hood, `ArrayList` is backed by a regular array. When it's full and a new element is added:

1. A **new, larger array** is allocated (typically 1.5x the old capacity)
2. All existing elements are **copied over**
3. The old array is discarded

```
Capacity 4, full:      [1, 2, 3, 4]
add(5) triggers resize:
New capacity 6:         [1, 2, 3, 4, 5, _]
```

→ This resize costs **O(n)**, but it happens rarely enough that `add()` is still **O(1) amortized** on average.

---

## Declaration & Initialization

```java
List<Integer> list = new ArrayList<>();
List<Integer> list2 = new ArrayList<>(Arrays.asList(1, 2, 3));  // pre-filled
List<Integer> list3 = new ArrayList<>(100);                      // initial capacity hint
```

> **Note:** `ArrayList<int>` is **not valid** — generics only work with objects, so you must use the wrapper class `Integer`, `Double`, `Character`, etc. (Java autoboxes primitives for you automatically.)

---

## Built-in Methods / Functions

| Method | What it does | Time Complexity |
|---|---|---|
| `list.add(x)` | Appends `x` to the end | O(1) amortized |
| `list.add(i, x)` | Inserts `x` at index `i`, shifts rest right | O(n) |
| `list.get(i)` | Returns element at index `i` | O(1) |
| `list.set(i, x)` | Replaces element at index `i` | O(1) |
| `list.remove(i)` | Removes by **index**, shifts rest left | O(n) |
| `list.remove(Object x)` | Removes first occurrence by **value** | O(n) |
| `list.contains(x)` | Whether `x` exists | O(n) |
| `list.indexOf(x)` | First index of `x`, or -1 | O(n) |
| `list.size()` | Current number of elements | O(1) |
| `list.isEmpty()` | Whether the list has 0 elements | O(1) |
| `list.clear()` | Removes all elements | O(n) |
| `Collections.sort(list)` | Sorts ascending, in-place | O(n log n) |
| `Collections.sort(list, comparator)` | Sorts with custom order | O(n log n) |
| `Collections.reverse(list)` | Reverses the list in-place | O(n) |
| `Collections.max(list)` / `min(list)` | Max/min element | O(n) |
| `list.toArray()` | Converts to an `Object[]` (or typed array) | O(n) |
| `list.subList(a, b)` | View of the range `[a, b)` (backed by original!) | O(1) — but a *view*, not a copy |

```java
List<Integer> nums = new ArrayList<>(List.of(5, 3, 8, 1));
Collections.sort(nums);                       // [1, 3, 5, 8]
Collections.sort(nums, Collections.reverseOrder()); // [8, 5, 3, 1]
```

> **Watch out ⭐:** `list.remove(2)` removes the element **at index 2**, but `list.remove(Integer.valueOf(2))` removes the **value 2**. This ambiguity (int vs Integer overload) is a classic Java gotcha — `remove(int)` and `remove(Object)` are two different overloaded methods.

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| `get(i)` / `set(i, x)` | O(1) |
| `add(x)` (at end) | O(1) amortized |
| `add(i, x)` (middle/start) | O(n) |
| `remove(i)` | O(n) |
| `contains(x)` / `indexOf(x)` | O(n) |
| `sort()` | O(n log n) |

---

## ArrayList vs LinkedList ⭐

| ArrayList | LinkedList |
|---|---|
| Backed by a resizable array | Backed by doubly-linked nodes |
| O(1) random access (`get`) | O(n) access — must traverse |
| O(n) insert/delete in middle | O(1) insert/delete once node is found |
| Better cache locality (faster in practice) | More memory overhead (pointers per node) |
| Default choice for most use cases | Use when frequent insert/delete at both ends (also implements `Deque`) |

> **Interview point ⭐:** Just like plain arrays vs linked lists, `ArrayList` trades insertion/deletion speed for fast random access — and in practice, `ArrayList` wins most of the time due to better cache performance, even for "insert-heavy" workloads at small-to-medium sizes.

---

## Quick Revision

- **`List`** → interface; **`ArrayList`** → resizable-array implementation of it.
- `ArrayList` grows via **resize + copy**, giving **O(1) amortized** `add()`.
- `get`/`set` are **O(1)**; `add`/`remove` in the middle are **O(n)**.
- Only works with **objects**, not primitives — Java autoboxes for you.
- `Collections.sort()` and `Collections.reverse()` are the go-to utility methods.
- `ArrayList` generally beats `LinkedList` in practice due to cache locality, despite the "textbook" complexity story.

---

## Interview Definition ⭐

> **`ArrayList` is a resizable implementation of the `List` interface, backed internally by a plain array that automatically grows by allocating a larger array and copying elements over when full — giving O(1) amortized appends and O(1) random access, at the cost of O(n) insertion/deletion when the position isn't the last index.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Design Dynamic Array](https://leetcode.com/problems/design-dynamic-array-resizable-array/) | Medium | Build the resize-and-copy mechanism yourself — cements how ArrayList works internally |
| 2 | [Shuffle the Array](https://leetcode.com/problems/shuffle-the-array/) | Easy | Basic list manipulation and indexing |
| 3 | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | Easy | In-place list/array modification, two pointers |
| 4 | [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/) | Medium | Combines ArrayList + HashMap for O(1) operations — a great design-level ArrayList problem |
| 5 | [Design a Stack With Increment Operation](https://leetcode.com/problems/design-a-stack-with-increment-operation/) | Medium | Using ArrayList as the backing structure for a custom data structure |
