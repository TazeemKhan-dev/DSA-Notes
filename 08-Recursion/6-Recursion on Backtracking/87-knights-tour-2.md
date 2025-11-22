<!-- #region 87-Knight’s Tour 2 -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q87: Knight’s Tour 2</h1>

## 1. Problem Understanding

- We are given a chessboard of size n x n and a knight starting at position (row, col).
- The goal is to move the knight so that it visits every square exactly once, following the standard L-shaped knight moves (2 in one direction + 1 in perpendicular direction).
- We must print all possible configurations where the knight covers every cell exactly once.
---

## 2. Constraints

- 1≤n≤5
- 0≤row,col<n
---

## 3. Edge Cases

- n = 1 → only one configuration [1].
- Knight cannot complete tour for small boards like n < 5 except special cases.
- Ensure boundary and visited checks to avoid infinite recursion.
---

## 4. Examples

```text
Input:

5
2 0


Output:
(prints all valid knight tours covering all 25 cells once)
```

---

## 5. Approaches

### Approach 1: Brute Force (Pure Backtracking)

**Idea:**
- Try all 8 possible moves from each cell recursively until all cells are visited.
- If stuck, backtrack and try another path.

**Steps:**
- Create a board[n][n] initialized with -1.
- Place the knight at the start cell (x, y) = (0, 0) → step = 0.
- Recursively move to the next valid unvisited cell.
- If all cells are filled (step == n*n), print or count the tour.
- Backtrack if no move is possible.

**Java Code:**
```java
public class KnightTour {
    static int[] dx = {2, 1, -1, -2, -2, -1, 1, 2};
    static int[] dy = {1, 2, 2, 1, -1, -2, -2, -1};

    public static void knightTour(int n) {
        int[][] board = new int[n][n];
        for (int[] row : board) Arrays.fill(row, -1);

        board[0][0] = 0; // start from (0,0)
        if (solve(board, 0, 0, 1, n))
            printBoard(board, n);
        else
            System.out.println("No tour exists.");
    }

    private static boolean solve(int[][] board, int x, int y, int step, int n) {
        if (step == n * n) return true;

        for (int i = 0; i < 8; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if (isValid(board, nx, ny, n)) {
                board[nx][ny] = step;
                if (solve(board, nx, ny, step + 1, n))
                    return true;
                board[nx][ny] = -1; // backtrack
            }
        }
        return false;
    }

    private static boolean isValid(int[][] board, int x, int y, int n) {
        return (x >= 0 && y >= 0 && x < n && y < n && board[x][y] == -1);
    }

    private static void printBoard(int[][] board, int n) {
        for (int[] row : board) {
            for (int cell : row)
                System.out.printf("%2d ", cell);
            System.out.println();
        }
    }
}

Recursion Tree (Example for 4×4 starting at (0, 0))
(0,0)
├── (2,1)
│   ├── (3,3)
│   │   ├── ...
│   │   └── Backtrack
│   ├── (1,3)
│   └── ...
├── (1,2)
│   ├── (3,3)
│   └── ...
└── ...


Each branch represents one possible knight move.
If a move leads to a dead end (no unvisited cell), recursion backtracks.


for accio just integrated in the boiler plate


import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws Exception     {
        Scanner scn = new Scanner(System.in);
        int n = scn.nextInt();
        int r = scn.nextInt();
        int c = scn.nextInt();
        int[][] chess = new int[n][n];
        printKnightsTour(chess, r, c, 1);
    }

  public static void printKnightsTour(int[][] chess, int r, int c, int move) {
    int n = chess.length;

    // Base case: all cells visited
    if (move == n * n) {
        chess[r][c] = move;
        displayBoard(chess);
        chess[r][c] = 0;  // backtrack
        return;
    }

    // Mark current cell with move number
    chess[r][c] = move;

    // Knight's 8 possible moves (clockwise starting from top-right)
    int[] dr = {-2, -1, 1, 2, 2, 1, -1, -2};
    int[] dc = {1, 2, 2, 1, -1, -2, -2, -1};

    for (int i = 0; i < 8; i++) {
        int nr = r + dr[i]; //next row
        int nc = c + dc[i]; //next column

        if (nr >= 0 && nc >= 0 && nr < n && nc < n && chess[nr][nc] == 0) {
            printKnightsTour(chess, nr, nc, move + 1);
        }
    }

    // Backtrack
    chess[r][c] = 0;
}

    public static void displayBoard(int[][] chess){
        for(int i = 0; i < chess.length; i++){
            for(int j = 0; j < chess[0].length; j++){
                System.out.print(chess[i][j] + " ");
            }
            System.out.println();
        }

        System.out.println();
    }
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Worst-case: O(8^(n²))
    * Each cell can have up to 8 recursive calls.
    * The recursion depth = number of cells = n².
  * Very slow for large n (exponential).
- 💾 Space Complexity
  * O(n²) → due to board matrix and recursion stack.

### Approach 2: Warnsdorff’s Heuristic (Optimized, not required here)

**Idea:**
- Instead of trying all 8 moves randomly, choose the move that has the least number of onward valid moves (i.e., moves leading to fewer further possibilities).
- This reduces backtracking dramatically.
- To reduce backtracking, always move the knight to the square with the fewest onward moves.
This ensures higher chance of successful tours early.

**Steps:**
- From the current cell, check all valid knight moves.
- For each move, count how many onward valid moves exist.
- Always choose the cell with the minimum onward count first.
- Continue until board is full or stuck.

**Java Code:**
```java
public class KnightTourHeuristic {
    static int[] dx = {2, 1, -1, -2, -2, -1, 1, 2};
    static int[] dy = {1, 2, 2, 1, -1, -2, -2, -1};

    public static void knightTourWarnsdorff(int n) {
        int[][] board = new int[n][n];
        for (int[] row : board) Arrays.fill(row, -1);
        int x = 0, y = 0;
        board[x][y] = 0;

        for (int step = 1; step < n * n; step++) {
            int nextMove = -1, minDegree = 9, nx = -1, ny = -1;

            for (int i = 0; i < 8; i++) {
                int xx = x + dx[i], yy = y + dy[i];
                if (isValid(board, xx, yy, n)) {
                    int degree = countOnwardMoves(board, xx, yy, n);
                    if (degree < minDegree) {
                        minDegree = degree;
                        nx = xx; ny = yy;
                        nextMove = i;
                    }
                }
            }

            if (nextMove == -1) break; // stuck
            x = nx; y = ny;
            board[x][y] = step;
        }

        printBoard(board, n);
    }

    private static int countOnwardMoves(int[][] board, int x, int y, int n) {
        int count = 0;
        for (int i = 0; i < 8; i++) {
            int xx = x + dx[i], yy = y + dy[i];
            if (isValid(board, xx, yy, n)) count++;
        }
        return count;
    }

    private static boolean isValid(int[][] board, int x, int y, int n) {
        return (x >= 0 && y >= 0 && x < n && y < n && board[x][y] == -1);
    }

    private static void printBoard(int[][] board, int n) {
        for (int[] row : board) {
            for (int cell : row)
                System.out.printf("%2d ", cell);
            System.out.println();
        }
    }
}


🌿 Visualization (Warnsdorff for 5×5)
scss
Copy code
(0,0)
↓ (2,1)
↓ (4,2)
↓ (3,4)
↓ (1,3)
↓ (0,1)
↓ (2,0)
↓ ...
→ covers all 25 squares sequentially
The knight never backtracks, because the heuristic prevents getting trapped early.
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Average case: O(n²) (linear traversal using heuristic)
  * Worst case: O(n² × 8) ≈ O(n²) (still practical)
    * Each move checks 8 directions and counts onward moves.
- 💾 Space Complexity
  * O(n²) for the board matrix.

---

## 6. Justification / Proof of Optimality

- Backtracking: Works for small boards but exponential.
- Warnsdorff’s heuristic: Extremely efficient — works for almost all n ≥ 5 directly.
---

## 7. Variants / Follow-Ups

- Closed Knight’s Tour: Last move connects to the first cell (forms a loop).
- Count All Tours: Count total unique paths (needs backtracking).
- Open Knight’s Tour: Regular version (does not loop back).
---

## 8. Tips & Observations

- Use Warnsdorff’s rule whenever only one path is needed.
- Use backtracking when you must count or print all tours.
- Pruning + move ordering drastically improves performance.
- Knight’s Tour → excellent example of heuristic-guided recursion.
---

<!-- #endregion -->
