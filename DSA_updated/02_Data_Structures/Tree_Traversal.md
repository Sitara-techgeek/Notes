# Tree Traversal

> **📅 Written:** 24 Sep 2026

### Definition

**Tree Traversal is the process of visiting every node in a tree exactly once, in a defined order — the two broad families are Depth-First (go deep before wide) and Breadth-First (go wide before deep).**

**In one sentence:**
> DFS dives all the way down one branch before backtracking; BFS sweeps across each level fully before moving to the next.

**Example tree used throughout:**
```
        1
       / \
      2   3
     / \
    4   5
```

---

## Depth-First Traversals (Recursive)

All three DFS orders visit the same nodes — they differ only in **when** the current node is processed relative to its children.

### 1. Pre-order — Node → Left → Right

```java
void preorder(TreeNode root, List<Integer> result) {
    if (root == null) return;
    result.add(root.val);           // visit node FIRST
    preorder(root.left, result);
    preorder(root.right, result);
}
// Output: 1, 2, 4, 5, 3
```
> Use when you need to **process a node before its children** — e.g. copying/serializing a tree (you need the root first to reconstruct it).

### 2. In-order — Left → Node → Right

```java
void inorder(TreeNode root, List<Integer> result) {
    if (root == null) return;
    inorder(root.left, result);
    result.add(root.val);           // visit node BETWEEN children
    inorder(root.right, result);
}
// Output: 4, 2, 5, 1, 3
```
> **Interview point ⭐:** In-order traversal of a **BST** visits nodes in **sorted ascending order** — this is one of the most useful facts in tree interviews (used for validating a BST, finding the kth smallest, etc.).

### 3. Post-order — Left → Right → Node

```java
void postorder(TreeNode root, List<Integer> result) {
    if (root == null) return;
    postorder(root.left, result);
    postorder(root.right, result);
    result.add(root.val);           // visit node LAST
}
// Output: 4, 5, 2, 3, 1
```
> Use when children must be **fully processed before the parent** — e.g. deleting a tree (delete children before the node itself), or computing subtree-dependent values like height/diameter.

---

## Depth-First Traversals (Iterative, using an explicit Stack)

Recursion uses the **call stack** implicitly — these do the same thing with an explicit `Deque` as a stack, useful when recursion depth could cause a `StackOverflowError` on very deep trees, or when an interviewer specifically asks for the iterative version.

### Iterative Pre-order

```java
List<Integer> preorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    Deque<TreeNode> stack = new ArrayDeque<>();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        result.add(node.val);
        if (node.right != null) stack.push(node.right);   // push right FIRST
        if (node.left != null) stack.push(node.left);      // so left is popped first
    }
    return result;
}
```

### Iterative In-order

```java
List<Integer> inorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode current = root;
    while (current != null || !stack.isEmpty()) {
        while (current != null) {         // go as far left as possible
            stack.push(current);
            current = current.left;
        }
        current = stack.pop();
        result.add(current.val);
        current = current.right;          // then explore right
    }
    return result;
}
```

> **Watch out ⭐:** Iterative post-order is the trickiest of the three — a common trick is to do a **modified pre-order** (Node → Right → Left) and then **reverse the result**, which gives Left → Right → Node without needing a more complex two-stack approach.

---

## Breadth-First Traversal (Level-Order)

Uses a **Queue**, not a stack — processes the tree level by level.

```java
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int levelSize = queue.size();          // snapshot the current level's size
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(level);
    }
    return result;
}
// Output: [[1], [2, 3], [4, 5]]
```

→ **O(n)** — every node enqueued and dequeued exactly once.

> **Interview point ⭐:** The `levelSize = queue.size()` snapshot **before** the inner loop is the key trick that separates levels cleanly — without it, you'd just get a flat traversal with no level boundaries.

---

## DFS vs BFS — When to Use Which ⭐

| | DFS | BFS |
|---|---|---|
| Data structure | Stack (or recursion) | Queue |
| Explores | Deep first, one branch fully | Wide first, level by level |
| Good for | Path-related problems, subtree computations, backtracking | Shortest path, level-based problems, "minimum steps" |
| Space (worst case) | O(h) — tree height | O(w) — max width of the tree (can be O(n) for a wide tree) |

> **Interview point ⭐:** For a very **wide** but **shallow** tree, BFS can use more memory than DFS (holding a whole level in the queue). For a very **deep** but **narrow** tree, DFS's recursion could risk a stack overflow. Picking the right one sometimes matters for practical reasons beyond just correctness.

---

## Time Complexity — Quick Table

| Traversal | Time | Space (recursive) | Space (iterative) |
|---|---|---|---|
| Pre-order / In-order / Post-order (DFS) | O(n) | O(h) — call stack | O(h) — explicit stack |
| Level-order (BFS) | O(n) | — | O(w) — queue holds a level |

---

## Quick Revision

- **Pre-order** (Node-Left-Right) → good for copying/serializing a tree.
- **In-order** (Left-Node-Right) → gives **sorted order** for a BST.
- **Post-order** (Left-Right-Node) → good for deletion, subtree-dependent computations.
- **Level-order (BFS)** → uses a Queue, processes level by level, great for shortest-path/level problems.
- Iterative DFS uses an explicit `Deque` as a stack; watch the **push order** (right before left) to get correct pop order.
- DFS space = O(height); BFS space = O(width).

---

## Interview Definition ⭐

> **Tree traversal visits every node exactly once, split into Depth-First orders (pre/in/post-order, differing only in when the node is processed relative to its children) implemented via recursion or an explicit stack, and Breadth-First order (level-order) implemented via a queue — DFS suits path and subtree computations while BFS naturally finds shortest paths and level-based answers, with in-order traversal of a BST specifically yielding sorted output.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/) | Easy | Recursive + iterative in-order, foundational |
| 2 | [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/) | Easy | Pre-order, both forms |
| 3 | [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/) | Easy | Post-order, including the reverse-trick iterative version |
| 4 | [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | Medium | The standard BFS-with-queue template |
| 5 | [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/) | Medium | BFS variant, alternating direction per level |
| 6 | [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | Medium | Deep understanding of what pre-order and in-order actually encode |
| 7 | [Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/) | Hard | Combines BFS/DFS with coordinate tracking — great stretch problem |
