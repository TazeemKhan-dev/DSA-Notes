<!-- #region 80-Count Maze Path -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q80: Count Maze Path</h1>

## 1. Problem Understanding

- You have a grid of size N × M.
- You start at the top-left cell (1,1) and want to reach bottom-right cell (N,M).
- Allowed moves:
  * Horizontal → move 1 step to the right (h)
  * Vertical → move 1 step down (v)
- Task: count the total number of paths (not print them).
---

## 2. Constraints

- 1 ≤ N ≤ 10
- 1 ≤ M ≤ 10
---

## 3. Edge Cases

- Single row (N = 1) → only horizontal moves possible → 1 path.
- Single column (M = 1) → only vertical moves possible → 1 path.
- Grid size 1 × 1 → already at destination → 1 path.
---

## 4. Examples

```text
Input:
2
2
Output:
2
Explanation:
Two possible paths: h→v and v→h.
```

---

## 5. Approaches

### Approach 1: Recursive Count

**Idea:**
- Each cell (i,j) can move:
- Horizontally to (i,j+1)
- Vertically to (i+1,j)
- Count paths recursively from (i,j) to destination (N,M).
- Base case: reached destination → return 1.

**Java Code:**
```java
static int countMazePath(int sr, int sc, int dr, int dc) {
    // Base case: reached destination
    if (sr == dr && sc == dc) return 1;

    int count = 0;

    // Move horizontally if within bounds
    if (sc + 1 <= dc) count += countMazePath(sr, sc + 1, dr, dc);

    // Move vertically if within bounds
    if (sr + 1 <= dr) count += countMazePath(sr + 1, sc, dr, dc);

    return count;
}
System.out.println(countMazePath(1, 1, N, M));
Initial call: (1,1) → destination (2,2)
               (1,1)
             /      \
        h→(1,2)      v→(2,1)
         |              |
      h not possible    v not possible
         |              |
       (2,2)           (2,2)
Count: 2 paths
```

**Complexity (Time & Space):**
- Time Complexity: O(2^(N+M)) → each step can branch into horizontal or vertical move.
- Space Complexity: O(N+M) → recursion stack depth.

---

## 6. Justification / Proof of Optimality

- Each cell is a decision point: move right or down.
- Base case ensures counting only valid paths.
- Recursive addition of counts gives total number of paths.
- Works correctly for edge cases (1×N, N×1 grids).
---

## 7. Variants / Follow-Ups

- Print all paths (instead of counting).
- Allow diagonal moves (h,v,d) → see Maze Paths with Jumps problem.
- Memoization/DP → optimize to O(N×M) time complexity.
- Return paths as list → for further processing instead of just count.
---

## 8. Tips & Observations

- Decision at each cell: You can either move right or down.
- Base case: When you reach the bottom-right cell (N,M), count as 1.
- Prune early: Do not move out of bounds (beyond row N or column M).
- Observation:
  * Paths in an N×M grid = combination problem → C(N+M-2, N-1) (can be verified via recursion).
  * Recursion explores all paths systematically.
- Edge cases:
  * Single row or single column → only 1 path.
  * Grid size 1×1 → only 1 path.
- Optimization hint:
  * Recursive solution can be converted to DP using a 2D table to store intermediate counts for each cell.
- Analogy: Think of recursion like building a tree of decisions at each cell: move right or down, then sum counts from each branch.
---

<!-- #endregion -->
