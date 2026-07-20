# Map

> **📅 Written:** 20 Jul 2026

### Definition

**A Map is a data structure that stores data as key-value pairs, where each key is unique and maps to exactly one value — allowing fast lookup of a value using its key instead of an index.**

**In one sentence:**
> A Map is like a real-world dictionary — you look up a "word" (key) to instantly get its "meaning" (value), instead of having to scan through everything one by one.

**Example:**

```
Key       Value
"apple"   1
"banana"  2
"cherry"  3
```

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
System.out.println(map.get("apple"));   // 1
```

---

## Key Features of a Map

- Stores data as **key-value pairs**
- Keys are **unique** — putting a value with an existing key **overwrites** the old value
- Values **can repeat**
- `Map` is an **interface** in Java — you can't do `new Map()`, you must instantiate one of its implementing classes
- Average **O(1)** lookup/insert/delete for hash-based implementations — far faster than scanning a list

> Just like `List` is an interface and `ArrayList` is the class you actually instantiate, `Map` is an interface too — `HashMap`, `LinkedHashMap`, and `TreeMap` are the classes that implement it and that you actually create objects from.

```java
Map<String, Integer> map = new HashMap<>();   // ✅ valid — Map is the interface, HashMap is the class
Map<String, Integer> map2 = new Map<>();      // ❌ invalid — Map is an interface, can't be instantiated
```

---

# Map Operations

> **📅 Written:** 20 Jul 2026

## 1. Insert / Update

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);     // inserts a new key-value pair
map.put("apple", 5);     // key already exists → overwrites the value with 5
```

→ **O(1)** average for hash-based maps.

---

## 2. Access

```java
map.get("apple");            // 5
map.getOrDefault("mango", 0); // 0 — "mango" doesn't exist, returns the default instead
```

→ **O(1)** average — jumps almost directly to the value via the key's hash.

---

## 3. Delete

```java
map.remove("apple");    // removes the key "apple" and its value entirely
```

→ **O(1)** average.

---

## 4. Search (key existence)

```java
map.containsKey("banana");    // true
map.containsValue(2);          // true
```

→ **O(1)** average for `containsKey`, **O(n)** for `containsValue` (has to check every value, since values aren't indexed).

---

## 5. Traversal

```java
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
```

→ **O(n)** — visits every key-value pair once.

---

## Time Complexity — Quick Table

| Operation                | Time Complexity (average) |
| ------------------------- | --------------------------- |
| Insert / Update (`put`)    | O(1)                         |
| Access (`get`)             | O(1)                         |
| Delete (`remove`)          | O(1)                         |
| Search key (`containsKey`) | O(1)                         |
| Search value (`containsValue`) | O(n)                    |
| Traversal                  | O(n)                         |

> These O(1) averages apply to **hash-based** maps (`HashMap`, `LinkedHashMap`). `TreeMap` is the exception — see below.

---

# Map Views: keySet(), values(), entrySet()

> **📅 Written:** 20 Jul 2026

### Definition

**A Map doesn't hand you its contents directly for iteration — instead it gives you a "view" of itself, focused on just the keys, just the values, or both paired together.**

**In one sentence:**
> These three methods are just different lenses to look through the same Map — pick the one that matches what you actually need, so you're not doing extra work you don't have to.

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("cherry", 3);
```

## 1. `keySet()`

Returns a **`Set`** of just the keys — no values attached.

```java
for (String key : map.keySet()) {
    System.out.println(key);
}
// apple
// banana
// cherry
```

Use this when you only care about **which keys exist** — if you also need the value, you'd have to call `map.get(key)` separately for each one, which is an extra O(1) lookup every iteration.

---

## 2. `values()`

Returns a **`Collection`** of just the values — no keys attached (and since values can repeat, it's a `Collection`, not a `Set`).

```java
for (int value : map.values()) {
    System.out.println(value);
}
// 1
// 2
// 3
```

Use this when you only care about the **values themselves** and don't need to know which key they belong to.

---

## 3. `entrySet()`

Returns a **`Set<Map.Entry<K, V>>`** — each element is one key-value pair bundled together as a single `Entry` object, so both are available at once with no extra lookup.

```java
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    int value = entry.getValue();
    System.out.println(key + " -> " + value);
}
// apple -> 1
// banana -> 2
// cherry -> 3
```

**`Entry`'s own methods:**

```java
entry.getKey();          // returns the key
entry.getValue();        // returns the value
entry.setValue(99);      // updates the value directly in the map, through this entry
```

**Why prefer `entrySet()` over `keySet()` + `get()` when you need both:**
Looping over `keySet()` and calling `map.get(key)` inside the loop repeats an O(1) hash lookup on **every single iteration** — small, but unnecessary. `entrySet()` skips that entirely since each `Entry` already *is* the pair, pulled straight from the map's internal bucket.

---

## keySet() vs values() vs entrySet() ⭐

| Method | Returns | Use when |
| --- | --- | --- |
| `keySet()` | `Set<K>` — just the keys | You only need the keys |
| `values()` | `Collection<V>` — just the values | You only need the values |
| `entrySet()` | `Set<Map.Entry<K,V>>` — key + value pairs | You need both, looping through the whole map |

---

# The Three Main Map Implementations

> **📅 Written:** 20 Jul 2026

## 1. HashMap

### Definition

**A HashMap stores key-value pairs using a hash table — it offers no guarantee about the order of its entries, but gives the fastest average performance.**

- Backed by an array of **buckets**, where each key's **hash code** decides which bucket it lands in
- **No ordering guarantee** — iterating a `HashMap` can return entries in a seemingly random order
- Allows **one `null` key** and multiple `null` values
- Fastest of the three for pure lookups/inserts

```java
Map<String, Integer> map = new HashMap<>();
map.put("banana", 2);
map.put("apple", 1);
map.put("cherry", 3);

System.out.println(map);
// order NOT guaranteed — e.g. {banana=2, cherry=3, apple=1}
```

---

## 2. LinkedHashMap

### Definition

**A LinkedHashMap is a HashMap that additionally maintains a doubly-linked list running through all its entries — preserving the order in which they were inserted.**

> Quick correction on the name — it's **`LinkedHashMap`**, not `LinkedMap` (an easy mix-up, since `LinkedList` doesn't have the "Hash" in it, but this one does).

- Same hash-table speed as `HashMap`
- **Preserves insertion order** (or optionally access order, if configured) when you iterate
- Slightly more memory overhead than `HashMap`, due to the extra linked-list pointers per entry

```java
Map<String, Integer> map = new LinkedHashMap<>();
map.put("banana", 2);
map.put("apple", 1);
map.put("cherry", 3);

System.out.println(map);
// {banana=2, apple=1, cherry=3} — always in insertion order
```

---

## 3. TreeMap

### Definition

**A TreeMap stores key-value pairs in a Red-Black Tree (a self-balancing binary search tree), automatically keeping the keys sorted at all times.**

- Keys are always kept in **sorted order** (natural order, or a custom `Comparator`)
- Slower than `HashMap`/`LinkedHashMap` — **O(log n)** instead of O(1), since it's tree-based, not hash-based
- Does **not** allow a `null` key (throws `NullPointerException`)
- Useful when you need range queries — e.g. "give me all keys between X and Y"

```java
Map<String, Integer> map = new TreeMap<>();
map.put("banana", 2);
map.put("apple", 1);
map.put("cherry", 3);

System.out.println(map);
// {apple=1, banana=2, cherry=3} — always sorted by key
```

```java
TreeMap<String, Integer> tmap = new TreeMap<>(map);
System.out.println(tmap.firstKey());   // "apple"  — smallest key
System.out.println(tmap.lastKey());    // "cherry" — largest key
```

---

## HashMap vs LinkedHashMap vs TreeMap ⭐

| Feature | HashMap | LinkedHashMap | TreeMap |
| --- | --- | --- | --- |
| Underlying structure | Hash table | Hash table + doubly-linked list | Red-Black Tree |
| Ordering | No guarantee | Insertion order (preserved) | Sorted by key |
| Get / Put / Remove | O(1) average | O(1) average | O(log n) |
| Allows `null` key | Yes (one) | Yes (one) | No — throws exception |
| Extra memory overhead | Lowest | Slightly higher (linked list pointers) | Higher (tree node pointers) |
| Best used when | You just need fast lookups, order doesn't matter | You need fast lookups **and** predictable iteration order | You need sorted keys or range queries |

---

## Quick Revision

- **Map** → Interface for storing unique key → value pairs; can't be instantiated directly.
- **HashMap** → Hash table backed, O(1) average, **no ordering guarantee**, allows one `null` key.
- **LinkedHashMap** → HashMap + a linked list to preserve **insertion order**, still O(1) average.
- **TreeMap** → Red-Black Tree backed, keeps keys **sorted**, O(log n), no `null` keys allowed.
- **Rule of thumb** → No order needed → `HashMap`. Need insertion order → `LinkedHashMap`. Need sorted/range access → `TreeMap`.

---

## Interview Definition ⭐

> **A Map is an interface for storing unique key-to-value pairs with fast lookup by key, implemented in Java primarily by HashMap (fastest, unordered), LinkedHashMap (hash speed with insertion order preserved), and TreeMap (sorted order via a Red-Black Tree, at O(log n) instead of O(1)).**