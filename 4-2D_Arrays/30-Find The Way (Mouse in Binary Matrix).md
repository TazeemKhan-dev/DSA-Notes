<!-- #region 30-Find The Way (Mouse in Binary Matrix) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q30: Find The Way (Mouse in Binary Matrix)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** A mouse enters a binary matrix at (0,0) moving left to right.  It moves straight on 0.  It turns right and changes 1 → 0 when it encounters 1.  Determine exit coordinates when the mouse leaves the matrix.
- **Goal:** Simulate the mouse movement until it exits the matrix.
- **Paraphrase:** Start at (0,0), follow direction rules:  0 → continue  1 → turn right, set 1 → 0  Stop when the next move goes outside the matrix bounds

---

## 2. Constraints

- 1 <= m, n <= 100
- matrix[i][j] ∈ {0,1}


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
3 3
0 1 0
0 1 0
1 0 1
```
Output:
```text
1 0
```

**Example 2 (Normal Case):**
Input:
```text
1 2
0 0
```
Output:
```text
0 1
```


---

## 4. Approaches

### Approach 1: Brute Force (Simulate Movement)

- **Idea:**
  - Maintain current position (i, j) and direction (0:right,1:down,2:left,3:up)
  - Move according to rules:
  - matrix[i][j] = 0 → continue
  - matrix[i][j] = 1 → turn right, set to 0
  - Stop when (i,j) goes outside bounds

**Java Code:**
```java
public static int[] findMouseExit(int[][] mat) {
    int m = mat.length;
    int n = mat[0].length;

    int dir = 0; // 0=right, 1=down, 2=left, 3=up
    int i = 0, j = 0;

    while (i >= 0 && i < m && j >= 0 && j < n) {
        if (mat[i][j] == 1) {
            mat[i][j] = 0; // change 1 → 0
            dir = (dir + 1) % 4; // turn right
        }

        // Move in current direction
        if (dir == 0) j++;
        else if (dir == 1) i++;
        else if (dir == 2) j--;
        else if (dir == 3) i--;
    }

    // Exit is last valid position
    if (dir == 0) j--; // right exit
    else if (dir == 1) i--; // down exit
    else if (dir == 2) j++; // left exit
    else if (dir == 3) i++; // up exit

    return new int[]{i, j};
}
```

**Complexity:**
- Time: O(m*n) → each cell is visited at most once
- Space: O(1) → in-place


---

## 5. Justification / Proof of Optimality

- Simulating step-by-step ensures all turns and updates are handled correctly.
- Changing 1 → 0 prevents infinite loops.
- Works for all matrix sizes within constraints.

---

## 6. Variants / Follow-Ups

- Mouse starting at arbitrary cell
- Multiple mice moving simultaneously → detect collisions
- 3D matrix → simulate 3D movement rules
- Count steps until exit

<!-- #endregion -->

