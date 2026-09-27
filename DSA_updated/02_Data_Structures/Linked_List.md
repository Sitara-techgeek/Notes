# Linked List - Implementation and Application

> **📅 Written:** 24 Sep 2026

### Definition

**A Linked List is a linear data structure where elements (nodes) are stored in separate memory locations, each holding data plus a reference (pointer) to the next node — trading an array's O(1) random access for O(1) insertion/deletion once the position is known.**

**In one sentence:**
> Unlike an array's row of neighbouring boxes, a linked list is a chain of boxes scattered anywhere in memory, each one holding a note that says where the next box is.

**Example:**

```
[10 | •]--->[20 | •]--->[30 | •]--->[40 | NULL]
```

---

## Node Structure (Java)

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; this.next = null; }
}
```

A linked list itself is just a reference to the **first node**, called the **head**.

```java
ListNode head = new ListNode(10);
head.next = new ListNode(20);
head.next.next = new ListNode(30);
// head -> [10] -> [20] -> [30] -> null
```

---

## Types of Linked Lists

### Singly Linked List
Each node points only to the **next** node. One-directional traversal.
```
[10]---->[20]---->[30]---->NULL
```

### Doubly Linked List
Each node points to **both** next and previous, allowing two-directional traversal.
```
NULL<----[10]<--->[20]<--->[30]---->NULL
```
```java
class DListNode {
    int val;
    DListNode next, prev;
    DListNode(int val) { this.val = val; }
}
```

### Circular Linked List
The last node points back to the **first**, forming a loop. Can be singly or doubly circular.
```
[10]---->[20]---->[30]---->(back to 10)
   ↑___________________________|
```

---

## Core Operations (Implementation)

### 1. Traversal — O(n)
```java
ListNode current = head;
while (current != null) {
    System.out.println(current.val);
    current = current.next;
}
```

### 2. Insertion at the Beginning — O(1)
```java
ListNode newNode = new ListNode(5);
newNode.next = head;
head = newNode;
```

### 3. Insertion at the End — O(n), or O(1) with a tail pointer
```java
ListNode current = head;
while (current.next != null) current = current.next;
current.next = newNode;
```

### 4. Insertion in the Middle — O(1) once positioned, O(n) to find the spot
```java
newNode.next = prevNode.next;
prevNode.next = newNode;
```

### 5. Deletion — O(1) once positioned, O(n) to find the spot
```java
prevNode.next = prevNode.next.next;
```

### 6. Search — O(n)
```java
ListNode current = head;
while (current != null) {
    if (current.val == target) return current;
    current = current.next;
}
return null;
```

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Access by index | O(n) |
| Traversal / Search | O(n) |
| Insert at beginning | O(1) |
| Insert at end (no tail pointer) | O(n) |
| Insert at end (with tail pointer) | O(1) |
| Insert/Delete (node known) | O(1) |

---

## Arrays vs Linked Lists ⭐

| Array | Linked List |
|---|---|
| Contiguous memory | Scattered memory, linked by pointers |
| Fixed size (unless dynamic) | Grows/shrinks freely |
| O(1) random access | O(n) access — must traverse |
| O(n) insert/delete in middle | O(1) insert/delete once node is found |
| No extra memory per element | Extra memory for storing pointers |

> **Interview point ⭐:** Arrays are fast to *read*, linked lists are fast to *insert/delete* (once you're already at the right spot).

---

## Applications — Essential Patterns

### 1. Reverse a Linked List

Iteratively re-point each node's `next` to the previous node.

```
Before: 1 -> 2 -> 3 -> null
After:  null <- 1 <- 2 <- 3
```

```java
ListNode reverse(ListNode head) {
    ListNode prev = null, current = head;
    while (current != null) {
        ListNode nextTemp = current.next;
        current.next = prev;
        prev = current;
        current = nextTemp;
    }
    return prev;   // new head
}
```
→ **O(n) time, O(1) space.** One of the most frequently asked linked-list questions — memorize this cold.

---

### 2. Cycle Detection — Floyd's Tortoise and Hare

Two pointers move at different speeds; if there's a cycle, they **must** eventually meet.

```java
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```
→ **O(n) time, O(1) space** — beats using a HashSet to track visited nodes (which needs O(n) extra space).

**Finding the cycle's start:** once `slow == fast`, reset one pointer to `head` and move both one step at a time — they meet exactly at the cycle's starting node (a neat proof from the math of the two pointers' relative distances).

---

### 3. Finding the Middle Node — Fast & Slow Pointers

```java
ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;   // middle node (2nd middle if even length)
}
```
→ When `fast` reaches the end, `slow` is exactly at the middle — no need to count length first.

---

### 4. Merging Two Sorted Lists

```java
ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) { tail.next = l1; l1 = l1.next; }
        else { tail.next = l2; l2 = l2.next; }
        tail = tail.next;
    }
    tail.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```
→ **O(n + m)**. The **dummy node** trick (a placeholder head) is a very common idiom to avoid special-casing the "first insertion".

---

### 5. Removing the Nth Node From the End — One-Pass Two Pointers

```java
ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode fast = dummy, slow = dummy;
    for (int i = 0; i < n; i++) fast = fast.next;   // move fast n steps ahead
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;   // skip the target node
    return dummy.next;
}
```
→ **O(n)**, single pass — the gap between `fast` and `slow` does the counting for you.

---

## Quick Revision

- **Linked List** → chain of nodes, each pointing to the next; **head** = entry point.
- **Singly / Doubly / Circular** — direction and looping variants.
- **Insert/Delete** → O(1) once you have the node, O(n) to find it.
- **Reverse** → iterative pointer re-pointing, O(n)/O(1) — must-know.
- **Fast & Slow pointers** → cycle detection, middle-node finding — same technique, two uses.
- **Dummy node** → idiom to simplify edge cases in insertion/merging/deletion.

---

## Interview Definition ⭐

> **A linked list is a linear data structure made of nodes scattered in memory, where each node holds data plus a reference to the next node — trading an array's O(1) random access for O(1) insertion/deletion once the position is known; most linked-list interview problems reduce to pointer manipulation via fast/slow pointers, dummy nodes, or in-place reversal.**

---

## A Quick Side Note: `List` vs `LinkedList` vs `ArrayList`

- `List` is an interface — `LinkedList` and `ArrayList` both implement it.
- Java's `LinkedList` is a **doubly linked list**, and also implements `Deque` — so it can be used as a Stack, Queue, or Deque directly.
```java
List<Integer> list = new LinkedList<>();     // ✅ valid, doubly-linked list
Deque<Integer> deque = new LinkedList<>();   // ✅ also valid — LinkedList implements Deque too
```

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | Easy | The single most important linked-list problem to know cold |
| 2 | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | Easy | Floyd's Tortoise and Hare |
| 3 | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | Medium | Finding the cycle's starting node |
| 4 | [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) | Easy | Fast & slow pointer pattern |
| 5 | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | Easy | Dummy node + two-pointer merge |
| 6 | [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | Medium | One-pass two-pointer trick |
| 7 | [Reorder List](https://leetcode.com/problems/reorder-list/) | Medium | Combines middle-finding + reversal + merging — a great synthesis problem |
| 8 | [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) | Medium | HashMap-assisted deep copy of a complex linked structure |
| 9 | [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | Hard | Extends the 2-list merge using a Priority Queue |
