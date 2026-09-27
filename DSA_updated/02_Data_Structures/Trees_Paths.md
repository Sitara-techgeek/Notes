# Trees - Paths

> **📅 Written:** 24 Sep 2026

### Definition

**Tree Path problems ask about routes through a tree — root-to-leaf paths, any-node-to-any-node paths, or properties derived from them like sum, length, or the diameter — almost all solved using DFS with information passed down (or returned up) the recursion.**

**In one sentence:**
> Most tree-path problems boil down to one question: "does this need information passed DOWN to children, or bubbled UP from children?" — get that right and the recursion writes itself.

---

## Two Fundamental Recursion Shapes

### Shape 1: Information Passed DOWN (top-down / "carry a state")

Used for **root-to-leaf** path problems — carry accumulated info as a parameter.

```java
void dfs(TreeNode node, int currentSum, List<Integer> result) {
    if (node == null) return;
    currentSum += node.val;
    if (node.left == null && node.right == null) {   // leaf reached
        result.add(currentSum);
        return;
    }
    dfs(node.left, currentSum, result);
    dfs(node.right, currentSum, result);
}
```

### Shape 2: Information Returned UP (bottom-up / "combine children's results")

Used for **any-node-to-any-node** problems, or anything depending on subtree properties — compute children's answers first, then combine.

```java
int dfsReturnsUp(TreeNode node) {
    if (node == null) return 0;
    int left = dfsReturnsUp(node.left);
    int right = dfsReturnsUp(node.right);
    return 1 + Math.max(left, right);   // e.g. height calculation
}
```

> **Interview point ⭐:** Whenever the problem says "root-to-leaf", think top-down (pass state as a parameter). Whenever it says "any path" or "diameter" or involves comparing left vs right subtrees, think bottom-up (return a value, combine at each node).

---

## Root-to-Leaf Path Problems

### Path Sum — Does Any Root-to-Leaf Path Sum to a Target?

```java
boolean hasPathSum(TreeNode root, int target) {
    if (root == null) return false;
    if (root.left == null && root.right == null) return root.val == target;
    int remaining = target - root.val;
    return hasPathSum(root.left, remaining) || hasPathSum(root.right, remaining);
}
```

### All Root-to-Leaf Paths (as strings)

```java
List<String> binaryTreePaths(TreeNode root) {
    List<String> result = new ArrayList<>();
    dfs(root, "", result);
    return result;
}

void dfs(TreeNode node, String path, List<String> result) {
    if (node == null) return;
    path += (path.isEmpty() ? "" : "->") + node.val;
    if (node.left == null && node.right == null) {
        result.add(path);
        return;
    }
    dfs(node.left, path, result);
    dfs(node.right, path, result);
}
```

---

## Any-Node-to-Any-Node Problems (Bottom-Up)

### Diameter of a Binary Tree

The **diameter** is the longest path between any two nodes — which may or may not pass through the root. At each node, the "path through this node" = left height + right height; track the max across all nodes while still returning height (for the parent's calculation).

```
        1
       / \
      2   3
     / \
    4   5

Diameter = 3 (path: 4 -> 2 -> 5, going through node 2, length = 2+1 edges... counted as node count - 1 or edge count depending on definition)
```

```java
int diameter = 0;

int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return diameter;
}

int height(TreeNode node) {
    if (node == null) return 0;
    int left = height(node.left);
    int right = height(node.right);
    diameter = Math.max(diameter, left + right);   // update global answer at EVERY node
    return 1 + Math.max(left, right);                // return height, for the parent to use
}
```

> **Interview point ⭐:** This is the defining pattern for "any path" tree problems — maintain a **global/instance variable** that gets updated at every node (checking "what if the best path passes through here?"), while the recursive function itself returns something else (height) that the parent actually needs.

### Maximum Path Sum (Any Node to Any Node)

Same shape as diameter, but with sums instead of heights, and negative values must be clamped to 0 (don't include a negative-contributing subtree).

```java
int maxSum = Integer.MIN_VALUE;

int maxPathSum(TreeNode root) {
    maxGain(root);
    return maxSum;
}

int maxGain(TreeNode node) {
    if (node == null) return 0;
    int leftGain = Math.max(maxGain(node.left), 0);    // ignore negative contributions
    int rightGain = Math.max(maxGain(node.right), 0);
    maxSum = Math.max(maxSum, node.val + leftGain + rightGain);   // path THROUGH this node
    return node.val + Math.max(leftGain, rightGain);               // best path continuing UP
}
```

---

## Lowest Common Ancestor (LCA)

The LCA of two nodes is the deepest node that has **both** as descendants (a node can be its own ancestor).

### LCA in a General Binary Tree

```java
TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) return root;   // p and q found on DIFFERENT sides
    return (left != null) ? left : right;               // both on the SAME side (or not found)
}
```
→ **O(n)** — must potentially visit every node.

### LCA in a BST (Faster, Using the Ordering)

```java
TreeNode lowestCommonAncestorBST(TreeNode root, TreeNode p, TreeNode q) {
    if (p.val < root.val && q.val < root.val) return lowestCommonAncestorBST(root.left, p, q);
    if (p.val > root.val && q.val > root.val) return lowestCommonAncestorBST(root.right, p, q);
    return root;   // split point found - this IS the LCA
}
```
→ **O(h)** — exploits BST ordering to skip entire subtrees, no need to search both sides blindly.

---

## Time Complexity — Quick Table

| Problem Type | Time | Space (recursion) |
|---|---|---|
| Root-to-leaf path sum/collection | O(n) | O(h) |
| Diameter / Max Path Sum (any-to-any) | O(n) | O(h) |
| LCA (general binary tree) | O(n) | O(h) |
| LCA (BST) | O(h) | O(h) |

---

## Quick Revision

- **Top-down recursion** (pass state as a parameter) → root-to-leaf path problems.
- **Bottom-up recursion** (return a value, combine at parent) → subtree-dependent computations.
- **Diameter/Max Path Sum** → global variable updated at every node ("what if the best answer passes through here?"), while the return value serves the parent's needs.
- **LCA (general tree)** → O(n), based on which side(s) return non-null.
- **LCA (BST)** → O(h), exploits ordering to eliminate one subtree entirely at each step.

---

## Interview Definition ⭐

> **Tree path problems split into two recursion shapes: top-down, where state (like accumulated sum) is passed as a parameter for root-to-leaf questions, and bottom-up, where each call returns a value that its parent combines — used for any-node-to-any-node problems like diameter or max path sum, typically alongside a global variable tracking the best answer found so far across all nodes.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Path Sum](https://leetcode.com/problems/path-sum/) | Easy | The basic top-down root-to-leaf pattern |
| 2 | [Path Sum II](https://leetcode.com/problems/path-sum-ii/) | Medium | Collecting all valid paths, not just checking existence |
| 3 | [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) | Easy | The defining bottom-up "global variable" pattern |
| 4 | [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) | Hard | Same pattern as diameter, with negative-value clamping |
| 5 | [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) | Medium | The general-tree LCA pattern |
| 6 | [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | Medium | The faster BST-specific LCA using ordering |
| 7 | [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/) | Medium | Top-down path accumulation, slightly different aggregation |
