<!-- #region 0-Recursion on 2D Arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Recursion on 2D Arrays</h1>

## 1. Problem Understanding

- Recursion on 2D arrays (matrix) involves processing elements row by row or column by column.
- Useful for:
  * Traversal / Printing
  * Sum / Count
  * Searching elements
  * Pathfinding (like maze, rat in a grid)
  * Backtracking problems (all paths, subsets)
- Typically uses two indices: i for row, j for column.
---

## 2. Constraints

- Array size = m x n
- Indices should stay within bounds (0 ≤ i < m, 0 ≤ j < n)
- Base case depends on:
  * End of row/column
  * Reaching last cell
- Stack depth = O(m * n) in full traversal
---

## 3. Edge Cases

- Empty matrix (m = 0 or n = 0)
- Single row or single column
- Diagonal-only traversal (for specialized problems)
- Obstacles (for pathfinding/backtracking)
---

## 4. Examples

```text
Input: [[1,2],[3,4]] → Output (traverse): 1 2 3 4

Input: [[1,2],[3,4]] → Sum = 10

Input: Maze with 0/1 → Print all paths from top-left to bottom-right
```

---

## 5. Approaches

### Approach 1: Full Traversal (Row-wise)

**Idea:**
- Traverse row by row, column by column.

**Java Code:**
```java
void traverse(int[][] mat, int i, int j) {
    int m = mat.length, n = mat[0].length;
    if (i == m) return;
    if (j == n) {
        traverse(mat, i + 1, 0);
        return;
    }
    System.out.print(mat[i][j] + " ");
    traverse(mat, i, j + 1);
}
```

**Complexity (Time & Space):**
- Time: O(m * n)
- Space: O(m * n)

### Approach 2: Sum of Elements

**Idea:**
- Add current element + recursion on rest of matrix.

**Java Code:**
```java
int sumMatrix(int[][] mat, int i, int j) {
    int m = mat.length, n = mat[0].length;
    if (i == m) return 0;
    if (j == n) return sumMatrix(mat, i + 1, 0);
    return mat[i][j] + sumMatrix(mat, i, j + 1);
}
```

**Complexity (Time & Space):**
- Time: O(m * n)
- Space: O(m * n)

### Approach 3: Search Element

**Idea:**
- Return true if element is found in recursion.

**Java Code:**
```java
boolean searchMatrix(int[][] mat, int i, int j, int key) {
    int m = mat.length, n = mat[0].length;
    if (i == m) return false;
    if (j == n) return searchMatrix(mat, i + 1, 0, key);
    if (mat[i][j] == key) return true;
    return searchMatrix(mat, i, j + 1, key);
}
```

**Complexity (Time & Space):**
- Time: O(m * n)
- Space: O(m * n)

### Approach 4: Row-wise Maximum

**Idea:**
- Find max in current row recursively, compare with rest.

**Java Code:**
```java
int rowMax(int[][] mat, int i, int j) {
    int n = mat[0].length;
    if (i == mat.length) return Integer.MIN_VALUE;
    if (j == n) return rowMax(mat, i + 1, 0);
    int maxInRest = rowMax(mat, i, j + 1);
    return Math.max(mat[i][j], maxInRest);
}
```

**Complexity (Time & Space):**
- Time: O(m * n)
- Space: O(m * n)

### Approach 5: Column-wise / Diagonal Traversal

**Idea:**
- Similar to row-wise but adjust indices.
- Column-wise: Swap i and j in recursion logic
- Diagonal traversal: move i+1, j+1 per step
- Time: O(m * n)
- Space: O(m * n)

### Approach 6: Pathfinding (Maze / Rat in Grid)

**Idea:**
- Explore all 4 directions using backtracking.

**Java Code:**
```java
void findPaths(int[][] maze, int i, int j, String path) {
    int m = maze.length, n = maze[0].length;
    if (i < 0 || j < 0 || i >= m || j >= n || maze[i][j] == 1) return;
    if (i == m - 1 && j == n - 1) {
        System.out.println(path);
        return;
    }
    maze[i][j] = 1; // mark visited
    findPaths(maze, i + 1, j, path + "D");
    findPaths(maze, i, j + 1, path + "R");
    findPaths(maze, i - 1, j, path + "U");
    findPaths(maze, i, j - 1, path + "L");
    maze[i][j] = 0; // unmark
}
```

**Complexity (Time & Space):**
- Time: O(4^(m * n)) worst-case
- Space: O(m * n) recursion stack

### Approach 7: Print All Paths from Top-left to Bottom-right

**Idea:**
- Only move right or down.

**Java Code:**
```java
void printAllPaths(int[][] mat, int i, int j, String path) {
    int m = mat.length, n = mat[0].length;
    if (i >= m || j >= n) return;
    if (i == m - 1 && j == n - 1) {
        System.out.println(path + mat[i][j]);
        return;
    }
    printAllPaths(mat, i, j + 1, path + mat[i][j] + " ");
    printAllPaths(mat, i + 1, j, path + mat[i][j] + " ");
}
```

**Complexity (Time & Space):**
- Time: O(2^(m+n))
- Space: O(m+n)

### Approach 8: Count Paths (Top-left to Bottom-right)

**Idea:**
- Count number of unique paths using recursion.Count number of unique paths using recursion.

**Java Code:**
```java
int countPaths(int i, int j, int m, int n) {
    if (i == m - 1 && j == n - 1) return 1;
    if (i >= m || j >= n) return 0;
    return countPaths(i + 1, j, m, n) + countPaths(i, j + 1, m, n);
}
```

**Complexity (Time & Space):**
- Time: O(2^(m+n))
- Space: O(m+n)

### Approach 9: Flood Fill / DFS on Grid

**Idea:**
- Mark visited cells and recursively fill adjacent cells.

**Java Code:**
```java
void floodFill(int[][] grid, int i, int j, int oldColor, int newColor) {
    int m = grid.length, n = grid[0].length;
    if (i < 0 || j < 0 || i >= m || j >= n || grid[i][j] != oldColor) return;
    grid[i][j] = newColor;
    floodFill(grid, i + 1, j, oldColor, newColor);
    floodFill(grid, i - 1, j, oldColor, newColor);
    floodFill(grid, i, j + 1, oldColor, newColor);
    floodFill(grid, i, j - 1, oldColor, newColor);
}
```

**Complexity (Time & Space):**
- Time: O(m*n)
- Space: O(m*n)

### Approach 10: Spiral / Zig-Zag Traversal (Advanced)

**Idea:**
- Maintain boundaries (top, bottom, left, right) and recurse layer by layer.
- Time: O(m * n)
- Space: O(m*n) recursion stack

---

## 6. Justification / Proof of Optimality

- Recursion simplifies handling of matrix traversal.
- Essential for backtracking problems in grids.
- Can be extended to DP (memoization) for optimization.
---

## 7. Variants / Follow-Ups

- Recursion on 3D arrays
- Recursion with constraints (obstacles, walls)
- Combined with subsequence / subset generation per row
---

## 8. Tips & Observations

- Base case is often reaching last row/column.
- Use visited matrix for cycles in grid problems.
- Visualize recursion as tree with branching directions.
- Backtracking requires state restoration after recursion.
- Optimize by reducing string concatenations in path storage.
---

<!-- #endregion -->
