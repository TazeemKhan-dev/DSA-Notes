<!-- #region 48-Pascal’s Triangle (I, II, III — Combined) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q48: Pascal’s Triangle (I, II, III — Combined)</h1>
<br>

## 1. Understand the Problem

- We have 3 variations of Pascal’s Triangle:
- Variant I: Find element at row r and column c.
- Variant II: Return the entire row r.
- Variant III: Return the first n rows.
- We will solve all three together in different approaches (Brute, Better, Optimal).
- Definition:
- Triangle starts with 1 at the top.
- Each row starts and ends with 1.
- Interior values: Pascal[r][c] = Pascal[r-1][c-1] + Pascal[r-1][c] (1-based indexing).

---

## 2. Constraints

- 1 ≤ r, c, n ≤ 30
- c ≤ r
- All values fit in 32-bit integer

---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
Row 1:        1
Row 2:       1 1
Row 3:      1 2 1
Row 4:     1 3 3 1
Row 5:    1 4 6 4 1
Row 6:   1 5 10 10 5 1
r=4, c=2
```
Output:
```text
3
```

**Example 2 (Normal Case):**
Input:
```text
Row 1:        1
Row 2:       1 1
Row 3:      1 2 1
Row 4:     1 3 3 1
Row 5:    1 4 6 4 1
Row 6:   1 5 10 10 5 1
r=5
```
Output:
```text
[1,4,6,4,1]
```

**Example 3 (Normal Case):**
Input:
```text
Row 1:        1
Row 2:       1 1
Row 3:      1 2 1
Row 4:     1 3 3 1
Row 5:    1 4 6 4 1
Row 6:   1 5 10 10 5 1
n=4
```
Output:
```text
[[1], [1,1], [1,2,1], [1,3,3,1]]
```


---

## 4. Approaches

### Approach 1: Brute Force (Build Full Triangle Always)

- **Idea:**
  - Use 2D array / list of lists to construct full triangle.
  - For Variant I: return triangle[r-1][c-1]
  - For Variant II: return triangle[r-1]
  - For Variant III: return triangle[0..n-1]

**Java Code:**
```java
import java.util.*;

class PascalBrute {

    static List<List<Integer>> buildTriangle(int rows) {
        List<List<Integer>> triangle = new ArrayList<>();
        for (int i = 0; i < rows; i++) {
            List<Integer> row = new ArrayList<>();
            row.add(1);
            for (int j = 1; j < i; j++)
                row.add(triangle.get(i-1).get(j-1) + triangle.get(i-1).get(j));
            if (i > 0) row.add(1);
            triangle.add(row);
        }
        return triangle;
    }

    // Variant I
    static int pascalValue(int r, int c) {
        return buildTriangle(r).get(r-1).get(c-1);
    }

    // Variant II
    static List<Integer> pascalRow(int r) {
        return buildTriangle(r).get(r-1);
    }

    // Variant III
    static List<List<Integer>> pascalTriangle(int n) {
        return buildTriangle(n);
    }
}
```

**Complexity:**
- Time: O(n²)
- Space: O(n²)

### Approach 2: Better (Avoid Building Whole Triangle)

- **Idea:**
  - ariant I: Recursive + memoization
  - Variant II: Iteratively build only the current row
  - Variant III: Still build full triangle (same as brute)

**Java Code:**
```java
import java.util.*;

class PascalBetter {
    static int[][] dp = new int[31][31];

    // Variant I
    static int pascalValue(int r, int c) {
        if (c == 1 || c == r) return 1;
        if (dp[r][c] != 0) return dp[r][c];
        return dp[r][c] = pascalValue(r-1, c-1) + pascalValue(r-1, c);
    }

    // Variant II
    static List<Integer> pascalRow(int r) {
        List<Integer> row = new ArrayList<>();
        row.add(1);
        for (int i = 1; i < r; i++) {
            List<Integer> newRow = new ArrayList<>();
            newRow.add(1);
            for (int j = 1; j < row.size(); j++)
                newRow.add(row.get(j-1) + row.get(j));
            newRow.add(1);
            row = newRow;
        }
        return row;
    }

    // Variant III
    static List<List<Integer>> pascalTriangle(int n) {
        return PascalBrute.buildTriangle(n);
    }
}
```

**Complexity:**
- Time: Variant I: O(rc),   Variant II: O(r²)  ,Variant III: O(n²)
- Space: Variant I: O(rc)  Variant II: O(r)  Variant III: O(n²)

### Approach 3: Optimal (Use Combinatorics / nCr)

- **Idea:**
  - Variant I: compute single element using nCr formula
  - Variant II: generate row using nCr iteratively
  - Variant III: generate each row using nCr

**Java Code:**
```java
import java.util.*;

class PascalOptimal {

    static int nCr(int n, int r) {
        long res = 1;
        for (int i = 0; i < r; i++)
            res = res * (n - i) / (i + 1);
        return (int) res;
    }

    // Variant I
    static int pascalValue(int r, int c) {
        return nCr(r-1, c-1);
    }

    // Variant II
    static List<Integer> pascalRow(int r) {
        List<Integer> row = new ArrayList<>();
        for (int i = 0; i < r; i++)
            row.add(nCr(r-1, i));
        return row;
    }

    // Variant III
    static List<List<Integer>> pascalTriangle(int n) {
        List<List<Integer>> triangle = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            List<Integer> row = new ArrayList<>();
            for (int j = 0; j < i; j++)
                row.add(nCr(i-1, j));
            triangle.add(row);
        }
        return triangle;
    }
}
```


---

## 5. Justification / Proof of Optimality

- Brute: Simple, works for all variants, high space/time.
- Better: Efficient for element/row queries, avoids full triangle.
- Optimal: Fastest for single element or row generation using combinatorics.

---

## 6. Variants / Follow-Ups

- Print Pascal’s triangle centered.
- Row sum = 2^(n-1)
- Values modulo 10^9+7 for large n
- Print kth diagonal
- Applications in combinatorics, probability, binomial expansions

---

## 7. Tips & Observations

- Row symmetry → Pascal[r][c] = Pascal[r][r-c+1]
- Row sum = 2^(r-1)
- Edges = 1
- Diagonals → 2nd diagonal = natural numbers, 3rd diagonal = triangular numbers
- Binomial theorem → row r = coefficients of (a+b)^(r-1)
- Fibonacci relation → sum of shallow diagonals = Fibonacci numbers
- Efficient nCr → iterative computation avoids overflow

<!-- #endregion -->

