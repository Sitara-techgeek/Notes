# Linked Lists

> **📅 Written:** 11 Jul 2026

### Definition

**A Linked List is a linear data structure where elements (nodes) are stored in separate memory locations, and each node points to the next one using a reference (pointer).**

**In one sentence:**
> Unlike an array's row of neighbouring boxes, a linked list is a chain of boxes scattered anywhere in memory, each one holding a note that says where the next box is.

**Example:**

```
[10 | •]--->[20 | •]--->[30 | •]--->[40 | NULL]
```

Each box (node) has two parts: the **data** and a **pointer** to the next node. The last node points to `NULL`, marking the end.

---

## Key Features of Linked Lists

- Elements (nodes) are **not** stored contiguously in memory
- Each node stores data + a reference to the next node
- No fixed size — grows and shrinks freely at runtime
- No random access — must traverse from the head to reach a node
- Insertion/deletion at the beginning or middle is fast, since nothing needs to shift

---

## Node Structure

```
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

A linked list itself is just a reference to the **first node**, called the **head**.

```
head ---> [10|•] ---> [20|•] ---> [30|NULL]
```

---

# Types of Linked Lists

> **📅 Written:** 11 Jul 2026

## 1. Singly Linked List

### Definition

Each node points only to the **next** node. Traversal is one-directional.

```
[10]---->[20]---->[30]---->NULL
```

---

## 2. Doubly Linked List

### Definition

Each node points to **both** the next and the previous node, allowing traversal in either direction.

```
NULL<----[10]<--->[20]<--->[30]---->NULL
```

```
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
        self.prev = None
```

---

## 3. Circular Linked List

### Definition

The last node points back to the **first node** instead of `NULL`, forming a loop. Can be singly or doubly circular.

```
[10]---->[20]---->[30]---->(back to 10)
   ↑___________________________|
```

---

# Linked List Operations

> **📅 Written:** 11 Jul 2026

## 1. Traversal

```
current = head
while current is not None:
    print(current.data)
    current = current.next
```

Visits every node once → **O(n)**, since you must follow pointers one at a time — no jumping to an index like an array.

---

## 2. Insertion at the Beginning

```
Before: head -> [20] -> [30] -> NULL
Insert 10:
new_node.next = head
head = new_node
After:  head -> [10] -> [20] -> [30] -> NULL
```

→ **O(1)** — no shifting required, just re-point the head.

---

## 3. Insertion at the End

```
current = head
while current.next is not None:
    current = current.next
current.next = new_node
```

→ **O(n)** for a singly linked list (must walk to the last node), unless you keep a **tail pointer**, which brings it down to **O(1)**.

---

## 4. Insertion in the Middle

```
Before: [10]->[20]->[40]
Insert 30 after 20:
new_node.next = current.next
current.next = new_node
After:  [10]->[20]->[30]->[40]
```

→ **O(1)** to insert **once you already have a reference to the node** — the expensive part is finding that node, which is O(n).

---

## 5. Deletion

```
Before: [10]->[20]->[30]
Delete 20:
prev.next = current.next
After:  [10]->[30]
```

Same idea as insertion: deleting is O(1) once you have the node, but finding it takes O(n).

---

## 6. Search

```
current = head
while current is not None:
    if current.data == target:
        return current
    current = current.next
```

→ **O(n)** — always linear, since there's no indexing.

---

## Time Complexity — Quick Table

| Operation                     | Time Complexity |
| ------------------------------ | ---------------- |
| Access by index                | O(n)              |
| Traversal                      | O(n)              |
| Search                         | O(n)              |
| Insert at beginning             | O(1)              |
| Insert at end (no tail pointer) | O(n)              |
| Insert at end (with tail pointer)| O(1)             |
| Insert in middle (node known)  | O(1)              |
| Delete (node known)            | O(1)              |

---

## Arrays vs Linked Lists ⭐

| Array                          | Linked List                        |
| -------------------------------- | ------------------------------------ |
| Contiguous memory                | Scattered memory, linked by pointers |
| Fixed size (unless dynamic)      | Grows/shrinks freely                 |
| O(1) random access               | O(n) access — must traverse          |
| O(n) insert/delete in middle     | O(1) insert/delete once node is found|
| No extra memory per element      | Extra memory for storing pointers    |

> **Interview point ⭐:** This is the classic tradeoff — arrays are fast to *read*, linked lists are fast to *insert/delete* (once you're already at the right spot).

---

## Quick Revision

- **Linked List** → Chain of nodes, each pointing to the next.
- **Head** → Reference to the first node; entry point of the list.
- **Singly** → One direction (next only).
- **Doubly** → Both directions (next + prev).
- **Circular** → Last node loops back to the first.
- **Traversal/Search/Access** → O(n), no random access.
- **Insert/Delete** → O(1) once you have the node reference, O(n) to find it.

---

## Interview Definition ⭐

> **A linked list is a linear data structure made of nodes scattered in memory, where each node holds data plus a reference to the next node — trading an array's O(1) random access for O(1) insertion/deletion once the position is known.**

---

## A Quick Side Note: `List` vs `ArrayList`

> **📅 Written:** 11 Jul 2026


- **`List`** is an **interface**, not a class. You **cannot** do `new List()` — an interface has no implementation of its own, so it can't be instantiated directly.
- **`ArrayList`** (and `LinkedList`) is a **class** that **implements** the `List` interface. That's what you actually instantiate to create a usable list object.

```java
List<Integer> list = new ArrayList<>();   // ✅ valid
List<Integer> list = new LinkedList<>();  // ✅ valid - ArrayList and LinkedList are List implementations
List<Integer> list = new List<>();        // ❌ invalid — List is an interface
```

> **One-liner to remember:** *You program to the interface (`List`), but you create the object from a class that implements it (`ArrayList`).*