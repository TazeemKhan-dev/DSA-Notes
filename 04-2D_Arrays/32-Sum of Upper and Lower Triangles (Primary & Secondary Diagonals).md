<!-- #region 32-Sum of Upper and Lower Triangles (Primary & Secondary Diagonals) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q32: Sum of Upper and Lower Triangles (Primary & Secondary Diagonals)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an n x n square matrix.  Task: Calculate sum of upper triangle and lower triangle w.r.t a diagonal.  Upper Triangle: diagonal + elements above it.  Lower Triangle: diagonal + elements below it.  Can be asked for:  Primary diagonal (↘ top-left → bottom-right).  Secondary diagonal (↙ top-right → bottom-left).

---

## 2. Constraints

- 1 ≤ n ≤ 1000
- -10³ ≤ mat[i][j] ≤ 10³


---

## 3. Examples & Edge Cases

**Example 1 (Primary Diagonal):**
Input:
```text
3
1 2 3
1 5 3
4 5 6
```
Output:
```text
20 22
Explanation:

Upper (primary): 1 + 2 + 3 + 5 + 3 + 6 = 20

Lower (primary): 1 + 1 + 4 + 5 + 5 + 6 = 22
```

**Example 2 (Secondary Diagona):**
Input:
```text
3
1 2 3
4 5 6
7 8 9
```
Output:
```text
23 21
Explanation:

Upper (secondary): includes diagonal + above it → 3 + 2 + 1 + 5 + 7 + 9 = 23

Lower (secondary): includes diagonal + below it → 3 + 6 + 9 + 5 + 8 = 21
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Traverse entire matrix twice:
  - First time → compute upper triangle sum.
  - Second time → compute lower triangle sum.
  - Works but has unnecessary double traversal.

**Java Code:**
```java
public static void triangleSumsBrute(int n, int[][] mat, boolean primary) {
    int upper = 0, lower = 0;

    // Upper Triangle
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (primary) { // primary diagonal
                if (j >= i) upper += mat[i][j];
            } else { // secondary diagonal
                if (i + j <= n - 1) upper += mat[i][j];
            }
        }
    }

    // Lower Triangle
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (primary) {
                if (j <= i) lower += mat[i][j];
            } else {
                if (i + j >= n - 1) lower += mat[i][j];
            }
        }
    }

    System.out.println(upper + " " + lower);
}
```

**Complexity:**
- Time: O(n²) × 2 = O(n²).
- Space: O(1).

### Approach 2: Optimal (Single Traversal)

- **Idea:**
  - Traverse matrix once.
  - Use conditions:
  - Primary Diagonal:
  - j >= i → upper, j <= i → lower.
  - Secondary Diagonal:
  - i + j <= n-1 → upper, i + j >= n-1 → lower.

**Java Code:**
```java
public static void triangleSumsOptimal(int n, int[][] mat, boolean primary) {
    int upper = 0, lower = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (primary) {  // Primary Diagonal
                if (j >= i) upper += mat[i][j];
                if (j <= i) lower += mat[i][j];
            } else {        // Secondary Diagonal
                if (i + j <= n - 1) upper += mat[i][j];
                if (i + j >= n - 1) lower += mat[i][j];
            }
        }
    }

    System.out.println(upper + " " + lower);
}
```

**Complexity:**
- Time: O(n²).
- Space: O(1).


---

## 5. Justification / Proof of Optimality

- Brute Force
- Traverse twice → once for upper, once for lower.
- Simple to understand, easy to debug.
- Works for both primary & secondary diagonals by just changing the condition.
- But redundant (two passes).
- Optimal (Single Traversal)
- Compute both sums in one loop.
- Reduces traversal overhead → still O(n²) but constant factors smaller.
- Cleaner & more efficient in practice.

---

## 6. Variants / Follow-Ups

- Variant A: Primary Diagonal (with diagonal) ✅ (default problem).
- Variant B: Secondary Diagonal (with diagonal) ✅ (extension).
- Variant C: Both diagonals at once ✅ (extended version).
- Variant D: Excluding diagonals ❌ (less common, but sometimes asked).

<!-- #endregion -->

