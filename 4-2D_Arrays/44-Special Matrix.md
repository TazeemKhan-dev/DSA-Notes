<!-- #region 44-Special Matrix -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q44: Special Matrix</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Determine whether a given square matrix is a special matrix.  Definition:  Diagonal elements (matrix[i][i]) are non-zero.  All non-diagonal elements (matrix[i][j] where i ≠ j) are zero.
- **Goal:** Return true if the matrix satisfies the above conditions, otherwise false.
- **Paraphrase:** Only diagonal elements are non-zero; everything else must be zero.

---

## 2. Constraints

- 1 <= T <= 10
- 1 <= N <= 200
- 0 <= A[i][j] <= 10^6


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
1
3
1 0 2
0 2 0
3 0 1
```
Output:
```text
true
```

**Example 2 (Normal Case):**
Input:
```text
1
3
1 0 1
1 2 0
2 0 3
```
Output:
```text
false
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Traverse the entire matrix.
  - For each element matrix[i][j]:
  - If i == j → check non-zero
  - Else → check zero

**Java Code:**
```java
public boolean isSpecialMatrix(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i == j) {
                if (matrix[i][j] == 0) return false;
            } else {
                if (matrix[i][j] != 0) return false;
            }
        }
    }
    return true;
}
```

**Complexity:**
- Time: O(N^2) → visit all elements.
- Space: O(1) → in-place.

### Approach 2: Optimized (Early Exit)

- **Idea:**
  - Traverse row by row.
  - As soon as a non-diagonal element is non-zero or a diagonal element is zero → return false.
  - Otherwise, after traversal → return true.

**Java Code:**
```java
public boolean isSpecialMatrixOptimized(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n; i++) {
        if (matrix[i][i] == 0) return false; // diagonal must be non-zero
        for (int j = 0; j < n; j++) {
            if (i != j && matrix[i][j] != 0) return false; // non-diagonal must be zero
        }
    }
    return true;
}
```

**Complexity:**
- Time: O(N^2) in worst-case, but can exit early.
- Space: O(1) → in-place.


---

## 5. Justification / Proof of Optimality

- Both approaches correctly check all diagonal and non-diagonal conditions.
- Optimized approach can exit early, saving some comparisons when matrix is not special.

---

## 6. Variants / Follow-Ups

- Check if a matrix is diagonal (diagonal elements can be zero).
- Check for identity matrix (diagonal elements must be 1, others 0).
- Special matrices with non-square matrices → check main diagonal only.

<!-- #endregion -->

