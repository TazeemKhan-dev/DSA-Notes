<!-- #region 29-Rotate a Matrix by 90° (Clockwise & Anti-Clockwise) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q29: Rotate a Matrix by 90° (Clockwise & Anti-Clockwise)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an n x n square matrix. We need to rotate it 90° clockwise or 90° anti-clockwise.
- **Goal:** Clockwise: top-left → top-right → bottom-right → bottom-left  Anti-clockwise: top-left → bottom-left → bottom-right → top-right
- **Paraphrase:** Rotate the matrix in-place using minimal extra space, ideally O(1).

---

## 2. Constraints

- 1 <= n <= 100
- In-place solution required


---

## 3. Examples & Edge Cases

**Example 1 (Clockwise 90° Rotation:):**
Input:
```text
3 3
7 2 3
2 3 4
5 6 1
```
Output:
```text
5 2 7
6 3 2
1 4 3
```

**Example 2 (Anti-Clockwise 90° Rotation:):**
Input:
```text
3 3
7 2 3
2 3 4
5 6 1
```
Output:
```text
3 4 1
2 3 6
7 2 5
```


---

## 4. Approaches

### Approach 1: Brute Force (Using Extra Matrix)

- **Idea:**
  - Clockwise: rotated[j][n-1-i] = matrix[i][j]
  - Anti-clockwise: rotated[n-1-j][i] = matrix[i][j]

**Java Code:**
```java
// Clockwise
public static int[][] rotateMatrixClockwiseBrute(int[][] mat) {
    int n = mat.length;
    int[][] rotated = new int[n][n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            rotated[j][n-1-i] = mat[i][j];
        }
    }
    return rotated;
}

// Anti-Clockwise
public static int[][] rotateMatrixAntiClockwiseBrute(int[][] mat) {
    int n = mat.length;
    int[][] rotated = new int[n][n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            rotated[n-1-j][i] = mat[i][j];
        }
    }
    return rotated;
}
```

**Complexity:**
- Time: O(N²)
- Space: O(n²) → extra matrix

### Approach 2: Optimal In-Place (Transpose + Reverse)

- **Idea:**
  - Idea (Clockwise):
  - Transpose matrix in-place
  - Reverse each row
  - Idea (Anti-Clockwise):
  - Transpose matrix in-place
  - Reverse each column

**Java Code:**
```java
// Clockwise 90°
public static void rotateMatrixClockwise(int[][] mat) {
    int n = mat.length;
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[j][i];
            mat[j][i] = temp;
        }
    }
    // Reverse rows
    for (int i = 0; i < n; i++) {
        int left = 0, right = n-1;
        while (left < right) {
            int temp = mat[i][left];
            mat[i][left] = mat[i][right];
            mat[i][right] = temp;
            left++; right--;
        }
    }
}

// Anti-Clockwise 90°
public static void rotateMatrixAntiClockwise(int[][] mat) {
    int n = mat.length;
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[j][i];
            mat[j][i] = temp;
        }
    }
    // Reverse columns
    for (int j = 0; j < n; j++) {
        int top = 0, bottom = n-1;
        while (top < bottom) {
            int temp = mat[top][j];
            mat[top][j] = mat[bottom][j];
            mat[bottom][j] = temp;
            top++; bottom--;
        }
    }
}
```

**Complexity:**
- Time: O(n²) → transpose + reverse
- Space: O(1) → in-place


---

## 5. Justification / Proof of Optimality

- Brute force is simple but uses extra space
- Optimal approach rotates in-place → meets constraints
- Works for any square matrix, handles maximum n efficiently

---

## 6. Variants / Follow-Ups

- Rotate 180° → two 90° rotations
- Rotate by arbitrary angle (multiple of 90°)
- Rotate non-square matrix → requires extra matrix
- Combine rotation with matrix reflection operations

<!-- #endregion -->

