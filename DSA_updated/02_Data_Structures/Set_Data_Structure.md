# Set Data Structure

> **📅 Written:** 24 Sep 2026

### Definition

**A Set is a data structure that stores a collection of unique elements — no duplicates allowed — with no concept of index-based access.**

**In one sentence:**
> A set is a Map that only cares about the keys — it answers "is this present?" in O(1), and automatically throws away anything you try to add twice.

**Example:**

```
add(5), add(3), add(5), add(7)

Set contents: {3, 5, 7}   <- second add(5) was silently ignored
```

---

## `Set` Interface — Main Implementations

| Implementation | Ordering | Backing Structure | Typical Use |
|---|---|---|---|
| `HashSet` | **No guaranteed order** | HashMap internally (values ignored) | Default choice — fastest general-purpose set |
| `LinkedHashSet` | **Insertion order** preserved | HashMap + doubly-linked list | Predictable iteration + uniqueness |
| `TreeSet` | **Sorted order** | Red-Black Tree (backed by `TreeMap`) | Sorted uniqueness, range queries |

```java
Set<Integer> hashSet = new HashSet<>();
Set<Integer> linkedSet = new LinkedHashSet<>();
Set<Integer> treeSet = new TreeSet<>();
```

> **Interview point ⭐:** `HashSet` is literally implemented internally using a `HashMap<E, Object>`, where every value maps to a single dummy constant object — the "set" behavior comes entirely from the map's unique-key guarantee.

---

## Declaration & Basic Usage

```java
Set<Integer> set = new HashSet<>();

set.add(5);              // true - added
set.add(5);               // false - already present, ignored
set.contains(5);          // true
set.remove(5);            // removes 5
set.size();               // number of unique elements
```

---

## Built-in Methods / Functions

| Method | What it does |
|---|---|
| `set.add(x)` | Adds `x`; returns `false` if already present |
| `set.remove(x)` | Removes `x` if present |
| `set.contains(x)` | Whether `x` exists — **O(1) avg** |
| `set.size()` | Number of unique elements |
| `set.isEmpty()` | Whether the set is empty |
| `set.addAll(collection)` | Union — adds all elements from another collection |
| `set.retainAll(collection)` | Intersection — keeps only elements also in the other collection |
| `set.removeAll(collection)` | Difference — removes all elements found in the other collection |
| `Collections.disjoint(a, b)` | Checks if two sets share no elements |

```java
Set<Integer> a = new HashSet<>(List.of(1, 2, 3));
Set<Integer> b = new HashSet<>(List.of(2, 3, 4));

Set<Integer> union = new HashSet<>(a);
union.addAll(b);              // {1, 2, 3, 4}

Set<Integer> intersection = new HashSet<>(a);
intersection.retainAll(b);    // {2, 3}

Set<Integer> difference = new HashSet<>(a);
difference.removeAll(b);      // {1}
```

---

## TreeSet-Specific Methods (Sorted Set)

```java
TreeSet<Integer> treeSet = new TreeSet<>(List.of(10, 5, 20, 15));

treeSet.first();          // 5   - smallest
treeSet.last();           // 20  - largest
treeSet.ceiling(12);      // 15  - smallest element >= 12
treeSet.floor(12);        // 10  - largest element <= 12
treeSet.higher(10);       // 15  - smallest element > 10
treeSet.lower(10);        // 5   - largest element < 10
```

→ All of these run in **O(log n)**, since `TreeSet` is backed by a balanced BST.

---

## Time Complexity — Quick Table

| Operation | HashSet (avg) | HashSet (worst) | TreeSet |
|---|---|---|---|
| `add` / `remove` / `contains` | O(1) | O(n) | O(log n) |
| Iteration | O(n) | O(n) | O(n), sorted order |
| Get min/max | O(n) — no direct method | O(n) | O(log n) via `first()`/`last()` |

---

## Set vs List vs Map ⭐

| | List | Set | Map |
|---|---|---|---|
| Duplicates | ✅ Allowed | ❌ Not allowed | Keys unique, values can repeat |
| Order | Insertion order (index-based) | Depends on implementation | Depends on implementation |
| Access | By index | By value only (`contains`) | By key |
| Use case | Ordered sequence of items | Fast membership checks, dedup | Key → value lookups |

> **Interview point ⭐:** Reach for a `Set` whenever a problem needs **fast membership testing** or **deduplication** — it turns an O(n) "have I seen this before?" scan into an O(1) check.

---

## Quick Revision

- **Set** → unique elements, no index access, O(1) average membership check.
- **HashSet** → no order, fastest; built internally on a `HashMap`.
- **LinkedHashSet** → insertion order preserved.
- **TreeSet** → sorted order, O(log n), supports `ceiling`/`floor`/`higher`/`lower`.
- `addAll` / `retainAll` / `removeAll` give you union / intersection / difference.
- Reach for a Set whenever you need fast "have I seen this?" checks or to remove duplicates.

---

## Interview Definition ⭐

> **A Set is a collection that enforces uniqueness with no index-based access, typically implemented on top of a Map (HashSet uses a HashMap internally) — giving O(1) average membership checks, with TreeSet trading that for O(log n) operations in exchange for sorted iteration order.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) | Easy | The most direct HashSet use case |
| 2 | [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/) | Easy | Set intersection via `retainAll` logic |
| 3 | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) | Medium | HashSet used for O(n) sequence detection without sorting |
| 4 | [Single Number](https://leetcode.com/problems/single-number/) | Easy | Set-based approach (though XOR is the optimal O(1)-space trick) |
| 5 | [Happy Number](https://leetcode.com/problems/happy-number/) | Easy | HashSet used for cycle detection |
| 6 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | Medium | TreeSet/sorted-order thinking applied to interval scheduling |
