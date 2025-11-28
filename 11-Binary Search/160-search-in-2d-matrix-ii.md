<!-- #region 160-Search in 2D matrix - II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q160: Search in 2D matrix - II</h1>

## 1. Problem Understanding

- You are given a 2D matrix where:
- Every row is sorted left → right
- Every column is sorted top → bottom
- You must determine if a given target exists in this matrix.
---

## 2. Constraints

- 1 ≤ n, m ≤ 300
- -10^9 ≤ matrix[i][j] ≤ 10^9
- Matrix sorted row-wise & column-wise
---

## 3. Edge Cases

- Target smaller than top-left element → false
- Target larger than bottom-right element → false
- Single row or single column
- Target equals first/last element
- Duplicate numbers DO NOT exist (but algorithm works even if they did)
---

## 4. Examples

```text
1   4   7   11  15
2   5   8   12  19
3   6   9   16  22
10 13  14  17  24
18 21  23  26  30
Example 1

target = 5 → true


Example 2

target = 20 → false


Example 3

target = 1 → true
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Simply check every cell.

**Steps:**
- Loop through all rows and columns.

**Java Code:**
```java
public boolean searchBrute(int[][] mat, int target) {
    for(int[] row : mat){
        for(int val : row){
            if(val == target) return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Straightforward but ignores matrix sorted structure.

**Complexity (Time & Space):**
- Time: O(n*m)
- Space: O(1)

### Approach 2: Binary Search on Each Row

**Idea:**
- Since each row is sorted:
  * For each row → do binary search.

**Steps:**
- For each row:
  * If target in range [row[0], row[m-1]], binary search.
- If found, return true.

**Java Code:**
```java
public boolean searchEachRow(int[][] mat, int target) {
    for(int[] row : mat){
        if(target >= row[0] && target <= row[row.length-1]){
            if(Arrays.binarySearch(row, target) >= 0) return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Uses row sorting but ignores column sorting.

**Complexity (Time & Space):**
- Time: O(n * log m)
- Space: O(1)

### Approach 3: Staircase Search (Top-Right Corner Method)

**Idea:**
- Start at top-right corner:
  * If target < current → move left
  * If target > current → move down
  * If equal → return true
- Why this works
  * From (i, j):
  * All values left of (i, j) are smaller
  * All values below (i, j) are larger
- So you eliminate one row OR one column in each step → O(n+m).

**Steps:**
- Set:
  * i = 0
  * j = m - 1
- While in bounds:
  * if mat[i][j] == target → true
  * if target < mat[i][j] → j-- (move left)
  * else → i++ (move down)
- If loop ends → false

**Java Code:**
```java
public boolean searchMatrix(int[][] matrix, int target) {
    int n = matrix.length;
    int m = matrix[0].length;

    int i = 0, j = m - 1;

    while(i < n && j >= 0){
        if(matrix[i][j] == target){
            return true;
        }
        else if(target < matrix[i][j]){
            j--;  // move left
        }
        else{
            i++;  // move down
        }
    }

    return false;
}
```

**💭 Intuition Behind the Approach:**
- You eliminate a FULL row or FULL column depending on comparison.
- This is like searching in a BST:
  * Left side smaller
  * Down side greater
- Traversal path looks like a staircase → hence the name.

**Complexity (Time & Space):**
- Time: O(n + m)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Starting from top-right is optimal because:
  * From here, moving left → values strictly decrease
  * Moving down → values strictly increase
- Sorted rows + sorted columns → monotonic movement
- Guarantees minimal comparisons
---

## 7. Variants / Follow-Ups

- Row with maximum 1s (same top-right staircase technique)
- Search in a sorted 2D matrix where entire matrix sorted as flattened 1D
- Search a target in rotated sorted 2D arrays (hard)
- Kth smallest element in sorted matrix (heap or binary search)
---

## 8. Tips & Observations

- Always start top-right OR bottom-left for matrix sorted both ways.
- Avoid binary search per row if you need faster than O(n log m).
- Staircase search = monotonic elimination.
- The moment you move left or down, you eliminate full row/column instantly.

- **⚠️ Pitfalls**
    - Don’t start from top-left → both right & down move increases
    - Don’t use BFS/DFS → waste of time
    - Don’t treat as binary matrix or DP
    - Carefully check boundaries
---

<!-- #endregion -->
