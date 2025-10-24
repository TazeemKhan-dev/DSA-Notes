<!-- #region 76-Print All Maze Paths -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q76: Print All Maze Paths</h1>

## 1. Problem Understanding

- You are at the top-left cell (1,1) of an N×M grid and must reach the bottom-right cell (N,M).
- At each step, you can only move:
  * 'h' → horizontally to the right (→)
  * 'v' → vertically downward (↓)
- You must print all possible paths to reach the destination.
---

## 2. Constraints

- 1 ≤ N, M ≤ 10
- Moves allowed: only h (right) and v (down)
- Recursive solution expected
---

## 3. Edge Cases

- If N = 1 → only horizontal moves ("h" * (M-1))
- If M = 1 → only vertical moves ("v" * (N-1))
- If N = 1 and M = 1 → already at destination, path = ""
---

## 4. Examples

```text
Example 1
Input:
2
2
Output:
hv
vh
Explanation:
Move right → down → "hv"
Move down → right → "vh"
```

---

## 5. Approaches

### Approach 1: Recursion

**Idea:**
- At each cell (i, j):
- If not at the destination, make recursive calls:
  * Move horizontally → (i, j + 1)
  * Move vertically → (i + 1, j)
- Append 'h' or 'v' to the path string.

**Steps:**
- Base Case: if (i == n && j == m), print the current path string.
- If j < m, make a horizontal move ('h').
- If i < n, make a vertical move ('v').
- Recursive exploration ensures all possible paths are printed.

**Java Code:**
```java
static void printMazePaths(int i, int j, int n, int m, String psf) {
    // Base case: reached destination
    if (i == n && j == m) {
        System.out.println(psf);
        return;
    }

    // Horizontal move (right)
    if (j < m) {
        printMazePaths(i, j + 1, n, m, psf + "h");
    }

    // Vertical move (down)
    if (i < n) {
        printMazePaths(i + 1, j, n, m, psf + "v");
    }
}

🟢 Initial call:
printMazePaths(1, 1, N, M, "");

ex
printMazePaths(1, 1, 3, 3, "")

(1,1,"")
           /                  \
      h→(1,2,"h")          v→(2,1,"v")
      /        \            /         \
 h→(1,3,"hh") v→(2,2,"hv") h→(2,2,"vh") v→(3,1,"vv")
    |          |             |            |
 v→(2,3,"hhv") v→(2,3,"hvv") h→(2,3,"vhh") v→(3,2,"vvh")
    |          |             |            |
 v→(3,3,"hhvv") v→(3,3,"hv v") h→(3,3,"vhhv") v→(3,3,"vvhv")

Output
hhvv
hvhv
hvvh
vhhv
vhhv
vvhh
```

**Complexity (Time & Space):**
- Time Complexity: O(2^(N+M)) → each move branches into two possibilities
- Space Complexity: O(N + M) → recursion stack depth

---

## 6. Justification / Proof of Optimality

- Every path represents a combination of horizontal and vertical moves.
- For a grid of size (N, M), total paths = C((N-1)+(M-1), (N-1)) (combinatorial count).
- Recursion naturally explores all paths.
---

## 7. Variants / Follow-Ups

- Print paths with diagonal moves (‘d’) — extend recursion with one more call.
- Return list of paths instead of printing.
- Count total paths (return int instead of printing).
- Blocked maze — skip moves through blocked cells.
---

## 8. Tips & Observations

- This is a template for many grid problems (rat in a maze, unique paths, etc.).
- Always make horizontal call before vertical, as required.
- For counting or storing paths, modify the base case to accumulate results.
---

<!-- #endregion -->
