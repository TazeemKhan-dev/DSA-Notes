<!-- #region 231-Rotting Oranges -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q231: Rotting Oranges</h1>

## 1. Problem Statement

- You are given an m x n grid where each cell can have one of three values:
  * 0 → empty cell
  * 1 → fresh orange
  * 2 → rotten orange
- Every minute, any fresh orange that is 4-directionally adjacent (up, down, left, right) to a rotten orange becomes rotten.
- Return the minimum number of minutes required so that no fresh orange remains.
- If it is impossible, return -1.
---

## 2. Problem Understanding

- Rot spreads simultaneously from all rotten oranges
- This is a level-by-level spread over time
- Each level represents 1 minute
- If even one fresh orange is isolated, answer is -1
- 👉 This is a multi-source BFS problem.
---

## 3. Constraints

- 1 <= m, n <= 10
- Grid size is small, but logic must be correct
- Only 4-directional movement allowed
---

## 4. Edge Cases

- No fresh oranges initially → answer 0
- Fresh oranges but no rotten orange → answer -1
- Single cell grid
- Fresh orange completely isolated
---

## 5. Examples

```text
Example 1
2 1 1
1 1 0
0 1 1


Output: 4

Example 2
2 1 1
0 1 1
1 0 1


Output: -1
```

---

## 6. Approaches

### Approach 1: Brute Force Simulation (Not Recommended)

**Idea:**
- Simulate the rotting process minute by minute by scanning the entire grid repeatedly and rotting fresh oranges adjacent to rotten ones.

**Steps:**
- Count total fresh oranges
- Repeat:
  * Traverse entire grid
  * For every rotten orange, mark adjacent fresh oranges as “to be rotten”
- Apply all changes at once (to simulate simultaneous spread)
- Increment minute counter
- Stop when:
  * No fresh oranges remain → return time
  * No change occurs but fresh still exist → return -1

**Java Code:**
```java
static int orangesRottingBrute(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    int fresh = 0;

    for (int[] row : grid)
        for (int cell : row)
            if (cell == 1) fresh++;

    int minutes = 0;
    int[][] dir = {{1,0},{-1,0},{0,1},{0,-1}};

    while (fresh > 0) {
        boolean changed = false;
        List<int[]> toRot = new ArrayList<>();

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    for (int[] d : dir) {
                        int ni = i + d[0], nj = j + d[1];
                        if (ni >= 0 && ni < m && nj >= 0 && nj < n && grid[ni][nj] == 1) {
                            toRot.add(new int[]{ni, nj});
                        }
                    }
                }
            }
        }

        for (int[] cell : toRot) {
            if (grid[cell[0]][cell[1]] == 1) {
                grid[cell[0]][cell[1]] = 2;
                fresh--;
                changed = true;
            }
        }

        if (!changed) return -1;
        minutes++;
    }

    return minutes;
}
```

**💭 Intuition Behind the Approach:**
- We imitate the real-world process directly
- Each loop represents one minute
- Inefficient because we re-scan the whole grid every time

**Complexity (Time & Space):**
- Time: O((m*n)^2) — full grid scan per minute
- Space: O(m*n) — temporary list for changes
- 👉 Avoid in interviews

### Approach 2: Multi-Source BFS (Optimal & Interview-Standard)

**Idea:**
- Treat every rotten orange as a BFS source
- Spread rot level by level
- Each BFS level = 1 minute
- Count fresh oranges to verify final state

**Steps:**
- Add all rotten oranges to queue
- Count fresh oranges
- BFS:
  * Process one level (1 minute)
  * Rot adjacent fresh oranges
- If fresh oranges remain → return -1
- Else return time

**Java Code:**
```java
static int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> q = new ArrayDeque<>();
    int fresh = 0;

    // Step 1: push all rotten oranges
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) {
                q.add(new int[]{i, j});
            } else if (grid[i][j] == 1) {
                fresh++;
            }
        }
    }

    // Edge case
    if (fresh == 0) return 0;

    int minutes = 0;
    int[][] dir = {{1,0},{-1,0},{0,1},{0,-1}};

    // Step 2: BFS
    while (!q.isEmpty()) {
        int size = q.size();
        boolean rotted = false;

        for (int i = 0; i < size; i++) {
            int[] cell = q.poll();
            int r = cell[0], c = cell[1];

            for (int[] d : dir) {
                int nr = r + d[0];
                int nc = c + d[1];

                if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    fresh--;
                    q.add(new int[]{nr, nc});
                    rotted = true;
                }
            }
        }

        if (rotted) minutes++;
    }

    return fresh == 0 ? minutes : -1;
}
```

**💭 Intuition Behind the Approach:**
- Rot spreads simultaneously → BFS
- Multiple rotten oranges → multi-source BFS
- Queue naturally handles time layers
- Stop when no more spread is possible

**Complexity (Time & Space):**
- Time: O(m*n) — each cell processed once
- Space: O(m*n) — queue in worst case

---

## 7. Justification / Proof of Optimality

- BFS models time-based spread perfectly
- Multi-source handles simultaneous rotting
- Guarantees minimum time
- Industry-standard solution
---

## 8. Variants / Follow-Ups

- Rotting Oranges II (different spread rules)
- Zombie infection problems
- Fire spread simulation
- Shortest path in matrix
---

## 9. Tips & Observations

- Always count fresh oranges first
- Increment time per BFS level
- Use a flag to check if any rotting happened
- 4-direction only (no diagonals)
---

## 10. Pitfalls

- Incrementing time when no rotting happens
- Forgetting multi-source initialization
- Missing edge case where no fresh oranges exist
- Treating it as DFS instead of BFS
---

<!-- #endregion -->
