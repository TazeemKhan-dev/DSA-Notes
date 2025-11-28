<!-- #region 159-Find row with maximum 1's -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q159: Find row with maximum 1's</h1>

## 1. Problem Understanding

- You are given a binary matrix where:
  * Each row is sorted in non-decreasing order → pattern 0 0 0 ... 1 1 1
- You must return the index of the row that has the maximum number of 1s.
- If multiple rows have same count → return the smallest index.
- If no 1 exists in the grid → return -1.
- Goal:
  * Find the row with the maximum number of ones (using sorted row property efficiently).
---

## 2. Constraints

- 1 ≤ n, m ≤ 100
- Values are 0 or 1
- All rows sorted (0’s → 1’s)
---

## 3. Edge Cases

- Entire matrix is all 0’s → return -1
- Multiple rows with same number of 1’s
- First row has max 1’s
- Only one row
- Rows like [1,1,1] or [0,0,0]
---

## 4. Examples

```text
Example 1

mat = [
 [1,1,1],
 [0,0,1],
 [0,0,0]
]
Output: 0


Example 2

mat = [
 [0,0],
 [0,0]
]
Output: -1


Example 3

mat = [
 [0,0,1],
 [0,1,1],
 [0,1,1]
]
Output: 1   (Rows 1 and 2 have 2 ones, choose smaller index)
```

---

## 5. Approaches

### Approach 1: Brute Force (Count 1's per row)

**Idea:**
- For each row:
  * Count number of 1s using simple loop.
  * Keep track of row with maximum count.

**Steps:**
- For every row i:
  * Iterate all columns.
  * Count 1s.
- Track maxCount and index.
- Return index or -1.

**Java Code:**
```java
public int rowWithMax1sBrute(int[][] mat) {
    int n = mat.length, m = mat[0].length;
    int maxCount = 0;
    int rowIndex = -1;

    for(int i = 0; i < n; i++){
        int count = 0;
        for(int j = 0; j < m; j++){
            if(mat[i][j] == 1) count++;
        }
        if(count > maxCount){
            maxCount = count;
            rowIndex = i;
        }
    }

    return (maxCount == 0 ? -1 : rowIndex);
}
```

**💭 Intuition Behind the Approach:**
- Straightforward counting, but ignores sorted property.

**Complexity (Time & Space):**
- Time:
  * O(n * m)
- Space:
  * O(1)

### Approach 2: Binary Search in Each Row

**Idea:**
- Since each row is sorted:
  * Use binary search to find first occurrence of 1
  * Ones count = m - firstIndexOfOne

**Steps:**
- For each row:
  * Do binary search for first 1
- Using count = m - index
- Track max

**Java Code:**
```java
public int rowWithMax1sBS(int[][] mat) {
    int n = mat.length, m = mat[0].length;
    int maxCount = 0;
    int rowIndex = -1;

    for(int i = 0; i < n; i++){
        int firstOne = firstOneIndex(mat[i]);
        if(firstOne != -1){
            int count = m - firstOne;
            if(count > maxCount){
                maxCount = count;
                rowIndex = i;
            }
        }
    }

    return (maxCount == 0 ? -1 : rowIndex);
}

private int firstOneIndex(int[] row){
    int low = 0, high = row.length - 1, ans = -1;
    while(low <= high){
        int mid = low + (high - low) / 2;
        if(row[mid] == 1){
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Binary search uses the sorted nature of each row.

**Complexity (Time & Space):**
- Time:
  * O(n * log m)
- Space:
  * O(1)

### Approach 3: (Optimal): Start from Top-Right Corner

**Idea:**
- Use the property:
  * Rows sorted → if you see a 1, go left
  * If you see a 0, go down
- This visits each row at most once and each column at most once → O(n + m).

**Steps:**
- Start at (0, m-1) top-right corner
- While inside grid:
  * If mat[i][j] == 1:
    * Update answer row = i
    * Move left: j--
- Else:
  * Move down: i++
- Return found row or -1.

**Java Code:**
```java
public int rowWithMax1s(int[][] mat) {
    int n = mat.length, m = mat[0].length;

    int i = 0, j = m - 1;
    int rowIndex = -1;

    while(i < n && j >= 0){
        if(mat[i][j] == 1){
            rowIndex = i;
            j--;
        } else {
            i++;
        }
    }

    return rowIndex;
}
```

**💭 Intuition Behind the Approach:**
- Moving left reduces 1s count → valid updates
- Moving down checks next rows
- Top-right traversal ensures minimal operations
- This is a classic matrix trick (“staircase search”)

**Complexity (Time & Space):**
- Time:
  * O(n + m)
- Space:
  * O(1)

---

## 6. Justification / Proof of Optimality

- Brute force checks all cells → correct but slow.
- Binary search uses sorted row property → faster.
- Top-right corner method leverages global movement pattern:
  * From top-right, every left move means more 1s
  * Every down move eliminates current row
- Guarantees minimal row index if tie happens naturally.
---

## 7. Variants / Follow-Ups

- Search in a sorted 2D matrix
- Count 1s efficiently
- First 1 in a binary row
- Row with max zeros (flip 0 and 1 logic)
---

## 8. Tips & Observations

- Sorted rows ALWAYS hint at binary search or pointer sliding.
- Top-right pointer trick is VERY common in 2D problems.
- If all rows are sorted same way, global navigation is optimal.
- Keep track of row index only when moving left on a 1.

- **⚠️ Pitfalls**
    - Return -1 if no 1 exists (corner case)
    - Returning maxCount row instead of first such row
    - Forgetting sorted property and using unnecessary loops
    - Array bounds in top-right method
---

<!-- #endregion -->
