# Basics of Matrix

> **📅 Written:** 24 Sep 2026

### Definition

**A Matrix is a 2D array — a grid of elements arranged in rows and columns, where each element is accessed using two indices: row and column.**

**In one sentence:**
> A matrix is just an array of arrays — think of it as a grid of boxes, and you need two numbers (row, column) instead of one to pinpoint any box.

**Example:**

```
        col0 col1 col2
row0  [  1,   2,   3  ]
row1  [  4,   5,   6  ]
row2  [  7,   8,   9  ]
```

---

## Declaration & Memory Model

```java
int[][] matrix = new int[3][3];                 // 3x3, all zeros
int[][] matrix2 = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};

int rows = matrix.length;        // number of rows
int cols = matrix[0].length;     // number of columns (assuming rectangular)
```

In Java, a 2D array is really an **array of arrays** — `matrix` is an array where each element `matrix[i]` is itself a full array (a row).

```
matrix ---> [ row0 ] ---> [1, 2, 3]
            [ row1 ] ---> [4, 5, 6]
            [ row2 ] ---> [7, 8, 9]
```

This also means Java allows **jagged arrays** (rows of different lengths):

```java
int[][] jagged = new int[3][];
jagged[0] = new int[]{1};
jagged[1] = new int[]{1, 2};
jagged[2] = new int[]{1, 2, 3};
```

---

## Traversal

### Row-major traversal (most common)

```java
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        System.out.print(matrix[i][j] + " ");
    }
}
```

→ **O(rows × cols)** — every cell visited once.

### Diagonal Traversal

```
Main diagonal (i == j):        [1, 5, 9]
Anti-diagonal (i + j == n-1):  [3, 5, 7]
```

```java
// Main diagonal
for (int i = 0; i < rows; i++) {
    System.out.print(matrix[i][i] + " ");
}
```

---

## Common Matrix Operations

### 1. Transpose

Flips rows and columns — `transpose[j][i] = matrix[i][j]`.

```
Before:        After (transpose):
1 2 3           1 4 7
4 5 6    -->    2 5 8
7 8 9           3 6 9
```

```java
int[][] transpose = new int[cols][rows];
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        transpose[j][i] = matrix[i][j];
    }
}
```
→ **O(rows × cols)**

### 2. Rotate 90° Clockwise (In-Place, Square Matrix)

Two-step trick: **transpose**, then **reverse each row**.

```
Original:      Transpose:      Reverse each row:
1 2 3           1 4 7           7 4 1
4 5 6    -->    2 5 8    -->    8 5 2
7 8 9           3 6 9           9 6 3
```

```java
// Step 1: Transpose in-place
for (int i = 0; i < n; i++) {
    for (int j = i + 1; j < n; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
    }
}
// Step 2: Reverse each row
for (int[] row : matrix) {
    int left = 0, right = row.length - 1;
    while (left < right) {
        int temp = row[left];
        row[left] = row[right];
        row[right] = temp;
        left++; right--;
    }
}
```
→ **O(n²)** time, **O(1)** extra space — a very common interview trick worth memorizing.

### 3. Spiral Traversal

Peels the matrix layer by layer, like unrolling a spiral, using 4 shrinking boundaries.

```
1 2 3
4 5 6   -->  Spiral order: 1, 2, 3, 6, 9, 8, 7, 4, 5
7 8 9
```

```java
int top = 0, bottom = rows - 1, left = 0, right = cols - 1;
List<Integer> result = new ArrayList<>();

while (top <= bottom && left <= right) {
    for (int j = left; j <= right; j++) result.add(matrix[top][j]);
    top++;
    for (int i = top; i <= bottom; i++) result.add(matrix[i][right]);
    right--;
    if (top <= bottom) {
        for (int j = right; j >= left; j--) result.add(matrix[bottom][j]);
        bottom--;
    }
    if (left <= right) {
        for (int i = bottom; i >= top; i--) result.add(matrix[i][left]);
        left++;
    }
}
```
→ **O(rows × cols)**

---

## Matrix as a Graph (Grid Traversal)

Matrices frequently double as **implicit graphs**, where each cell is a node connected to its (usually 4) neighbors — this underlies flood fill, island-counting, and shortest-path-on-grid problems.

```java
int[] dx = {-1, 1, 0, 0};
int[] dy = {0, 0, -1, 1};

for (int d = 0; d < 4; d++) {
    int ni = i + dx[d], nj = j + dy[d];
    if (ni >= 0 && ni < rows && nj >= 0 && nj < cols) {
        // valid neighbor (ni, nj)
    }
}
```

> **Interview point ⭐:** Whenever a matrix problem mentions "connected region", "island", or "shortest path", think **BFS/DFS on the grid**, treating cells as graph nodes.

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Access `matrix[i][j]` | O(1) |
| Full traversal | O(rows × cols) |
| Transpose | O(rows × cols) |
| Rotate 90° (in-place, square) | O(n²), O(1) space |
| Spiral traversal | O(rows × cols) |
| Grid BFS/DFS | O(rows × cols) |

---

## Quick Revision

- **Matrix** → array of arrays, accessed via `matrix[row][col]`.
- Java 2D arrays can be **jagged** (rows of unequal length).
- **Transpose** swaps `matrix[i][j]` with `matrix[j][i]`.
- **Rotate 90°** = transpose + reverse each row, O(1) extra space.
- **Spiral traversal** peels the matrix using 4 shrinking boundaries.
- Matrices often represent **implicit graphs** — grid BFS/DFS is a huge recurring pattern.

---

## Interview Definition ⭐

> **A matrix is a 2D array — in Java, an array of arrays — accessed via row and column indices, where common operations like transpose and rotation are typically solved in-place using index-swapping tricks, and grid-based problems are frequently modeled as graph traversal over the cells.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Transpose Matrix](https://leetcode.com/problems/transpose-matrix/) | Easy | The basic transpose operation |
| 2 | [Rotate Image](https://leetcode.com/problems/rotate-image/) | Medium | The classic transpose + reverse in-place rotation |
| 3 | [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) | Medium | Boundary-shrinking traversal pattern |
| 4 | [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) | Medium | In-place marking using the matrix's own first row/column |
| 5 | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) | Medium | Binary search treating the matrix as a flattened sorted array |
| 6 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | Medium | Grid as a graph — DFS/BFS on connected regions |
| 7 | [Word Search](https://leetcode.com/problems/word-search/) | Medium | Backtracking over a grid |
