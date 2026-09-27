# Map Data Structure - Java

> **📅 Written:** 24 Sep 2026

### Definition

**A Map is a data structure that stores data as key-value pairs, where each key is unique and maps to exactly one value, allowing O(1) average-time lookup, insertion, and deletion by key.**

**In one sentence:**
> A map is a labeled-storage system — instead of finding data by a numeric index like an array, you find it instantly by a meaningful key, like a name or an ID.

**Example:**

```
Key      Value
"Alice"  --> 25
"Bob"    --> 30
"Eve"    --> 22
```

---

## `Map` Interface — Main Implementations

`Map` is an interface (like `List`) — you don't instantiate it directly. The three main implementations behave very differently:

| Implementation | Ordering | Backing Structure | Typical Use |
|---|---|---|---|
| `HashMap` | **No guaranteed order** | Hash table (array of buckets) | Default choice — fastest general-purpose map |
| `LinkedHashMap` | **Insertion order** preserved | Hash table + doubly-linked list | When you need fast lookup *and* predictable iteration order |
| `TreeMap` | **Sorted by key** | Red-Black Tree | When you need keys in sorted order, or range queries |

```java
Map<String, Integer> hashMap = new HashMap<>();
Map<String, Integer> linkedMap = new LinkedHashMap<>();
Map<String, Integer> treeMap = new TreeMap<>();
```

---

## How HashMap Works Internally

1. Each key's `hashCode()` is computed, then mapped to a **bucket index** (array slot)
2. The key-value pair is stored in that bucket
3. Multiple keys landing in the same bucket (a **collision**) are stored together — as a linked list, or a balanced tree if the bucket gets large (Java 8+ optimization for heavy collisions)

```
hashCode("Alice") -> bucket 3
hashCode("Bob")    -> bucket 7
hashCode("Eve")    -> bucket 3   <- collision with "Alice", chained in bucket 3

Buckets: [ _, _, _, [Alice, Eve], _, _, _, [Bob], ... ]
```

→ This is *why* average-case operations are **O(1)** — you jump directly to a bucket via hashing instead of scanning. Worst case (many collisions) degrades to **O(n)**.

> **Interview point ⭐:** A good `hashCode()` implementation (spreading keys evenly across buckets) is what keeps HashMap operations close to O(1) in practice. Two equal objects **must** have the same `hashCode()` — this is the `equals()`/`hashCode()` contract, and breaking it silently corrupts HashMap behavior.

---

## Declaration & Basic Usage

```java
Map<String, Integer> map = new HashMap<>();

map.put("Alice", 25);          // insert / update
map.get("Alice");              // 25
map.getOrDefault("Bob", -1);   // -1, since "Bob" isn't present yet
map.containsKey("Alice");      // true
map.remove("Alice");           // removes the entry
```

---

## Iterating Over a Map

```java
// Preferred: entrySet — gets key AND value in one pass
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}

// keySet — only keys (then look up value if needed, extra O(1) call each time)
for (String key : map.keySet()) {
    System.out.println(key + " -> " + map.get(key));
}
```

> **Watch out ⭐:** Iterating `keySet()` and calling `map.get(key)` inside the loop works, but `entrySet()` is preferred since it avoids a redundant lookup per entry.

---

## Built-in Methods / Functions

| Method | What it does |
|---|---|
| `map.put(k, v)` | Inserts, or updates if `k` already exists |
| `map.putIfAbsent(k, v)` | Inserts only if `k` is **not** already present |
| `map.get(k)` | Returns value for `k`, or `null` if absent |
| `map.getOrDefault(k, def)` | Returns value for `k`, or `def` if absent |
| `map.remove(k)` | Removes the entry for `k` |
| `map.containsKey(k)` | Whether `k` exists |
| `map.containsValue(v)` | Whether `v` exists (O(n) — scans all values!) |
| `map.size()` | Number of entries |
| `map.isEmpty()` | Whether the map has 0 entries |
| `map.keySet()` | View of all keys |
| `map.values()` | View of all values |
| `map.entrySet()` | View of all key-value pairs |
| `map.merge(k, v, fn)` | Combines new value with existing via a function (great for counting) |
| `map.compute(k, fn)` | Recomputes the value for `k` based on current value |
| `map.forEach((k, v) -> ...)` | Functional-style iteration |

```java
// Classic frequency-counting one-liner using merge()
Map<Character, Integer> freq = new HashMap<>();
for (char c : "leetcode".toCharArray()) {
    freq.merge(c, 1, Integer::sum);
}
```

---

## Time Complexity — Quick Table

| Operation | HashMap (avg) | HashMap (worst) | TreeMap |
|---|---|---|---|
| `put` / `get` / `remove` | O(1) | O(n) | O(log n) |
| `containsKey` | O(1) | O(n) | O(log n) |
| Iteration | O(n) | O(n) | O(n) |
| Ordered traversal | ❌ not supported | ❌ | ✅ sorted by key |

---

## HashMap vs TreeMap vs LinkedHashMap ⭐

| | HashMap | LinkedHashMap | TreeMap |
|---|---|---|---|
| Order | None | Insertion order | Sorted by key |
| Lookup | O(1) avg | O(1) avg | O(log n) |
| Null keys | 1 allowed | 1 allowed | ❌ not allowed |
| Use case | Default/fastest | Predictable iteration (e.g. LRU cache) | Range queries, sorted keys, `firstKey()`/`ceilingKey()` |

```java
TreeMap<Integer, String> treeMap = new TreeMap<>();
treeMap.firstKey();       // smallest key
treeMap.lastKey();        // largest key
treeMap.ceilingKey(5);    // smallest key >= 5
treeMap.floorKey(5);      // largest key <= 5
```

---

## Quick Revision

- **Map** → key-value pairs, unique keys, O(1) average lookup via hashing.
- **HashMap** → no order, fastest; **LinkedHashMap** → insertion order; **TreeMap** → sorted, O(log n).
- Internally: `hashCode()` → bucket index; **collisions** chained within a bucket.
- Prefer `entrySet()` over `keySet()` + `get()` when iterating both key and value.
- `merge()` and `getOrDefault()` are the go-to tools for frequency counting.
- `equals()`/`hashCode()` contract must hold, or HashMap behavior breaks silently.

---

## Interview Definition ⭐

> **A Map stores unique keys mapped to values, using the key's hash code to compute a bucket index for near O(1) average-case put/get/remove — HashMap offers no ordering guarantee, LinkedHashMap preserves insertion order, and TreeMap keeps keys sorted at O(log n) per operation via an underlying Red-Black Tree.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) | Easy | The single most common HashMap warm-up problem |
| 2 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | Medium | Map with a computed key (sorted string / frequency signature) |
| 3 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Medium | HashMap counting + heap/bucket sort |
| 4 | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | Medium | Prefix sum + HashMap for O(n) counting |
| 5 | [LRU Cache](https://leetcode.com/problems/lru-cache/) | Medium | LinkedHashMap (or HashMap + doubly-linked list) design problem — very common in interviews |
| 6 | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) | Medium | HashSet/HashMap for O(n) sequence detection |
| 7 | [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) | Medium | HashMap used to map old nodes to cloned nodes |
