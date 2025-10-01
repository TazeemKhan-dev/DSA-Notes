<!-- #region 34-Diagonal Traversal Function (Top-Right & Top-Left) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q34: Diagonal Traversal Function (Top-Right & Top-Left)</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** We start from either top-right corner or top-left corner, and move diagonal by diagonal until the end.

---

## 2. Constraints

- 1 <= N <= 500
- -10^4 <= mat[i][j] <= 10^4


---

## 3. Examples & Edge Cases

**Example 1 (Top-Right → Bottom-Left):**
Input:
```text
3
1 2 3
4 5 6
7 8 9
topRight
```
Output:
```text
3 2 6 1 5 9 4 8 7
```

**Example 2 (Top-Left → Bottom-Right):**
Input:
```text
3
1 2 3
4 5 6
7 8 9
topLeft
```
Output:
```text
1 2 4 3 5 7 6 8 9
```


---

## 4. Approaches

### Approach 1: Brute Force Using Extra Storage

- **Idea:**
  - Store each diagonal in a list and print later.

**Java Code:**
```java
List<Integer> result = new ArrayList<>();
// Traverse diagonals, store elements in result
// Print result at end
```

**Complexity:**
- Time: O(N^2) — every element visited once.
- Space: O(N^2) — storing traversal in a list.

### Approach 2: Optimal In-Place Traversal

- **Idea:**
  - Idea: Print elements while traversing diagonals without extra memory.
  - Steps:
  - For Top-Right → Bottom-Left:
  - Start from top row, last column → traverse diagonals.
  - Then start from first column, row 1 → traverse remaining diagonals.
  - For Top-Left → Bottom-Right:
  - Start from top row, first column → traverse diagonals.
  - Then start from last column, row 1 → traverse remaining diagonals.

**Java Code:**
```java
public static void diagonalTraversalInPlace(int[][] mat, int n, String direction) {
    if(direction.equalsIgnoreCase("topRight")) {
        for(int col=n-1; col>=0; col--) {
            int i=0, j=col;
            while(i<n && j<n) { System.out.print(mat[i][j]+" "); i++; j++; }
        }
        for(int row=1; row<n; row++) {
            int i=row, j=0;
            while(i<n && j<n) { System.out.print(mat[i][j]+" "); i++; j++; }
        }
    } else if(direction.equalsIgnoreCase("topLeft")) {
        for(int col=0; col<n; col++) {
            int i=0, j=col;
            while(i<n && j>=0) { System.out.print(mat[i][j]+" "); i++; j--; }
        }
        for(int row=1; row<n; row++) {
            int i=row, j=n-1;
            while(i<n && j>=0) { System.out.print(mat[i][j]+" "); i++; j--; }
        }
    }
}
```

**Complexity:**
- Time: O(N^2)
- Space: O(1) (no extra list)


---

## 5. Justification / Proof of Optimality

- In-place solution is optimal: no extra memory, linear traversal of all elements.
- Brute-force using extra list is unnecessary for large N.
- Complexity is optimal for N x N matrix: O(N^2) time, O(1) space.

---

## 6. Variants / Follow-Ups

- Non-square matrix (M x N) — can adapt traversal loops.
- Anti-diagonal traversal — starting from bottom-right instead of top corners.
- Diagonal sum instead of traversal — compute sums during traversal.

<!-- #endregion -->

