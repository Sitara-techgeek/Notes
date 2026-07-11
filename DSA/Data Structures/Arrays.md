# Arrays

> **📅 Written:** 11 Jul 2026

### Definition

**An Array is a linear data structure that stores a fixed-size collection of elements of the same type in contiguous memory locations.**

**In one sentence:**
> An array is a row of boxes, all the same size, sitting right next to each other in memory, and each box has a number (index) so you can jump straight to it.

**Example:**

```
Index:    0    1    2    3    4
        [10] [20] [30] [40] [50]
```

Each element sits at a fixed offset from the start, arrays let you jump directly to any index instantly.

---

## Key Features of Arrays

- Fixed size (in most languages, decided at creation)
- Elements stored in contiguous memory
- Same data type for every element
- Each element accessed by an index (usually starting at 0)
- Random access — O(1) to reach any element by index
- Fast reads, but costly inserts/deletes in the middle

---

## Why Contiguous Memory Matters

Because all elements sit back-to-back in memory, the computer can calculate the exact address of any element using a simple formula:

```
address(i) = base_address + (i * size_of_element)
i = ( adress at i = 0 ) + (i * size of array type)
```

This is **why array access is O(1)** — no searching required, just math.

---

# Types of Arrays


## 1. One-Dimensional Array

### Definition

A simple linear list of elements.

```
int arr[5] = {10, 20, 30, 40, 50};
```

---

## 2. Multi-Dimensional Array

### Definition

An array of arrays — most commonly seen as a 2D array (a grid/table).

```
int matrix[2][3] = {
  {1, 2, 3},
  {4, 5, 6}
};
```

```
        col0  col1  col2
row0 →   1     2     3
row1 →   4     5     6
```

---

## 3. Dynamic Array

### Definition

An array that can **grow or shrink** at runtime (e.g. Python `list`, Java `ArrayList`, C++ `vector`, JS array).

Under the hood, when it runs out of space, it:

1. Allocates a **new, bigger block** of memory (usually double the size).
2. Copies all existing elements over.
3. Frees the old block.

```
Capacity 4 → full → insert triggers resize → Capacity 8
```

> This resize operation is occasionally O(n), but it happens rarely enough that insertion is still considered **O(1) amortized**.

#### What is O(1) amortized ??

In this sentence, **amortized** means **the average cost of an operation when we consider the cost over a long sequence of operations, rather than looking at one operation individually**.

For a dynamic array:

* Most insertions (`push_back` / adding at the end) are **O(1)** because there is empty space available.
* Occasionally, the array becomes full, so it needs to **resize**:

  * Create a bigger array (usually double the size)
  * Copy all existing elements into the new array
  * Add the new element
    This single insertion costs **O(n)**.

Example:

Suppose the capacity doubles:

```
Capacity: 1

Insert 1:
[1]          → O(1)

Insert 2:
Need resize:
[1] → [1,2]  → copy 1 element → O(1)

Insert 3:
Need resize:
[1,2] → [1,2,3, ] → copy 2 elements → O(2)
```

The expensive operations happen at sizes:

```
1, 2, 4, 8, 16, 32...
```

Imagine inserting 16 elements:

* Most insertions cost 1 step.
* Resizing costs:

  * copy 1 element
  * copy 2 elements
  * copy 4 elements
  * copy 8 elements

Total copying work:

```
1 + 2 + 4 + 8 = 15
```

Plus the 16 insertions themselves:

```
16 + 15 = 31 operations
```

For 16 insertions:

```
Total cost = 31
Average cost per insertion = 31/16 ≈ 2
```

Even though **one insertion took O(n)**, when you spread that cost across all insertions, each insertion is effectively **constant time**.

So:

* **Worst-case complexity of one insertion:** O(n)
* **Amortized complexity of insertion over many operations:** O(1)

A simple analogy: if you spend ₹1200 on groceries once a month and ₹0 on other days, your daily average expense is not ₹1200 — you spread that cost over the whole month. Amortized analysis does something similar with algorithm costs.

---

# Array Operations

> **📅 Written:** 11 Jul 2026

## 1. Access

```
arr[2]   // jumps straight to index 2 → O(1)
```

---

## 2. Traversal

```
for i in range(len(arr)):
    print(arr[i])
```

Visits every element once → **O(n)**.

---

## 3. Insertion

**At the end (dynamic array, space available):**

```
arr.append(60)   // O(1)
```

**At the beginning or middle:**

```
Insert 25 at index 1:

Before: [10, 20, 30, 40]
After:  [10, 25, 20, 30, 40]
```

Every element after the insertion point must **shift right by one** → **O(n)**.

---

## 4. Deletion

**From the end:**

```
arr.pop()   // O(1)
```

**From the beginning or middle:**

```
Remove index 1:

Before: [10, 20, 30, 40]
After:  [10, 30, 40]
```

Every element after the deleted index must **shift left by one** → **O(n)**.

---

## 5. Search

**Linear Search** (unsorted array):

```
for i in range(len(arr)):
    if arr[i] == target:
        return i
```

→ **O(n)**

**Binary Search** (sorted array only):

```
low, high = 0, len(arr) - 1
while low <= high:
    mid = (low + high) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        low = mid + 1
    else:
        high = mid - 1
```

→ **O(log n)**

---

## Time Complexity — Quick Table

| Operation                | Time Complexity |
| ------------------------- | ---------------- |
| Access by index           | O(1)              |
| Traversal                 | O(n)              |
| Search (unsorted)         | O(n)              |
| Search (sorted, binary)   | O(log n)          |
| Insert at end             | O(1) amortized    |
| Insert at beginning/middle| O(n)              |
| Delete at end             | O(1)              |
| Delete at beginning/middle| O(n)              |

---

## Why is inserting/deleting in the middle so expensive?

Because memory is contiguous — there's no gap to insert into. Every element after the target position has to physically move over by one slot to keep the array unbroken.

```
Insert at index 1:

[10][20][30][40]        original
[10][ ][20][30][40]     shift everything right of index 1
[10][25][20][30][40]    place new element
```

> **Interview point ⭐:** This is the exact tradeoff arrays make against linked lists — arrays win on random access (O(1)), linked lists win on middle insert/delete (O(1) once you have the node).

---

## Static Array vs Dynamic Array

| Static Array              | Dynamic Array                     |
| -------------------------- | ---------------------------------- |
| Fixed size at creation      | Grows/shrinks automatically        |
| No resizing overhead        | Occasional O(n) resize cost        |
| Wastes no extra memory      | May over-allocate for future growth|
| e.g. `int arr[5]` in C      | e.g. Python `list`, C++ `vector`   |

---

## Quick Revision

- **Array** → Fixed-size, contiguous collection of same-type elements.
- **Access** → O(1), thanks to index-based address calculation.
- **Insert/Delete at end** → O(1) (amortized for dynamic arrays).
- **Insert/Delete at beginning/middle** → O(n), because elements must shift.
- **Search** → O(n) unsorted, O(log n) sorted (binary search).
- **Dynamic Array** → Resizes by allocating a bigger block and copying elements over.

---

## Interview Definition ⭐

> **An array is a linear, index-based data structure that stores elements of the same type in contiguous memory, giving O(1) random access but O(n) cost for insertion or deletion anywhere except the end.**