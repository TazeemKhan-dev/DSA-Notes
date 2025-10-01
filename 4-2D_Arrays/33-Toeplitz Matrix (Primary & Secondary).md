<!-- #region 33-Toeplitz Matrix (Primary & Secondary) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q33: Toeplitz Matrix (Primary & Secondary)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an m x n matrix.  Primary Toeplitz: Every ↘ diagonal (from top-left to bottom-right) has the same value.  Secondary Toeplitz: Every ↙ diagonal (from top-right to bottom-left) has the same value.
- **Goal:** Return true if the matrix is Toeplitz in the chosen sense (Primary or Secondary)
- **Paraphrase:** Primary: Check if mat[i][j] == mat[i-1][j-1].     Secondary: Check if mat[i][j] == mat[i-1][j+1].

---

## 2. Constraints

- 1 <= m, n <= 20
- -1000 <= matrix[i][j] <= 1000


---

## 3. Examples & Edge Cases

**Example 1 (Primary Diagonal):**
Input:
```text
3 3
1 2 3
4 1 2
5 4 1
```
Output:
```text
true
Explanation:
All ↘ diagonals (Primary) are equal → 1,1,1, 2,2, 3, etc.
```

**Example 2 (Secondary Diagona):**
Input:
```text
3 3
1 2 3
2 3 1
3 1 2
```
Output:
```text
true
Explanation:
All ↙ diagonals (Secondary) are equal → 3,3,3, 2,2, 1, etc.
```


---

## 4. Approaches

### Approach 1: Brute Force (Check Both Separately)

- **Idea:**
  - Use two boolean flags:
  - Check Primary Toeplitz (mat[i][j] == mat[i-1][j-1]).
  - Check Secondary Toeplitz (mat[i][j] == mat[i-1][j+1]).

**Java Code:**
```java
class Solution {
    public String checkToeplitz(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean isPrimary = true;
        boolean isSecondary = true;

        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // Primary Toeplitz check: ↘ (i-1, j-1)
                if (j > 0 && matrix[i][j] != matrix[i - 1][j - 1]) {
                    isPrimary = false;
                }
                // Secondary Toeplitz check: ↙ (i-1, j+1)
                if (j < n - 1 && matrix[i][j] != matrix[i - 1][j + 1]) {
                    isSecondary = false;
                }
            }
        }

        if (isPrimary && isSecondary) return "Both";
        if (isPrimary) return "Primary Toeplitz";
        if (isSecondary) return "Secondary Toeplitz";
        return "None";
    }
}
```

**Complexity:**
- Time: O(m * n)
- Space: O(1)

### Approach 2: HashMap (Unified Indexing)

- **Idea:**
  - Use keys to group diagonals:
  - Primary → key = i - j.
  - Secondary → key = i + j.
  - Store the first element for each diagonal in a map and check others.

**Java Code:**
```java
import java.util.*;

class Solution {
    public String checkToeplitz(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean isPrimary = true;
        boolean isSecondary = true;

        Map<Integer, Integer> primaryMap = new HashMap<>();
        Map<Integer, Integer> secondaryMap = new HashMap<>();

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int keyP = i - j; // key for primary diagonals
                int keyS = i + j; // key for secondary diagonals

                // Primary
                if (!primaryMap.containsKey(keyP)) {
                    primaryMap.put(keyP, matrix[i][j]);
                } else if (primaryMap.get(keyP) != matrix[i][j]) {
                    isPrimary = false;
                }

                // Secondary
                if (!secondaryMap.containsKey(keyS)) {
                    secondaryMap.put(keyS, matrix[i][j]);
                } else if (secondaryMap.get(keyS) != matrix[i][j]) {
                    isSecondary = false;
                }
            }
        }

        if (isPrimary && i
```

**Complexity:**
- Time: O(m * n)
- Space: O(m * n)


---

## 5. Justification / Proof of Optimality

- Brute Force → simple & optimal for small 20x20 matrices.
- HashMap → scalable if matrix grows larger (e.g., streaming input).

---

## 6. Variants / Follow-Ups

- Strictly Primary only Toeplitz check (original problem).
- Strictly Secondary only Toeplitz check.
- Check if matrix is Hankel matrix (same as Secondary Toeplitz).
- Allow up to k mismatches.
- Generate Toeplitz matrix programmatically.

<!-- #endregion -->

