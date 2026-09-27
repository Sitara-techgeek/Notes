# Trees - Binary Tree and BST Operations

> **📅 Written:** 24 Sep 2026

### Definition

**A Tree is a hierarchical data structure made of nodes connected by edges, with one root node and no cycles. A Binary Tree restricts each node to at most two children; a Binary Search Tree (BST) further orders those children so left < node < right.**

**In one sentence:**
> A tree is a linked list that's allowed to branch — and a BST is a tree that branches in a way that keeps everything sorted, so searching feels like binary search but on pointers instead of an array.

**Example:**

```
Binary Tree (unordered):        Binary Search Tree (ordered):
        5                                5
       / \                              / \
      3   8                            3   8
     /                                / \   \
    9                                1   4   9

(any arrangement allowed)      (left < node < right, everywhere)
```

---

## Node Structure (Java)

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```

---

## Binary Tree — Key Terminology

| Term | Meaning |
|---|---|
| **Root** | The topmost node, with no parent |
| **Leaf** | A node with no children |
| **Height of a node** | Longest path from that node down to a leaf |
| **Depth of a node** | Distance from the root to that node |
| **Height of the tree** | Height of the root |
| **Balanced tree** | Height difference between left and right subtrees is small (usually ≤1) at every node |
| **Complete binary tree** | Every level filled except possibly the last, filled left to right |
| **Full binary tree** | Every node has either 0 or 2 children (never exactly 1) |
| **Perfect binary tree** | All leaves at the same depth, every internal node has exactly 2 children |

---

## Binary Search Tree (BST) — The Core Property

For **every** node: all values in its **left subtree** are smaller, and all values in its **right subtree** are larger (assuming no duplicates).

```
        5
       / \
      3   8
     / \   \
    1   4   9

Every left value < parent < every right value, at EVERY node, not just the root.
```

> **Interview point ⭐:** The BST property must hold across the **entire** subtree, not just the immediate children — this is exactly the trap in "Validate Binary Search Tree", where checking only immediate children isn't sufficient.

---

## BST Operations

### 1. Search — O(h), h = tree height

```java
TreeNode search(TreeNode root, int target) {
    if (root == null || root.val == target) return root;
    return target < root.val ? search(root.left, target) : search(root.right, target);
}
```
→ **O(log n)** for a balanced BST, **O(n)** worst case for a skewed tree (essentially a linked list).

### 2. Insertion — O(h)

```java
TreeNode insert(TreeNode root, int val) {
    if (root == null) return new TreeNode(val);
    if (val < root.val) root.left = insert(root.left, val);
    else root.right = insert(root.right, val);
    return root;
}
```

### 3. Deletion — O(h)

Three cases to handle:
- **Leaf node** — simply remove it
- **One child** — replace the node with its child
- **Two children** — replace the node's value with its **in-order successor** (smallest value in the right subtree), then delete that successor

```java
TreeNode delete(TreeNode root, int key) {
    if (root == null) return null;
    if (key < root.val) root.left = delete(root.left, key);
    else if (key > root.val) root.right = delete(root.right, key);
    else {
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;
        TreeNode successor = findMin(root.right);
        root.val = successor.val;
        root.right = delete(root.right, successor.val);
    }
    return root;
}

TreeNode findMin(TreeNode node) {
    while (node.left != null) node = node.left;
    return node;
}
```

---

## Why BST Height Matters So Much ⭐

```
Balanced BST (height = log n):        Skewed BST (height = n):
        4                                   1
      /   \                                  \
     2     6                                  2
    / \   / \                                  \
   1  3  5   7                                  3
                                                  \
                                                   4

Search: O(log n)                       Search: O(n) - degrades to a linked list!
```

> **Interview point ⭐:** A plain BST offers **no guarantee** of balance — inserting sorted data in order produces a completely skewed tree, degrading every operation to O(n). This is exactly why **self-balancing BSTs** (AVL Trees, Red-Black Trees) exist — Java's `TreeMap`/`TreeSet` use a Red-Black Tree internally to *guarantee* O(log n).

---

## Height, Balance, and Common Checks

```java
int height(TreeNode root) {
    if (root == null) return -1;   // or 0, depending on convention — be consistent!
    return 1 + Math.max(height(root.left), height(root.right));
}

boolean isBalanced(TreeNode root) {
    return checkHeight(root) != -1;
}

int checkHeight(TreeNode root) {
    if (root == null) return 0;
    int left = checkHeight(root.left);
    if (left == -1) return -1;
    int right = checkHeight(root.right);
    if (right == -1) return -1;
    if (Math.abs(left - right) > 1) return -1;
    return 1 + Math.max(left, right);
}
```
→ The "bottom-up" version above computes height and checks balance **in a single pass**, O(n) — much better than a naive approach that recomputes height at every node (O(n log n) or O(n²)).

### Validate BST — The Right Way

```java
boolean isValidBST(TreeNode root) {
    return validate(root, null, null);
}

boolean validate(TreeNode node, Integer lower, Integer upper) {
    if (node == null) return true;
    if (lower != null && node.val <= lower) return false;
    if (upper != null && node.val >= upper) return false;
    return validate(node.left, lower, node.val) && validate(node.right, node.val, upper);
}
```
→ Passes down a **valid range** at every recursive call — this is the correct approach, since checking only immediate parent-child relationships misses violations further down the tree.

---

## Time Complexity — Quick Table

| Operation | Balanced BST | Skewed BST (worst case) |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Find min/max | O(log n) | O(n) |
| Height calculation | O(n) | O(n) |

---

## Quick Revision

- **Binary Tree** → each node has ≤2 children, no ordering guarantee.
- **BST** → ordered: left subtree < node < right subtree, at every node (not just immediate children).
- **Search/Insert/Delete** → O(log n) balanced, O(n) worst case (skewed).
- **Deletion** → 3 cases: leaf, one child, two children (use in-order successor).
- Plain BSTs have **no self-balancing guarantee** — that's what AVL/Red-Black Trees solve (Java's `TreeMap`/`TreeSet`).
- **Validate BST** correctly by passing down a valid `(lower, upper)` range, not just comparing to immediate children.

---

## Interview Definition ⭐

> **A Binary Search Tree is a binary tree where every node's left subtree contains only smaller values and its right subtree only larger values, giving O(log n) search/insert/delete when the tree is balanced — but with no self-balancing guarantee built in, a poorly-inserted BST can degrade to O(n), which is precisely why self-balancing variants like Red-Black Trees exist underneath structures like Java's TreeMap.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) | Medium | The range-passing technique, a very common trap problem |
| 2 | [Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/) | Medium | Direct BST insertion practice |
| 3 | [Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/) | Medium | All three deletion cases in one problem |
| 4 | [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | Medium | Exploits the BST ordering property directly |
| 5 | [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) | Medium | In-order traversal + BST property |
| 6 | [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/) | Easy | The single-pass height+balance check pattern |
| 7 | [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/) | Easy | Building a balanced BST from sorted data |
