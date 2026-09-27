# Stack, Queue, Priority Queue, and Deque - Implementation

> **📅 Written:** 24 Sep 2026

### Definition

**Stack, Queue, Priority Queue, and Deque are all linear data structures that restrict *how* elements can be added or removed, unlike arrays/lists which allow access anywhere — this restriction is exactly what makes them useful.**

**In one sentence:**
> Each of these is really just "an array or list with rules" — the rules about which end you can touch are what give each structure its special behavior.

---

## 1. Stack — LIFO (Last In, First Out)

### Definition

Elements are added and removed from the **same end** — the "top". The last element pushed is the first one popped.

```
push(1) push(2) push(3)        pop()
                          
   |3|  <- top               |2|  <- top
   |2|                       |1|
   |1|
```

### Java Implementation

Java's legacy `Stack` class exists, but the **recommended** way is `ArrayDeque` implementing the `Deque` interface — it's faster and unsynchronized (no needless overhead).

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);       // add to top
stack.push(2);
stack.pop();          // removes & returns 2 (top)
stack.peek();          // returns 1 without removing
stack.isEmpty();        // false
```

> **Interview point ⭐:** Avoid `java.util.Stack` in real code — it extends `Vector`, which is synchronized (unnecessary overhead) and considered a legacy class. `ArrayDeque` is the modern, faster choice for stack behavior.

**Time Complexity:** `push`, `pop`, `peek` all **O(1)**.

---

## 2. Queue — FIFO (First In, First Out)

### Definition

Elements are added at the **rear** and removed from the **front**. The first element added is the first one removed.

```
enqueue(1) enqueue(2) enqueue(3)      dequeue()

front -> |1|2|3| <- rear             front -> |2|3| <- rear
```

### Java Implementation

```java
Queue<Integer> queue = new LinkedList<>();   // or ArrayDeque
queue.offer(1);        // add to rear (preferred over add() — doesn't throw on failure)
queue.offer(2);
queue.poll();           // removes & returns 1 (front) — returns null if empty
queue.peek();            // returns 2 without removing — returns null if empty
```

> **Watch out ⭐:** `add()`/`remove()`/`element()` throw exceptions on failure (empty queue, full capacity); `offer()`/`poll()`/`peek()` return `null`/`false` instead — prefer the latter for safer code, especially with bounded queues.

**Time Complexity:** `offer`, `poll`, `peek` all **O(1)**.

---

## 3. Deque — Double-Ended Queue

### Definition

Generalizes both Stack and Queue — insertion and removal allowed at **both** ends.

```
   addFirst()                    addLast()
        ↓                             ↓
      |___|_______________|___|
        ↑                             ↑
   removeFirst()                removeLast()
```

### Java Implementation

```java
Deque<Integer> deque = new ArrayDeque<>();
deque.addFirst(1);       // add to front
deque.addLast(2);         // add to rear
deque.removeFirst();       // remove from front
deque.removeLast();         // remove from rear
deque.peekFirst();           // view front
deque.peekLast();             // view rear
```

- Used **as a Stack** via `push()`/`pop()` (both operate on the front)
- Used **as a Queue** via `offer()`/`poll()` (rear in, front out)
- Extremely common as the backbone of **sliding window maximum/minimum** problems (a monotonic deque)

**Time Complexity:** All operations at either end are **O(1)**.

---

## 4. Priority Queue — Always Serve the "Highest Priority" Element

### Definition

Elements are removed in order of **priority**, not insertion order — internally backed by a **Binary Heap** (min-heap by default in Java).

```
insert(5), insert(1), insert(3)

Min-Heap internally:      poll() always returns the smallest:
        1                  poll() -> 1
       / \                 poll() -> 3
      5   3                poll() -> 5
```

### Java Implementation

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();               // min-heap by default
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder()); // max-heap

minHeap.offer(5);
minHeap.offer(1);
minHeap.offer(3);
minHeap.poll();     // 1 (smallest removed first)

// Custom objects with a comparator
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]); // min-heap by first element
```

> **Interview point ⭐:** `PriorityQueue` guarantees the **head** is always the min/max, but **iterating** it (`for` loop, `toString()`) does **NOT** give sorted order — only repeated `poll()` calls do. This trips people up constantly.

**Time Complexity:** `offer`/`poll` → **O(log n)**; `peek` → **O(1)**.

*(Full internal mechanics — heapify, sift up/down — covered in the dedicated Priority Queue and Binary Heap note.)*

---

## Summary Comparison ⭐

| Structure | Access Rule | Java Class | Key Ops |
|---|---|---|---|
| Stack | LIFO — same end | `ArrayDeque` | `push`, `pop`, `peek` |
| Queue | FIFO — opposite ends | `LinkedList` / `ArrayDeque` | `offer`, `poll`, `peek` |
| Deque | Both ends | `ArrayDeque` | `addFirst/Last`, `removeFirst/Last` |
| Priority Queue | By priority, not order | `PriorityQueue` | `offer`, `poll` (O(log n)) |

---

## Time Complexity — Quick Table

| Operation | Stack | Queue | Deque | Priority Queue |
|---|---|---|---|---|
| Insert | O(1) | O(1) | O(1) | O(log n) |
| Remove | O(1) | O(1) | O(1) | O(log n) |
| Peek | O(1) | O(1) | O(1) | O(1) |
| Search | O(n) | O(n) | O(n) | O(n) |

---

## Quick Revision

- **Stack** → LIFO, use `ArrayDeque` with `push`/`pop`/`peek`.
- **Queue** → FIFO, use `offer`/`poll`/`peek` (avoid `add`/`remove` — they throw on failure).
- **Deque** → both ends open, backbone of stack, queue, *and* the sliding-window-max pattern.
- **Priority Queue** → backed by a binary heap, O(log n) insert/remove, head is always min (or max, with a comparator) — but iteration order is NOT sorted.
- `ArrayDeque` is the modern go-to for both Stack and Deque behavior.

---

## Interview Definition ⭐

> **Stack, Queue, and Deque are linear structures distinguished purely by which ends allow insertion/removal — LIFO for Stack, FIFO for Queue, both ends for Deque — all offering O(1) operations, while Priority Queue instead orders removal by priority via an internal binary heap, trading O(1) for O(log n) insert/remove in exchange for always serving the min or max element first.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | Easy | The canonical stack use case |
| 2 | [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) | Easy | Tests real understanding of LIFO vs FIFO |
| 3 | [Min Stack](https://leetcode.com/problems/min-stack/) | Medium | Augmented stack design, O(1) min tracking |
| 4 | [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | Hard | Monotonic deque — a must-know deque pattern |
| 5 | [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) | Easy | PriorityQueue for maintaining top-k |
| 6 | [Design Circular Queue](https://leetcode.com/problems/design-circular-queue/) | Medium | Building queue mechanics from scratch |
| 7 | [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | Hard | PriorityQueue applied to merge multiple sorted sources |
