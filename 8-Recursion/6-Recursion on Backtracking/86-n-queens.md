<!-- #region 86- N Queens -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q86: N Queens</h1>

## 1. Problem Understanding

- We need to place n queens on an n × n chessboard such that no two queens attack each other, meaning:
- No two queens share the same row.
- No two queens share the same column.
- No two queens share the same diagonal.
- We must count the number of possible valid configurations (not print them).
---

## 2. Constraints

- 1 ≤ n ≤ 10
- Output: number of valid configurations
---

## 3. Edge Cases

- n = 1 → only one configuration.
- n = 2 or n = 3 → no valid solutions.
- Larger n (like 4 or 5) → multiple valid configurations.
---

## 4. Examples

```text
Example 2
Input:
4
Output:
2
Explanation:
There are exactly two valid ways to place 4 queens safely.
```

---

## 5. Approaches

### Approach 1: Backtracking (Recursive with Board Matrix)

**Idea:**
- We try to place queens row by row.
- At each row, we explore all columns and check if placing a queen is safe.
- If safe → place it and recursively place queens in the next row.
- If not → backtrack and try another column.

**Steps:**
- Use an n×n matrix to represent the board.
- Start from row 0.
- For each column:
  * Check if it’s safe to place a queen.
  * If safe, place it and move to the next row recursively.
  * Backtrack by removing the queen.
- When row == n, increment the count (found a valid configuration).

**Java Code:**
```java
public int totalNQueens(int n) {
    int[][] board = new int[n][n];
    return solve(board, 0, n);
}

private int solve(int[][] board, int row, int n) {
    if (row == n) return 1; // valid configuration found
    int count = 0;
    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col, n)) {
            board[row][col] = 1;        // place queen
            count += solve(board, row + 1, n); // move to next row
            board[row][col] = 0;        // backtrack
        }
    }
    return count;
}

private boolean isSafe(int[][] board, int row, int col, int n) {
    // check column
    for (int i = 0; i < row; i++)
        if (board[i][col] == 1) return false;

    // check left diagonal
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
        if (board[i][j] == 1) return false;

    // check right diagonal
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++)
        if (board[i][j] == 1) return false;

    return true;
}


Recursion Tree (for n = 4)

We try to place queens row by row:

Row 0 → Try col 0
│
├── Not safe down the line → backtrack
│
Row 0 → Try col 1
│
├── Place Q at (0,1)
│   ├── Row 1 → Try col 3 (safe)
│   │   ├── Row 2 → Try col 0 (safe)
│   │   │   ├── Row 3 → Try col 2 (safe) ✅ Solution 1
│   │   │   └── Backtrack
│   │   └── Backtrack
│   └── Backtrack
│
Row 0 → Try col 2
│
├── Place Q at (0,2)
│   ├── Row 1 → Try col 0 (safe)
│   │   ├── Row 2 → Try col 3 (safe)
│   │   │   ├── Row 3 → Try col 1 (safe) ✅ Solution 2
│   │   │   └── Backtrack
│   │   └── Backtrack
│   └── Backtrack
│
Row 0 → Try col 3 → no valid solution


✅ Total Solutions = 2
```

**Complexity (Time & Space):**
- Time: O(N!) — exploring all column placements per row.
- Space: O(N^2) — recursion + board storage.

### Approach 2: Optimized Backtracking (Using Arrays)

**Idea:**
- Instead of checking the board each time, keep arrays to mark attacks on columns and diagonals:
  * cols[col] → if a queen is in that column.
  * diag1[row + col] → left diagonal.
  * diag2[row - col + n - 1] → right diagonal.
- This avoids scanning the whole board on each check.

**Java Code:**
```java
public int totalNQueens(int n) {
    boolean[] cols = new boolean[n];
    boolean[] diag1 = new boolean[2 * n];
    boolean[] diag2 = new boolean[2 * n];
    return backtrack(0, n, cols, diag1, diag2);
}

private int backtrack(int row, int n, boolean[] cols, boolean[] diag1, boolean[] diag2) {
    if (row == n) return 1;
    int count = 0;
    for (int col = 0; col < n; col++) {
        int d1 = row + col;
        int d2 = row - col + n - 1;
        if (cols[col] || diag1[d1] || diag2[d2]) continue;

        cols[col] = diag1[d1] = diag2[d2] = true;
        count += backtrack(row + 1, n, cols, diag1, diag2);
        cols[col] = diag1[d1] = diag2[d2] = false; // backtrack
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N!) (same order but faster in practice)
- Space: O(N)

### Approach 3: Bitmask Optimization (Most Efficient for N ≤ 15)

**Idea:**
- Use bitmasks instead of boolean arrays to store attacked columns and diagonals.
- Each bit position represents whether a column/diagonal is occupied.

**Java Code:**
```java
int solve(int n) {
    return place(0, 0, 0, 0, n);
}

int place(int row, int cols, int d1, int d2, int n) {
    if (row == n) return 1;
    int count = 0;
    int available = ((1 << n) - 1) & (~(cols | d1 | d2));
    while (available != 0) {
        int pos = available & (-available); // pick rightmost available bit
        available -= pos;
        count += place(row + 1,
                       cols | pos,
                       (d1 | pos) << 1,
                       (d2 | pos) >> 1,
                       n);
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N!) but extremely fast in practice due to bit-level pruning.
- Space: O(N)

---

## 6. Justification / Proof of Optimality

- Backtracking guarantees exploration of all valid board configurations, and pruning ensures we only go deeper on safe placements.
- Bitmask and array optimizations make the checking O(1).
---

## 7. Variants / Follow-Ups

- Print all N-Queens configurations instead of counting them.
- Return board states as List<List<String>>.
- M-Queens variant → different attack patterns.
---

## 8. Tips & Observations

- n = 2 or 3 → always 0 solutions.
- Use arrays or bitmasks to speed up safe checks.
- Backtracking is ideal because of branch pruning.
- Bitmask method is best for competitive programming.
---

<!-- #endregion -->
