<!-- #region 77-Maze Paths with Jumps -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q77: Maze Paths with Jumps</h1>

## 1. Problem Understanding

- You are in the top-left cell (1, 1) of an n × m grid and must reach the bottom-right cell (n, m).
- In each move, you can jump:
- Horizontally → to the right, by 1 to m - j steps (h1, h2, …).
- Vertically → downward, by 1 to n - i steps (v1, v2, …).
- Diagonally → to the bottom-right, by 1 to min(n - i, m - j) steps (d1, d2, …).
- You must print all possible paths to reach the destination.
---

## 2. Constraints

- 1 ≤ N, M ≤ 5
- Allowed moves: multiple jumps (h, v, d)
- Recursive solution expected (no loops for path generation except for jump range)
---

## 3. Edge Cases

- If already at destination (i == n && j == m), print the path.
- If n = 1 and m = 1, only one possible path — empty string.
- Smallest jump allowed = 1 step.
- Largest jump allowed = remaining steps in that direction.
---

## 4. Examples

```text
Input:
2
2
Output:
h1v1
v1h1
d1
Explanation:
From (1,1) to (2,2):
Move right 1 → down 1 → "h1v1"
Move down 1 → right 1 → "v1h1"
Move diagonally 1 → "d1"

```

---

## 5. Approaches

### Approach 1: Recursive Backtracking

**Idea:**
- At each position (i, j):
- Try all horizontal jumps (from 1 to m - j).
- Try all vertical jumps (from 1 to n - i).
- Try all diagonal jumps (from 1 to min(n - i, m - j)).
- Each recursive call appends the move (h, v, or d + jump size) to the path string.

**Steps:**
- Base Case:
- If (i == n && j == m), print the path string.
- Recursive Exploration:
- For h → loop through all possible jump sizes (1 to m - j)
- For v → loop through (1 to n - i)
- For d → loop through (1 to min(n - i, m - j))
- Recurse to new position after jump.

**Java Code:**
```java
static void printMazePathsWithJumps(int i, int j, int n, int m, String psf) {
    // Base case: reached destination
    if (i == n && j == m) {
        System.out.println(psf);
        return;
    }

    // Horizontal jumps
    for (int jump = 1; j + jump <= m; jump++) {
        printMazePathsWithJumps(i, j + jump, n, m, psf + "h" + jump);
    }

    // Vertical jumps
    for (int jump = 1; i + jump <= n; jump++) {
        printMazePathsWithJumps(i + jump, j, n, m, psf + "v" + jump);
    }

    // Diagonal jumps
    for (int jump = 1; i + jump <= n && j + jump <= m; jump++) {
        printMazePathsWithJumps(i + jump, j + jump, n, m, psf + "d" + jump);
    }
}
Initial call:

printMazePathsWithJumps(1, 1, n, m, "");

                   (1,1,"")
                 /     |     \
          h1→(1,2)   v1→(2,1)  d1→(2,2)
           /             \
   v1→(2,2,"h1v1")   h1→(2,2,"v1h1")

Prints:
  "h1v1"
  "v1h1"
  "d1"

```

**Complexity (Time & Space):**
- Time Complexity:
- O(3^(N+M)) → each cell can generate up to 3 recursive branches (h, v, d).
- (Each branch further multiplies by possible jump counts, but grid size ≤ 5 keeps it feasible.)
- Space Complexity:
- O(N + M) → recursion stack depth.

---

## 6. Justification / Proof of Optimality

- Each recursive call explores all valid jump paths until reaching the destination.
- The use of nested loops ensures every possible jump distance is explored once per direction.
---

## 7. Variants / Follow-Ups

- Return paths instead of printing — collect paths in a list and return.
- Blocked cells — skip moves through blocked cells.
- Count total paths — return an integer instead of printing.
- Allow only specific move sets (e.g., no diagonals) — adjust recursive branches.
---

## 8. Tips & Observations

- Use this as a template for problems involving multiple move lengths.
- “Jump” just means moving multiple steps in one direction, not repeated single moves.
- Keep horizontal call before vertical, and vertical before diagonal for consistent order.
- The recursive structure is almost identical to normal maze paths — just extended with loops for jump lengths.
---

<!-- #endregion -->
