# Priority Queue and Binary Heap

> **📅 Written:** 24 Sep 2026

### Definition

**A Binary Heap is a complete binary tree stored in an array, satisfying the heap property (every parent is ≤ or ≥ its children) — it's the data structure that powers Java's `PriorityQueue`, giving O(log n) insertion and removal while always keeping the min (or max) instantly accessible.**

**In one sentence:**
> A heap is a tree shaped like a triangle, filled in strictly left-to-right, level-by-level, where every parent is always "better" (smaller or larger, depending on type) than its children — but siblings aren't compared to each other at all.

**Example (Min-Heap):**

```
          1
        /   \
       3     5
      / \   /
     8   9 6
```
Every parent ≤ its children. Note: `8` and `9` (both under `3`) aren't compared to `5`, `6` — only ancestor/descendant relationships are guaranteed.

---

## Complete Binary Tree — The Shape Rule

A heap must always be a **complete binary tree**: every level is fully filled except possibly the last, which fills **left to right** with no gaps. This is exactly what makes array storage work.

```
Array:   [1, 3, 5, 8, 9, 6]
Index:    0  1  2  3  4  5

For a node at index i:
  parent(i)      = (i - 1) / 2
  leftChild(i)   = 2*i + 1
  rightChild(i)  = 2*i + 2
```

```
          1(0)
        /      \
      3(1)     5(2)
     /   \     /
   8(3) 9(4) 6(5)
```

→ No pointers needed at all — just index arithmetic, which is why heaps are compact and cache-friendly compared to pointer-based trees.

---

## Min-Heap vs Max-Heap

| Min-Heap | Max-Heap |
|---|---|
| Every parent ≤ its children | Every parent ≥ its children |
| Root is always the **smallest** element | Root is always the **largest** element |
| Java `PriorityQueue<>()` default | `PriorityQueue<>(Collections.reverseOrder())` |

---

## Core Operations (How the Heap Maintains Itself)

### 1. Insertion (Push) — "Sift Up" / "Bubble Up"

Add the new element at the **next available leaf position** (end of the array), then repeatedly swap it with its parent while it violates the heap property.

```
Insert 2 into: [1, 3, 5, 8, 9, 6]

Step 1: Place at end:     [1, 3, 5, 8, 9, 6, 2]
Step 2: Compare with parent(5)=idx2: 2 < 5, swap
        [1, 3, 2, 8, 9, 6, 5]
Step 3: Compare with parent(1)=idx0: 2 > 1, stop
Final: [1, 3, 2, 8, 9, 6, 5]
```

```java
void siftUp(int[] heap, int i) {
    while (i > 0) {
        int parent = (i - 1) / 2;
        if (heap[i] >= heap[parent]) break;   // min-heap: stop if order is satisfied
        swap(heap, i, parent);
        i = parent;
    }
}
```
→ **O(log n)** — the height of a complete binary tree with n nodes is O(log n).

### 2. Deletion (Poll) — "Sift Down" / "Heapify Down"

Remove the **root** (always the min/max), move the **last element** to the root position, then repeatedly swap it down with its smaller (min-heap) child until the heap property holds.

```
Remove min from: [1, 3, 2, 8, 9, 6, 5]

Step 1: Move last element to root: [5, 3, 2, 8, 9, 6]
Step 2: Compare with children (3, 2): 2 is smaller, swap
        [2, 3, 5, 8, 9, 6]
Step 3: 5 has no children (leaf), stop
Final: [2, 3, 5, 8, 9, 6]
```

```java
void siftDown(int[] heap, int i, int size) {
    while (true) {
        int left = 2*i + 1, right = 2*i + 2, smallest = i;
        if (left < size && heap[left] < heap[smallest]) smallest = left;
        if (right < size && heap[right] < heap[smallest]) smallest = right;
        if (smallest == i) break;
        swap(heap, i, smallest);
        i = smallest;
    }
}
```
→ **O(log n)**.

### 3. Peek — O(1)

The root (`heap[0]`) is always the min/max — no traversal needed.

### 4. Build Heap from an Existing Array — Heapify

Naively inserting n elements one by one costs O(n log n). Instead, calling `siftDown` on every non-leaf node **bottom-up** builds the whole heap in **O(n)** — a classic, slightly counter-intuitive result (the math works out because most nodes are near the bottom, where sift-down does very little work).

```java
void buildHeap(int[] arr) {
    int n = arr.length;
    for (int i = n / 2 - 1; i >= 0; i--) {   // start from the last non-leaf node
        siftDown(arr, i, n);
    }
}
```

> **Interview point ⭐:** "Insert n elements one by one" is O(n log n). "Heapify an existing array" is O(n). This distinction is a favorite complexity-analysis interview question.

---

## Java's `PriorityQueue` — Using the Built-In Implementation

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

minHeap.offer(5);
minHeap.offer(1);
minHeap.offer(3);
minHeap.poll();     // 1

// Custom comparator, e.g. for int[] pairs sorted by the second value
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);

// Building from an existing collection - uses O(n) heapify internally
PriorityQueue<Integer> pq2 = new PriorityQueue<>(List.of(5, 1, 8, 3));
```

> **Watch out ⭐:** Iterating a `PriorityQueue` directly (`for (int x : pq)`) does **NOT** give sorted order — it walks the internal array in heap-storage order, not sorted order. Only repeated `poll()` calls guarantee sorted output.

---

## Common Application: Top-K Problems

The **"keep a heap of size k"** pattern is one of the most frequently reused heap tricks — maintain a min-heap of size k for the k *largest* elements (counter-intuitively, a min-heap, so the smallest of the current top-k sits at the root ready to be evicted).

```java
int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll();   // evict the smallest, keeping only the top k
        }
    }
    return minHeap.peek();
}
```
→ **O(n log k)** — much better than sorting the whole array (O(n log n)) when k is small.

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Peek (min/max) | O(1) |
| Insert (offer/push) | O(log n) |
| Remove (poll/pop) | O(log n) |
| Build heap from array (heapify) | O(n) |
| Insert n elements one-by-one | O(n log n) |
| Search for arbitrary element | O(n) — no ordering guarantee beyond parent/child |

---

## Quick Revision

- **Heap** → complete binary tree stored as an array; parent/child found via index math, no pointers.
- **Min-Heap** → parent ≤ children, root is minimum. **Max-Heap** → parent ≥ children, root is maximum.
- **Sift Up** (insertion) and **Sift Down** (deletion) both run in **O(log n)**.
- **Heapify** an existing array → **O(n)**, faster than inserting one by one (O(n log n)).
- Iterating a `PriorityQueue` is **not** sorted order — only repeated `poll()` is.
- **Top-K pattern** → maintain a heap of size k, O(n log k).

---

## Interview Definition ⭐

> **A binary heap is a complete binary tree stored compactly in an array, maintaining the invariant that every parent is smaller (min-heap) or larger (max-heap) than its children — insertion and removal both run in O(log n) via sift-up/sift-down, while building a heap from an existing array runs in O(n) using bottom-up heapify, making it the ideal structure whenever you need repeated access to the current min/max, such as in top-k problems or priority-based scheduling.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | Medium | The core top-k-heap pattern |
| 2 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Medium | Heap combined with frequency counting |
| 3 | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | Medium | Heap with a custom distance comparator |
| 4 | [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | Hard | Heap used to merge multiple sorted sources efficiently |
| 5 | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | Hard | Two-heap design (max-heap + min-heap) — a classic advanced heap problem |
| 6 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | Medium | Max-heap driven greedy scheduling |
| 7 | [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/) | Medium | Heap (or DP) for generating ordered sequences |
