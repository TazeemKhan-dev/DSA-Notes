<!-- #region 48-Pascal’s Triangle I, II, III -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q48: Pascal’s Triangle I, II, III</h1>

## 1. Problem Understanding

- Pascal’s Triangle I: Return the value at a specific row r and column c (1-indexed).
- Pascal’s Triangle II: Return all values in the r-th row (1-indexed).
- Pascal’s Triangle III: Return the first n rows of Pascal’s Triangle.
- Key differences:
- I → Single value lookup.
- II → Entire row.
- III → Full triangle up to n rows.
- All variants follow the rule:
- Pascal[r][c] = Pascal[r-1][c-1] + Pascal[r-1][c] with 1-indexed rows and columns.
---

## 2. Constraints

- Pascal’s Triangle I → 1 ≤ r, c ≤ 30, c ≤ r
- Pascal’s Triangle II → 1 ≤ r ≤ 30
- Pascal’s Triangle III → 1 ≤ n ≤ 30
- All values fit in a 32-bit integer.
---

## 3. Edge Cases

- c = 1 or c = r → Always 1.
- r = 1 → Only first element.
- Smallest row (r = 1) or first n rows (n = 1).
- Maximum row (r = 30) to test large numbers.
- Empty output (n = 0 or invalid index) → Handle gracefully.
---

## 4. Examples

### Example 1:

```text
(Pascal’s Triangle I):
Input: r = 4, c = 2 → Output: 3
Explanation: Row 4 → [1, 3, 3, 1] → value at column 2 = 3
Row 1:        1
Row 2:       1 1
Row 3:      1 2 1
Row 4:     1 3 3 1
```

### Example 2:

```text
(Pascal’s Triangle II):
Input: r = 5 → Output: [1, 4, 6, 4, 1]
Row 1:        1
Row 2:       1 1
Row 3:      1 2 1
Row 4:     1 3 3 1
Row 5:    1 4 6 4 1
```

### Example 3:

```text
(Pascal’s Triangle III):
Input: n = 4 → Output: [[1], [1, 1], [1, 2, 1], [1, 3, 3, 1]]
Row 1:        1
Row 2:       1 1
Row 3:      1 2 1
Row 4:     1 3 3 1
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Build Pascal’s Triangle row by row until required value, row, or n rows.

**Steps:**
- Start from first row [1].
- For each subsequent row, compute elements using current[j] = previous[j-1] + previous[j].
- Store or return required value/row/all rows.

**Java Code:**
```java
// Brute Force: Build Pascal Triangle iteratively
class PascalTriangle {
    // Return single value (I), row (II), or all rows (III)
    public int getValue(int r, int c) {
        List<List<Integer>> triangle = buildTriangle(r);
        return triangle.get(r-1).get(c-1);
    }

    public List<Integer> getRow(int r) {
        List<List<Integer>> triangle = buildTriangle(r);
        return triangle.get(r-1);
    }

    public List<List<Integer>> getRows(int n) {
        return buildTriangle(n);
    }

    private List<List<Integer>> buildTriangle(int n) {
        List<List<Integer>> triangle = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            List<Integer> row = new ArrayList<>();
            for (int j = 0; j <= i; j++) {
                if (j == 0 || j == i) row.add(1);
                else row.add(triangle.get(i-1).get(j-1) + triangle.get(i-1).get(j));
            }
            triangle.add(row);
        }
        return triangle;
    }
}
```

**Complexity (Time & Space):**
- Pascal I → Time: O(r²), Space: O(r²)
- Pascal II → Time: O(r²), Space: O(r²)
- Pascal III → Time: O(n²), Space: O(n²)

### Approach 2: Better / Improved (Row Optimization)

**Idea:**
- Instead of storing the entire triangle, generate only required row or value.
- Use previous row to compute current row.

**Steps:**
- Initialize first row [1].
- Iteratively compute next row using only previous row.
- For single value, stop at required column.
- For row, return last computed row.

**Java Code:**
```java
class PascalTriangleOptimized {
    public int getValue(int r, int c) {
        List<Integer> row = getRow(r);
        return row.get(c-1);
    }

    public List<Integer> getRow(int r) {
        List<Integer> row = new ArrayList<>();
        row.add(1);
        for (int i = 1; i < r; i++) {
            row.add(0); // expand row
            for (int j = i; j > 0; j--) {
                row.set(j, row.get(j) + row.get(j-1));
            }
        }
        return row;
    }

    public List<List<Integer>> getRows(int n) {
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            result.add(new ArrayList<>(getRow(i)));
        }
        return result;
    }
}
```

**Complexity (Time & Space):**
- Pascal I → Time: O(r²), Space: O(r)
- Pascal II → Time: O(r²), Space: O(r)
- Pascal III → Time: O(n²), Space: O(r) per row

### Approach 3: Optimal / Most Efficient (Binomial Coefficient)

**Idea:**
- Use combinatorial formula: C(r-1, c-1) = (r-1)! / ((c-1)! * (r-c)!).
- Compute row or value directly using combination formulas.

**Steps:**
- For Pascal I → Compute single combination.
- For Pascal II → Compute each element using C(r-1, i).
- For Pascal III → Generate each row using combination formula iteratively.

**Java Code:**
```java
class PascalTriangleOptimal {
    public int getValue(int r, int c) {
        return combination(r-1, c-1);
    }

    public List<Integer> getRow(int r) {
        List<Integer> row = new ArrayList<>();
        int val = 1;
        for (int i = 0; i < r; i++) {
            row.add(val);
            val = val * (r-1-i) / (i+1);
        }
        return row;
    }

    public List<List<Integer>> getRows(int n) {
        List<List<Integer>> triangle = new ArrayList<>();
        for (int i = 1; i <= n; i++) triangle.add(getRow(i));
        return triangle;
    }

    private int combination(int n, int k) {
        int res = 1;
        for (int i = 0; i < k; i++) res = res * (n-i) / (i+1);
        return res;
    }
}
```

**Complexity (Time & Space):**
- Pascal I → Time: O(c), Space: O(1)
- Pascal II → Time: O(r), Space: O(r)
- Pascal III → Time: O(n²), Space: O(r) per row

---

## 6. Justification / Proof of Optimality

- Brute Force: Simple, builds full triangle, high space usage.
- Better / Optimized: Reduces space for single row/value, still O(r²) time.
- Optimal / Binomial: Fastest for single value or row, uses formula, minimal space.
---

## 7. Variants / Follow-Ups

- Pascal’s Triangle modulo m
- Generate middle element only
- Print triangle upside-down or in other patterns
- Count paths in grid using combinatorial logic
---

## 8. Tips & Observations

- First and last elements of any row are always 1.
- Each row can be generated iteratively using previous row (space optimized).
- Binomial coefficient formula avoids building entire triangle.
- Use long type if intermediate factorials may exceed 32-bit integers.
---

<!-- #endregion -->
