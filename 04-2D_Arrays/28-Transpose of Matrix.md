<!-- #region 28-Transpose of Matrix -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q28: Transpose of Matrix</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given a square matrix of size N x N. We need to transpose it, i.e., switch rows and columns.
- **Goal:** Convert matrix[i][j] to matrix[j][i].  Do it in-place without using extra space.
- **Paraphrase:** Swap elements above the diagonal with the corresponding elements below the diagonal.

---

## 2. Constraints

- 1 <= N <= 100
- -10^3 <= mat[i][j] <= 10^3


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
1 1 1 1
2 2 2 2
3 3 3 3
4 4 4 4
```
Output:
```text
1 2 3 4
1 2 3 4
1 2 3 4
1 2 3 4
```


---

## 4. Approaches

### Approach 1: Brute Force (Using Extra Matrix)

- **Idea:**
  - Create a new matrix transpose[N][N]
  - Set transpose[j][i] = matrix[i][j]
  - Print the new matrix

**Java Code:**
```java
public static void transposeMatrixBrute(int[][] mat) {
    int N = mat.length;
    int[][] trans = new int[N][N];
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            trans[j][i] = mat[i][j];
        }
    }

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            System.out.print(trans[i][j] + " ");
        }
        System.out.println();
    }
}
```

**Complexity:**
- Time: O(N²)
- Space: O(N²)

### Approach 2: Optimal (In-Place Swap)

- **Idea:**
  - Only swap elements above the diagonal with elements below the diagonal
  - For all i < j, swap mat[i][j] with mat[j][i]

**Java Code:**
```java
public static void transposeMatrixOptimal(int[][] mat) {
    int N = mat.length;

    for (int i = 0; i < N; i++) {
        for (int j = i + 1; j < N; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[j][i];
            mat[j][i] = temp;
        }
    }

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            System.out.print(mat[i][j] + " ");
        }
        System.out.println();
    }
}
```

**Complexity:**
- Time: O(N²) → every element visited once
- Space: O(1) → in-place


---

## 5. Justification / Proof of Optimality

- Brute force works but uses extra space
- Optimal approach modifies the matrix in-place → meets the expected space complexity
- Swapping above diagonal with below diagonal ensures all elements are transposed

---

## 6. Variants / Follow-Ups

- Transpose non-square matrix → requires extra matrix
- Rotate matrix 90°, 180° → transpose + reverse operations
- Compute transpose without changing original matrix
- Use in algorithms like matrix multiplication optimization

<!-- #endregion -->

