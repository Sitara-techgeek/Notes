# Stack and Queue - Application

> **📅 Written:** 24 Sep 2026

### Definition

**Beyond their basic push/pop and enqueue/dequeue mechanics, Stacks and Queues power a set of recurring problem-solving patterns — Monotonic Stacks, Expression Evaluation, and BFS-based traversal being the most important.**

**In one sentence:**
> A Stack remembers "what came before and is still relevant" (great for matching/undo-style problems), while a Queue remembers "what to process next, in order" (great for level-by-level or shortest-path problems).

*(This note assumes familiarity with the basic Stack/Queue/Deque mechanics from the Implementation note — the focus here is entirely on application patterns.)*

---

## 1. Monotonic Stack

### Definition

A stack that is kept either strictly increasing or strictly decreasing from bottom to top, by popping elements that violate the order before pushing a new one. Used to efficiently find the **next/previous greater or smaller element** for every position in an array.

```
Array: [2, 1, 5, 6, 2, 3]

Finding "next greater element" for each position using a monotonic (decreasing) stack:
- Push 2               stack: [2]
- 1 < 2, push 1          stack: [2, 1]
- 5 > 1 -> pop 1 (NGE=5), 5 > 2 -> pop 2 (NGE=5), push 5   stack: [5]
- 6 > 5 -> pop 5 (NGE=6), push 6                            stack: [6]
- 2 < 6, push 2                                              stack: [6, 2]
- 3 > 2 -> pop 2 (NGE=3), 3 < 6, push 3                      stack: [6, 3]
```

```java
int[] nextGreaterElement(int[] arr) {
    int n = arr.length;
    int[] result = new int[n];
    Arrays.fill(result, -1);
    Deque<Integer> stack = new ArrayDeque<>();   // stores INDICES

    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
            result[stack.pop()] = arr[i];
        }
        stack.push(i);
    }
    return result;
}
```
→ **O(n)** — each element is pushed and popped at most once, even though there's a nested-looking loop.

> **Interview point ⭐:** Whenever a problem asks for "next greater/smaller element", "next warmer day", or involves comparing each element to the nearest one before/after it that satisfies a condition — think **Monotonic Stack** immediately. The O(n) trick (vs. an O(n²) brute force) is exactly this pattern.

---

## 2. Expression Evaluation & Parsing

Stacks are the natural fit for anything involving **nested, matched structures** — parentheses, operator precedence, and calculators.

### Valid Parentheses Matching

```java
boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    Map<Character, Character> pairs = Map.of(')', '(', ']', '[', '}', '{');
    for (char c : s.toCharArray()) {
        if (pairs.containsValue(c)) {
            stack.push(c);
        } else if (pairs.containsKey(c)) {
            if (stack.isEmpty() || stack.pop() != pairs.get(c)) return false;
        }
    }
    return stack.isEmpty();
}
```

### Basic Calculator (Infix Expression Evaluation)

The general idea: use a stack to hold pending numbers/results, and resolve operations as precedence rules dictate (e.g. handle `*`/`/` immediately, defer `+`/`-` onto the stack).

```java
int calculate(String s) {
    Deque<Integer> stack = new ArrayDeque<>();
    int num = 0;
    char sign = '+';
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (Character.isDigit(c)) num = num * 10 + (c - '0');
        if ((!Character.isDigit(c) && c != ' ') || i == s.length() - 1) {
            switch (sign) {
                case '+': stack.push(num); break;
                case '-': stack.push(-num); break;
                case '*': stack.push(stack.pop() * num); break;
                case '/': stack.push(stack.pop() / num); break;
            }
            sign = c;
            num = 0;
        }
    }
    int result = 0;
    for (int val : stack) result += val;
    return result;
}
```

### Postfix (Reverse Polish Notation) Evaluation

```java
int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String token : tokens) {
        if ("+-*/".contains(token) && token.length() == 1) {
            int b = stack.pop(), a = stack.pop();
            switch (token) {
                case "+": stack.push(a + b); break;
                case "-": stack.push(a - b); break;
                case "*": stack.push(a * b); break;
                case "/": stack.push(a / b); break;
            }
        } else {
            stack.push(Integer.parseInt(token));
        }
    }
    return stack.pop();
}
```

---

## 3. Queue-Based Traversal — BFS (Breadth-First Search)

A Queue's FIFO property is exactly what's needed to explore a graph/tree **level by level** — process everything at the current distance before moving further out.

```java
void bfs(int start, Map<Integer, List<Integer>> graph) {
    Queue<Integer> queue = new LinkedList<>();
    Set<Integer> visited = new HashSet<>();
    queue.offer(start);
    visited.add(start);

    while (!queue.isEmpty()) {
        int node = queue.poll();
        System.out.println(node);
        for (int neighbor : graph.getOrDefault(node, List.of())) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                queue.offer(neighbor);
            }
        }
    }
}
```

**Level-order traversal**, tracking level boundaries explicitly (very common in tree problems):

```java
void levelOrder(TreeNode root) {
    if (root == null) return;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int levelSize = queue.size();   // snapshot before processing this level
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            System.out.print(node.val + " ");
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        System.out.println();   // end of a level
    }
}
```

> **Interview point ⭐:** BFS naturally finds the **shortest path** in an unweighted graph, because it explores nodes in strictly increasing order of distance from the source — this is why "shortest path", "minimum number of steps", and "level order" all point to BFS.

---

## 4. Monotonic Deque — Sliding Window Maximum

Combines the monotonic stack idea with a deque to track the max/min within a **sliding window** in O(n) total, instead of O(n·k) with a naive re-scan per window.

```java
int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> deque = new ArrayDeque<>();   // stores indices, values decreasing
    int[] result = new int[nums.length - k + 1];

    for (int i = 0; i < nums.length; i++) {
        if (!deque.isEmpty() && deque.peekFirst() <= i - k) {
            deque.pollFirst();                    // remove indices outside the window
        }
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();                      // maintain decreasing order
        }
        deque.offerLast(i);
        if (i >= k - 1) result[i - k + 1] = nums[deque.peekFirst()];
    }
    return result;
}
```
→ **O(n)** — each index is added and removed from the deque at most once.

---

## Quick Revision

- **Monotonic Stack** → O(n) technique for next/previous greater/smaller element problems.
- **Stack for expressions** → parenthesis matching, calculator evaluation, postfix/RPN evaluation.
- **Queue for BFS** → level-by-level graph/tree traversal, finds shortest path in unweighted graphs.
- **Monotonic Deque** → sliding window max/min in O(n) total.
- Rule of thumb: Stack ↔ "nested/matching/undo" structure; Queue ↔ "process in arrival/distance order".

---

## Interview Definition ⭐

> **Stacks and Queues extend beyond basic push/pop mechanics into two major problem-solving families: Monotonic Stacks solve next/previous greater-or-smaller-element problems in O(n) by maintaining sorted order through selective popping, while Queues power BFS traversal, which explores level by level and therefore guarantees the shortest path in unweighted graphs — the same FIFO discipline, applied to a deque, also yields O(n) sliding window maximum/minimum.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) | Medium | The classic Monotonic Stack problem |
| 2 | [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/) | Easy | Direct application of the monotonic stack template |
| 3 | [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) | Hard | Advanced monotonic stack — very common "hard" interview pick |
| 4 | [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) | Medium | Postfix expression evaluation with a stack |
| 5 | [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) | Medium | Infix expression evaluation with operator precedence |
| 6 | [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | Medium | The standard BFS-with-queue template |
| 7 | [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) | Medium | Multi-source BFS on a grid |
| 8 | [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | Hard | Monotonic deque application |
