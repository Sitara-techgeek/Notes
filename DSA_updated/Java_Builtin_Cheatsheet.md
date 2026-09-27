# Java Built-in Methods — Master Cheatsheet

> **📅 Written:** 24 Sep 2026

### Purpose

This file consolidates **every built-in method table** scattered across the individual topic notes into one place — purely for rapid, "night-before-the-interview" scanning. For the *why* behind any method (internals, complexity reasoning, gotchas), go back to that topic's dedicated file — this is a lookup reference, not a teaching document.

---

## `Arrays` (java.util.Arrays)

| Method | What it does |
|---|---|
| `arr.length` | Field (not a method) — array size |
| `Arrays.sort(arr)` | Ascending sort, in-place — O(n log n) |
| `Arrays.sort(arr, l, r)` | Sorts only range `[l, r)` |
| `Arrays.sort(arr, comparator)` | Custom sort — **objects only**, not primitives |
| `Arrays.fill(arr, val)` | Fills every slot with `val` |
| `Arrays.equals(a, b)` | Element-wise equality check |
| `Arrays.copyOf(arr, newLen)` | Resized copy |
| `Arrays.copyOfRange(arr, from, to)` | Sub-range copy |
| `Arrays.toString(arr)` | Human-readable string, e.g. `[1, 2, 3]` |
| `Arrays.asList(arr)` | Fixed-size `List` view of the array |
| `Arrays.binarySearch(arr, key)` | O(log n) search on a **sorted** array |
| `Arrays.stream(arr)` | Converts to a `Stream` for functional ops |
| *(2D)* `Arrays.deepToString(arr)` | Readable string for 2D arrays |

*(Full context: `Arrays.md`)*

---

## `String`

| Method | What it does |
|---|---|
| `s.length()` | Number of characters |
| `s.charAt(i)` | Character at index `i` |
| `s.substring(a, b)` | Substring `[a, b)` |
| `s.indexOf(x)` / `s.lastIndexOf(x)` | First/last index of `x`, or -1 |
| `s.contains(x)` | Whether `x` is a substring |
| `s.equals(t)` / `.equalsIgnoreCase(t)` | Content comparison |
| `s.compareTo(t)` | Lexicographic comparison |
| `s.toCharArray()` | Converts to `char[]` |
| `s.toUpperCase()` / `.toLowerCase()` | Case conversion |
| `s.trim()` / `.strip()` | Removes leading/trailing whitespace |
| `s.split(regex)` | Splits into `String[]` |
| `s.replace(a, b)` | Replaces all occurrences |
| `String.join(delim, arr)` | Joins with a delimiter |
| `String.valueOf(x)` | Converts to String |

*(Full context: `Strings.md`)*

## `StringBuilder`

| Method | What it does |
|---|---|
| `sb.append(x)` | Appends — amortized O(1) |
| `sb.reverse()` | Reverses in place |
| `sb.deleteCharAt(i)` | Removes char at index `i` |
| `sb.insert(i, x)` | Inserts at index `i` |
| `sb.toString()` | Converts to an immutable `String` |
| `sb.length()` | Current length |
| `sb.setCharAt(i, c)` | Replaces char at index `i` |

*(Full context: `Strings.md`)*

---

## `List` / `ArrayList` / `Collections`

| Method | What it does | Time |
|---|---|---|
| `list.add(x)` | Appends | O(1) amortized |
| `list.add(i, x)` | Inserts at index, shifts right | O(n) |
| `list.get(i)` / `.set(i, x)` | Access / replace | O(1) |
| `list.remove(i)` | Remove by **index** | O(n) |
| `list.remove(Object x)` | Remove by **value** | O(n) |
| `list.contains(x)` / `.indexOf(x)` | Search | O(n) |
| `list.size()` / `.isEmpty()` | Size checks | O(1) |
| `Collections.sort(list)` | Ascending sort, in-place | O(n log n) |
| `Collections.sort(list, comparator)` | Custom sort | O(n log n) |
| `Collections.reverse(list)` | Reverses in-place | O(n) |
| `Collections.max(list)` / `.min(list)` | Max/min element | O(n) |
| `list.subList(a, b)` | View of range `[a,b)` — backed by original! | O(1) |

*(Full context: `List_and_ArrayList.md`)*

---

## `Map` (HashMap / TreeMap / LinkedHashMap)

| Method | What it does |
|---|---|
| `map.put(k, v)` | Insert or update |
| `map.putIfAbsent(k, v)` | Insert only if absent |
| `map.get(k)` | Value for `k`, or `null` |
| `map.getOrDefault(k, def)` | Value for `k`, or `def` |
| `map.remove(k)` | Removes entry |
| `map.containsKey(k)` | O(1) — key check |
| `map.containsValue(v)` | O(n) — scans values! |
| `map.keySet()` / `.values()` / `.entrySet()` | Views for iteration |
| `map.merge(k, v, fn)` | Combine new + existing value (great for counting) |
| `map.compute(k, fn)` | Recompute value based on current |
| `map.forEach((k,v) -> ...)` | Functional iteration |

### TreeMap-specific
| Method | What it does |
|---|---|
| `treeMap.firstKey()` / `.lastKey()` | Smallest / largest key |
| `treeMap.ceilingKey(x)` / `.floorKey(x)` | Smallest ≥x / largest ≤x |
| `treeMap.higherKey(x)` / `.lowerKey(x)` | Smallest >x / largest <x |

*(Full context: `Map_Data_Structure.md`)*

---

## `Set` (HashSet / TreeSet / LinkedHashSet)

| Method | What it does |
|---|---|
| `set.add(x)` | Adds; returns `false` if already present |
| `set.remove(x)` | Removes if present |
| `set.contains(x)` | O(1) avg membership check |
| `set.addAll(other)` | **Union** |
| `set.retainAll(other)` | **Intersection** |
| `set.removeAll(other)` | **Difference** |
| `Collections.disjoint(a, b)` | True if no shared elements |

### TreeSet-specific
| Method | What it does |
|---|---|
| `treeSet.first()` / `.last()` | Smallest / largest |
| `treeSet.ceiling(x)` / `.floor(x)` | Smallest ≥x / largest ≤x |
| `treeSet.higher(x)` / `.lower(x)` | Smallest >x / largest <x |

*(Full context: `Set_Data_Structure.md`)*

---

## `Deque` — as Stack / Queue / Deque (java.util.ArrayDeque)

| As... | Method | Does |
|---|---|---|
| Stack | `push(x)` / `pop()` / `peek()` | Add/remove/view top — LIFO |
| Queue | `offer(x)` / `poll()` / `peek()` | Add/remove/view front — FIFO (poll from front) |
| Deque | `addFirst(x)` / `addLast(x)` | Insert at either end |
| Deque | `removeFirst()` / `removeLast()` | Remove from either end |
| Deque | `peekFirst()` / `peekLast()` | View either end |

> Prefer `offer/poll/peek` over `add/remove/element` — the latter throw exceptions on failure instead of returning `null`/`false`.

*(Full context: `Stack_Queue_PriorityQueue_Deque_Implementation.md`)*

---

## `PriorityQueue`

| Method | What it does | Time |
|---|---|---|
| `new PriorityQueue<>()` | Min-heap by default | — |
| `new PriorityQueue<>(Collections.reverseOrder())` | Max-heap | — |
| `new PriorityQueue<>((a,b) -> ...)` | Custom comparator | — |
| `pq.offer(x)` | Insert | O(log n) |
| `pq.poll()` | Remove & return min/max | O(log n) |
| `pq.peek()` | View min/max without removing | O(1) |

> Iterating a `PriorityQueue` directly does **NOT** give sorted order — only repeated `poll()` does.

*(Full context: `Priority_Queue_and_Binary_Heap.md`)*

---

## Bit Manipulation — `Integer` / `Long` Static Methods

| Method | What it does |
|---|---|
| `Integer.bitCount(n)` | Number of set bits (popcount) |
| `Integer.toBinaryString(n)` | Binary string representation |
| `Integer.numberOfLeadingZeros(n)` | Count of leading 0 bits |
| `Integer.numberOfTrailingZeros(n)` | Count of trailing 0 bits |
| `Integer.highestOneBit(n)` / `.lowestOneBit(n)` | Isolates highest/lowest set bit |
| `Integer.parseInt(s)` | String → int |
| `Integer.MAX_VALUE` / `MIN_VALUE` | Int bounds — useful as sentinel values |
| `Long.bitCount(n)` | Same as above, for `long` |

**Manual bit tricks** (not built-in, but essential):
```java
(n & (1 << i)) != 0     // is bit i set?
n | (1 << i)             // set bit i
n & ~(1 << i)            // clear bit i
n ^ (1 << i)              // toggle bit i
n & (n - 1)                // clears lowest set bit (power-of-2 check, popcount)
n & (-n)                    // isolates lowest set bit
```

*(Full context: `Bit_Manipulation_Easy.md`, `Bit_Manipulation_Application.md`)*

---

## `Math` (commonly used in DSA)

| Method | What it does |
|---|---|
| `Math.max(a, b)` / `Math.min(a, b)` | Max/min of two values |
| `Math.abs(x)` | Absolute value |
| `Math.pow(base, exp)` | Power (returns `double` — cast/round carefully) |
| `Math.sqrt(x)` | Square root |
| `Math.ceil(x)` / `Math.floor(x)` | Round up/down (returns `double`) |
| `Math.log(x)` | Natural log — for log base b, divide: `Math.log(x)/Math.log(b)` |

---

## Comparators & Sorting Utilities

```java
Comparator.comparing(Type::field)              // sort by a field
Comparator.comparing(Type::field).reversed()     // descending
Comparator.comparing(A::field).thenComparing(B::field)  // multi-level sort
Comparator.naturalOrder() / .reverseOrder()       // default ascending/descending
Collections.reverseOrder()                          // for use with Arrays.sort on objects
```

*(Full context: `Sorting_and_Custom_Sorting.md`)*

---

## Quick Index — "Which File Has the Full Explanation?"

| Class / Topic | File |
|---|---|
| `Arrays` utility methods | `02_Data_Structures/Arrays.md` |
| `String` / `StringBuilder` | `02_Data_Structures/Strings.md` |
| `List` / `ArrayList` / `Collections` | `02_Data_Structures/List_and_ArrayList.md` |
| `Map` (HashMap/TreeMap/LinkedHashMap) | `02_Data_Structures/Map_Data_Structure.md` |
| `Set` (HashSet/TreeSet/LinkedHashSet) | `02_Data_Structures/Set_Data_Structure.md` |
| `Deque` / Stack / Queue | `02_Data_Structures/Stack_Queue_PriorityQueue_Deque_Implementation.md` |
| `PriorityQueue` internals | `02_Data_Structures/Priority_Queue_and_Binary_Heap.md` |
| Bit tricks & `Integer`/`Long` bit methods | `03_Algorithms_and_Techniques/Bit_Manipulation_Easy.md` & `_Application.md` |
| `Comparator`/`Comparable` sorting | `03_Algorithms_and_Techniques/Sorting_and_Custom_Sorting.md` |
| GCD/LCM/modular arithmetic | `03_Algorithms_and_Techniques/Logic_Building_Number_Theory.md` |
