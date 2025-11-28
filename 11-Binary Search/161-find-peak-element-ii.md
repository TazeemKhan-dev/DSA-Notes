<!-- #region 161-Find Peak Element - II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q161: Find Peak Element - II</h1>

## 1. Problem Understanding

- You are given an n x m matrix mat where:
- No two adjacent cells are equal.
- Each cell has up to 4 neighbors: left, right, top, bottom.
- A 2D peak element is one where:
  * mat[i][j] > all adjacent neighbors
  * You must return any peak coordinate [i, j].
  * The matrix is conceptually surrounded by -1 boundaries, so outer edges can also be peaks.
- Goal:
  * Find ANY peak in O(n log m) or O(m log n)
- This is the classic 2D peak finding problem.
---

## 2. Constraints

- 1 ≤ n, m ≤ 500
- 1 ≤ mat[i][j] ≤ 1e5
- Adjacent cells have different values
- Multiple peaks possible
---

## 3. Edge Cases

- Peak at a corner
- Peak in the first or last row
- Peak in the first or last column
- Entire matrix strictly increasing → bottom-right is peak
- Entire matrix strictly decreasing → top-left is peak
---

## 4. Examples

```text
Example 1

mat = [
 [10, 20, 15],
 [21, 30, 14],
 [7, 16, 32]
]
Output: [1, 1]  → 30 is a peak


Example 2

mat = [
 [10, 7],
 [11, 17]
]
Output: [1, 1]
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check every cell, compare with up to 4 neighbors.

**Steps:**
- For each (i, j):
  * Check left, right, top, bottom safely.
  * If all are smaller → return [i, j].

**Java Code:**
```java
public int[] findPeakBrute(int[][] mat){
    int n = mat.length, m = mat[0].length;

    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            int val = mat[i][j];

            boolean left  = j-1 < 0       || mat[i][j-1] < val;
            boolean right = j+1 >= m      || mat[i][j+1] < val;
            boolean up    = i-1 < 0       || mat[i-1][j] < val;
            boolean down  = i+1 >= n      || mat[i+1][j] < val;

            if(left && right && up && down){
                return new int[]{i, j};
            }
        }
    }
    return new int[]{-1, -1};
}
```

**💭 Intuition Behind the Approach:**
- We check all cells — guaranteed to find a peak, but slow.

**Complexity (Time & Space):**
- Time: O(n*m)
- Space: O(1)

### Approach 2: Row-wise Binary Search

**Idea:**
- For each row, do binary search to find element bigger than neighbors.
- But worst case = O(n log m).

**Steps:**
- For each row:
  * Binary search column mid.
  * Compare with neighbors.
- Move search space accordingly.

**💭 Intuition Behind the Approach:**
- : peak in 2D can be found by treating each row like a 1D peak problem.

**Complexity (Time & Space):**
- Time: O(n log m)
- Space: O(1)

### Approach 3: Column-wise Binary Search

**Idea:**
- We apply binary search on columns, not rows.
- For a chosen column mid:
  * Find the row where the column value is maximum.
  * Check if that cell is a peak.
  * If right neighbor is bigger → move right (peak must be on right).
  * Else → move left.
- This works because the matrix adjacency and monotonic constraints create a directional slope.

**Steps:**
- Set:
  * low = 0
  * high = m - 1
- While low ≤ high:
  * mid = (low + high) / 2
  * Find maxRow = row with maximum value in column mid
  * Let val = mat[maxRow][mid]
- Compare neighbors:
  * if left > val → peak is on left half → high = mid - 1
  * else if right > val → peak is on right half → low = mid + 1
  * else → this cell is a peak, return [maxRow, mid]
- This guarantees peak because:
  * If neighbor is larger, moving in that direction must lead to a peak.

**Java Code:**
```java
public class Solution {

    public int[] findPeakGrid(int[][] mat) {
        int n = mat.length;
        int m = mat[0].length;

        int low = 0, high = m - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // find row with max in this column
            int maxRow = 0;
            for (int i = 0; i < n; i++) {
                if (mat[i][mid] > mat[maxRow][mid]) 
                    maxRow = i;
            }

            int left  = (mid - 1 >= 0) ? mat[maxRow][mid - 1] : -1;
            int right = (mid + 1 < m) ? mat[maxRow][mid + 1] : -1;
            int curr  = mat[maxRow][mid];

            // check if peak
            if (curr > left && curr > right) {
                return new int[]{maxRow, mid};
            }
            else if (right > curr) {
                low = mid + 1;  // go right
            }
            else {
                high = mid - 1; // go left
            }
        }

        return new int[]{-1, -1};  // should never reach here
    }
}
```

**💭 Intuition Behind the Approach:**
- Why choose the max element in the column?
  * Because the best chance to find a peak is at a local maximum in that column.
- Why move left/right?
  * If right neighbor is greater, the slope leads upward → peak must exist there.
  * Same logic for left.
- This works like mountain climbing:
  * If the slope goes up, move toward the higher direction until peak is found.

**Complexity (Time & Space):**
- Time: O(n log m)
  * Each binary search iteration → O(n) to find column max
  * log m iterations of binary search
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- A peak must exist by problem guarantee.
- Searching direction based on neighbor comparison always moves toward a higher cell.
- A strictly higher neighbor ensures a peak in that direction (like 1D peak logic).
- The matrix always has at least one strict peak because adjacent cells differ.
---

## 7. Variants / Follow-Ups

- 1D peak finding
- Find peak in 2D matrix using divide and conquer (classic MIT lecture)
- Find local maxima in a grid
- Mountain matrix traversal
---

## 8. Tips & Observations

- Start binary search on columns, not rows → fewer columns than elements.
- Always pick the max element of the mid column to compare.
- No need to check top/bottom because you're already checking max.
- Outer boundary assumed -1 → natural peak on edges possible.

- **⚠️ Pitfalls**
    - Forgetting to check matrix boundaries
    - Picking ANY element instead of column max
    - Checking only one neighbor
    - Using DFS/BFS (not needed)
    - Assuming sorted matrix (it is NOT sorted)
    - Confusing with "Search in 2D matrix" problem — totally different
---

<!-- #endregion -->
