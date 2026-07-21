# Set

> **📅 Written:** 21 Jul 2026

### Definition

**A Set is a data structure that stores a collection of elements with no duplicates allowed — unlike a Map, it holds standalone values rather than key-value pairs.**

**In one sentence:**
> A Set is like a bag of unique items — you can check if something's in the bag or add/remove items, but you'll never find two of the same thing inside it.

**Example:**

```
{ "apple", "banana", "cherry" }
```

```java
Set<String> set = new HashSet<>();
set.add("apple");
set.add("banana");
set.add("apple");    // ignored — "apple" already exists

System.out.println(set);   // {banana, apple}  — only 2 elements, no duplicate
```

---

## Key Features of a Set

- Stores **only unique elements** — adding a duplicate is silently ignored
- No index-based access — you can't do `set.get(0)` like an array/list
- `Set` is an **interface** in Java — you can't do `new Set()`, you must instantiate one of its implementing classes
- Average **O(1)** add/remove/contains for hash-based implementations
- Supports classic **mathematical set operations** — union, intersection, difference

```java
Set<Integer> set = new HashSet<>();       // ✅ valid — Set is the interface, HashSet is the class
Set<Integer> set2 = new Set<>();          // ❌ invalid — Set is an interface, can't be instantiated
```

---

## Set vs Map — What's Actually Different

**In one sentence:**
> A Map stores key → value pairs; a Set stores just values, with no keys attached.

| Map | Set |
| --- | --- |
| Stores **key-value pairs** | Stores **standalone elements** |
| Access via `get(key)` | Access only via `contains(element)` — no direct "get" |
| Keys must be unique, values can repeat | **All elements must be unique** |
| `Map.Entry` bundles a key + value together | No such pairing — just the element itself |

> **The interesting part ⭐:** even though *what* they store is different, Java's `Set` implementations are literally **built on top of** the matching `Map` implementations under the hood — which is why they share the same performance characteristics and ordering behavior. `HashSet` internally wraps a `HashMap`, `LinkedHashSet` wraps a `LinkedHashMap`, and `TreeSet` wraps a `TreeMap` — the Set just stores each element as a **key**, paired with a dummy placeholder value that's never actually used.

---

# Set Operations

> **📅 Written:** 20 Jul 2026

## 1. Insert

```java
Set<String> set = new HashSet<>();
set.add("apple");     // added
set.add("apple");     // ignored, already exists — add() returns false here
```

→ **O(1)** average for hash-based sets.

---

## 2. Delete

```java
set.remove("apple");    // removes "apple" if present
```

→ **O(1)** average.

---

## 3. Search (contains)

```java
set.contains("banana");   // true or false
```

→ **O(1)** average — this is the main reason Sets are used: **fast existence checks**, much faster than scanning a List with `.contains()` which is O(n).

---

## 4. Traversal

```java
for (String item : set) {
    System.out.println(item);
}
```

→ **O(n)** — visits every element once.

---

## Time Complexity — Quick Table

| Operation      | Time Complexity (average) |
| --------------- | --------------------------- |
| Insert (`add`)   | O(1)                         |
| Delete (`remove`)| O(1)                         |
| Search (`contains`)| O(1)                       |
| Traversal        | O(n)                         |

> Same caveat as Map — these O(1) averages apply to **hash-based** sets (`HashSet`, `LinkedHashSet`). `TreeSet` is the exception, at O(log n).

---

# Set Mathematical Operations

> **📅 Written:** 20 Jul 2026

### Definition

Since a Set mirrors the mathematical concept of a set, Java's `Set` interface supports the classic set operations directly through bulk methods.

```java
Set<Integer> a = new HashSet<>(Arrays.asList(1, 2, 3, 4));
Set<Integer> b = new HashSet<>(Arrays.asList(3, 4, 5, 6));
```

## 1. Union — `addAll()`

```java
Set<Integer> union = new HashSet<>(a);
union.addAll(b);
System.out.println(union);   // [1, 2, 3, 4, 5, 6]
```

## 2. Intersection — `retainAll()`

```java
Set<Integer> intersection = new HashSet<>(a);
intersection.retainAll(b);
System.out.println(intersection);   // [3, 4]
```

## 3. Difference — `removeAll()`

```java
Set<Integer> difference = new HashSet<>(a);
difference.removeAll(b);
System.out.println(difference);   // [1, 2] — elements in "a" but not in "b"
```

```
a:  {1, 2, 3, 4}
b:        {3, 4, 5, 6}

Union:         {1, 2, 3, 4, 5, 6}
Intersection:        {3, 4}
Difference (a-b): {1, 2}
```

---

# The Three Main Set Implementations

> **📅 Written:** 20 Jul 2026

## 1. HashSet

### Definition

**A HashSet stores elements using a hash table (internally backed by a HashMap) — offering no guarantee about the order of its elements, but the fastest average performance.**

- **No ordering guarantee** — iterating a `HashSet` can return elements in a seemingly random order
- Allows **one `null` element**
- Fastest of the three for pure add/remove/contains

```java
Set<String> set = new HashSet<>();
set.add("banana");
set.add("apple");
set.add("cherry");

System.out.println(set);
// order NOT guaranteed — e.g. [banana, cherry, apple]
```

---

## 2. LinkedHashSet

### Definition

**A LinkedHashSet is a HashSet that additionally maintains a doubly-linked list running through all its elements — preserving the order in which they were inserted.**

- Same hash-table speed as `HashSet`
- **Preserves insertion order** when you iterate
- Slightly more memory overhead than `HashSet`, due to the extra linked-list pointers per element

```java
Set<String> set = new LinkedHashSet<>();
set.add("banana");
set.add("apple");
set.add("cherry");

System.out.println(set);
// [banana, apple, cherry] — always in insertion order
```

---

## 3. TreeSet

### Definition

**A TreeSet stores elements in a Red-Black Tree (internally backed by a TreeMap), automatically keeping them sorted at all times.**

- Elements are always kept in **sorted order** (natural order, or a custom `Comparator`)
- Slower than `HashSet`/`LinkedHashSet` — **O(log n)** instead of O(1), since it's tree-based, not hash-based
- Does **not** allow a `null` element (throws `NullPointerException`)
- Useful for range queries — e.g. "give me all elements between X and Y"

```java
Set<String> set = new TreeSet<>();
set.add("banana");
set.add("apple");
set.add("cherry");

System.out.println(set);
// [apple, banana, cherry] — always sorted
```

```java
TreeSet<String> tset = new TreeSet<>(set);
System.out.println(tset.first());   // "apple"  — smallest element
System.out.println(tset.last());    // "cherry" — largest element
```

---

## HashSet vs LinkedHashSet vs TreeSet ⭐

| Feature | HashSet | LinkedHashSet | TreeSet |
| --- | --- | --- | --- |
| Backed internally by | `HashMap` | `LinkedHashMap` | `TreeMap` |
| Ordering | No guarantee | Insertion order (preserved) | Sorted order |
| Add / Remove / Contains | O(1) average | O(1) average | O(log n) |
| Allows `null` | Yes (one) | Yes (one) | No — throws exception |
| Extra memory overhead | Lowest | Slightly higher (linked list pointers) | Higher (tree node pointers) |
| Best used when | Fast checks, order doesn't matter | Fast checks **and** predictable iteration order | Sorted elements or range queries needed |

---

## Set vs Map Implementations — Side by Side ⭐

This is the part worth remembering: **the three Set classes and three Map classes aren't separate ideas — they're the same underlying engines, just exposed differently.**

| Set class | Internally backed by | Matching Map class | Shared behavior |
| --- | --- | --- | --- |
| `HashSet` | `HashMap` | `HashMap` | O(1) average, no ordering |
| `LinkedHashSet` | `LinkedHashMap` | `LinkedHashMap` | O(1) average, insertion order preserved |
| `TreeSet` | `TreeMap` | `TreeMap` | O(log n), sorted order, no `null` allowed |

---

## Quick Revision

- **Set** → Interface for storing unique elements only; can't be instantiated directly.
- **Set vs Map** → Map stores key→value pairs; Set stores standalone unique elements — but under the hood, each Set class literally wraps its matching Map class.
- **HashSet** → Backed by HashMap, O(1) average, **no ordering guarantee**, allows one `null`.
- **LinkedHashSet** → Backed by LinkedHashMap, O(1) average, preserves **insertion order**.
- **TreeSet** → Backed by TreeMap, O(log n), keeps elements **sorted**, no `null` allowed.
- **Mathematical ops** → Union (`addAll`), Intersection (`retainAll`), Difference (`removeAll`).
- **Rule of thumb** → No order needed → `HashSet`. Need insertion order → `LinkedHashSet`. Need sorted/range access → `TreeSet`.

---

## Interview Definition ⭐

> **A Set is an interface for storing unique elements with fast existence checks, implemented in Java by HashSet (fastest, unordered), LinkedHashSet (hash speed with insertion order preserved), and TreeSet (sorted via a Red-Black Tree, O(log n)) — each of which is internally just a Map implementation (HashMap, LinkedHashMap, TreeMap respectively) storing elements as keys with a dummy value.**