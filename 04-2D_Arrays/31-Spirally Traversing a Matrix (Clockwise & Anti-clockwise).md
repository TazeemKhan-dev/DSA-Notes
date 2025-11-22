<!-- #region 31-Spirally Traversing a Matrix (Clockwise & Anti-clockwise) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q31: Spirally Traversing a Matrix (Clockwise & Anti-clockwise)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** You are given a matrix of size m x n.  Task is to print all elements in spiral order:  Clockwise → top row → right column → bottom row → left column → shrink boundaries, repeat.  Anti-clockwise → left column → bottom row → right column → top row → shrink boundaries, repeat.
- **Goal:** Print traversal order of the matrix elements.
- **Paraphrase:** Keep four boundaries (top, bottom, left, right) and traverse them layer by layer until all elements are covered.

---

## 2. Constraints

- 1 <= m, n <= 100
- -10^3 <= mat[i][j] <= 10^3


---

## 3. Examples & Edge Cases

**Example 1 (Clockwise):**
Input:
```text
3 3
1 2 3
4 5 6
7 8 9
```
Output:
```text
1 2 3 6 9 8 7 4 5
```

**Example 2 (Anti-clockwise):**
Input:
```text
3 3
1 2 3
4 5 6
7 8 9
```
Output:
```text
1 4 7 8 9 6 3 2 5
```


---

## 4. Approaches

### Approach 1: Brute Force (Simulate Layer-by-Layer)

- **Idea:**
  - Maintain top, bottom, left, right indices.
  - Clockwise order: top → right → bottom → left.
  - Anti-clockwise order: left → bottom → right → top.
  - After traversing a boundary, shrink it (top++, bottom--, left++, right--).

**Java Code:**
```java
Java Code (Clockwise Spiral Traversal)

public static void spiralClockwise(int[][] mat) {
    int m = mat.length, n = mat[0].length;
    int top = 0, bottom = m - 1, left = 0, right = n - 1;

    while (top <= bottom && left <= right) {
        // Traverse top row
        for (int j = left; j <= right; j++)
            System.out.print(mat[top][j] + " ");
        top++;

        // Traverse right column
        for (int i = top; i <= bottom; i++)
            System.out.print(mat[i][right] + " ");
        right--;

        // Traverse bottom row
        if (top <= bottom) {
            for (int j = right; j >= left; j--)
                System.out.print(mat[bottom][j] + " ");
            bottom--;
        }

        // Traverse left column
        if (left <= right) {
            for (int i = bottom; i >= top; i--)
                System.out.print(mat[i][left] + " ");
            left++;
        }
    }
}


Java Code (Anti-clockwise Spiral Traversal)

public static void spiralAntiClockwise(int[][] mat) {
    int m = mat.length, n = mat[0].length;
    int top = 0, bottom = m - 1, left = 0, right = n - 1;

    while (top <= bottom && left <= right) {
        // Traverse left column
        for (int i = top; i <= bottom; i++)
            System.out.print(mat[i][left] + " ");
        left++;

        // Traverse bottom row
        for (int j = left; j <= right; j++)
            System.out.print(mat[bottom][j] + " ");
        bottom--;

        // Traverse right column
        if (left <= right) {
            for (int i = bottom; i >= top; i--)
                System.out.print(mat[i][right] + " ");
            right--;
        }

        // Traverse top row
        if (top <= bottom) {
            for (int j = right; j >= left; j--)
                System.out.print(mat[top][j] + " ");
            top++;
        }
    }
}
```

**Complexity:**
- Time: O(m*n) → Every element is visited once.
- Space: O(1) → in-place


---

## 5. Variants / Follow-Ups

- Return spiral traversal as an array instead of printing.
- Zigzag spiral traversal.
- Multi-layered spiral (spiral inward, then outward).

<!-- #endregion -->

