<!-- #region 137-Search A 2D Matrix -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q137: Search A 2D Matrix</h1>

## 1. Problem Understanding

- You are given a matrix where:
  * Each row is sorted from left → right.
  * The first element of each row is greater than the last element of previous row.
  * This means the entire matrix behaves like one sorted array.
- Goal:
  * Search if x exists.
  * Return true / false.
---

## 2. Constraints

- 1 <= m, n <= 1000
- m * n ≤ 10^6 → binary search is perfect.
- Values range from -10^4 to 10^4.
---

## 3. Edge Cases

- x smaller than smallest element → false
- x larger than largest element → false
- 1×1 matrix
- Single row or single column
- Large matrix → must use O(log(m*n))
---

## 4. Examples

```text
Example 1
Matrix:
1  3  5  7
10 11 16 20
23 30 34 60
x = 10 → true

Example 2
x = 12 → false
```

---

## 5. Approaches

### Approach 1: Brute Force (Check Every Element)

**Idea:**
- Scan all rows and all columns until element is found.

**Java Code:**
```java
public static boolean searchMatrix(int[][] mat, int x) {
    int m = mat.length, n = mat[0].length;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (mat[i][j] == x) return true;
        }
    }
    return false;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(m*n) — checks every cell
  * Why: No skipping possible.
- 💾 Space Complexity
  * O(1) — no extra space

### Approach 2: Row Selection + Binary Search (Better)

**Idea:**
- Identify which row x could be in:
  * Check first/last elements of each row.
- Binary search that row.

**Java Code:**
```java
public static boolean searchMatrix(int[][] mat, int x) {
    int m = mat.length, n = mat[0].length;

    for (int i = 0; i < m; i++) {
        if (x >= mat[i][0] && x <= mat[i][n-1]) {
            int l = 0, r = n-1;
            while (l <= r) {
                int mid = l + (r - l) / 2;
                if (mat[i][mid] == x) return true;
                else if (mat[i][mid] < x) l = mid + 1;
                else r = mid - 1;
            }
            return false;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Each row is sorted, so we can binary search inside the correct row.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Worst-case: O(m + log n)
  * Why: Scan rows → then binary search inside one row.
- 💾 Space Complexity
  * O(1)

### Approach 3: Treat as Sorted 1D Array (Optimal — O(log(m*n)))

**Idea:**
- Because:
  * Last element of row i < first element of row i+1
  * Matrix behaves like a single sorted array of length m*n.
- Map 1D index to matrix index:
  * row = mid / n
  * col = mid % n
- Binary search on 1D indexed search space.

**Java Code:**
```java
public static boolean searchMatrix(int[][] mat, int x) {
    int m = mat.length, n = mat[0].length;

    int l = 0, r = m * n - 1;

    while (l <= r) {
        int mid = l + (r - l) / 2;

        int row = mid / n;
        int col = mid % n;

        if (mat[row][col] == x) return true;
        else if (mat[row][col] < x) l = mid + 1;
        else r = mid - 1;
    }

    return false;
}
```

**💭 Intuition Behind the Approach:**
- You’re performing binary search on a flattened version of the matrix.
- Because matrix is globally sorted, this works flawlessly.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log(m*n))
  * Why: Classic binary search on total number of elements.
- 💾 Space Complexity
  * O(1) — constant space.

---

## 6. Justification / Proof of Optimality

- Brute force is too slow for large m*n.
- Row-wise selection reduces search but still touches many rows.
- Flattened binary search guarantees minimal comparisons and utilizes full sorted property — best approach.
---

## 7. Variants / Follow-Ups

- Search in a matrix where each row AND column is sorted (different problem, different approach).
- Find the number of occurrences of x.
- Return position instead of boolean.
---

## 8. Tips & Observations

- Whenever a matrix has this row-start > previous-row-end pattern → treat like 1D sorted list.
- Binary search remains the strongest pattern if global monotonicity exists.
---

<!-- #endregion -->
