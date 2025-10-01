<!-- #region 17-Buildings Pattern -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q17: Buildings Pattern</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an array arr of size n. Each element represents the height of a building. We need to print a building-like structure using *.
- **Goal:** Print a 2D pattern where each column represents a building.  The number of * in a column equals the value at that index in arr.  Columns are tab-separated.
- **Paraphrase:** Imagine a skyline of buildings where height is represented by *.  Build the output row by row from top to bottom.

---

## 2. Input, Output, & Constraints

**Constraints:**
- 1 ≤ n ≤ 1000
- 0 ≤ arr[i] ≤ 1000


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
7
9 3 7 6 2 0 4
```
Output:
```text
*                       
*                       
*               *       
*               *       *
*               *       *
*               *       *                       *
*       *       *       *                       *
*       *       *       *       *               *
*       *       *       *       *               *
```


---

## 4. Approaches

### Approach 1: Iterative Row-by-Row

- **Idea:**
  - Find maxHeight = max(arr)
  - For each row i from maxHeight down to 1:
  - For each column j from 0 to n-1:
  - If arr[j] >= i → print *
  - Else → print spaces
  - Separate columns with tabs

**Java Code:**
```java
public static void printBuildings(int[] arr) {
    int n = arr.length;
    int maxHeight = 0;
    for (int h : arr) maxHeight = Math.max(maxHeight, h);

    for (int i = maxHeight; i >= 1; i--) {
        for (int j = 0; j < n; j++) {
            if (arr[j] >= i) {
                System.out.print("*\t");
            } else {
                System.out.print("\t");
            }
        }
        System.out.println();
    }
}
```

**Complexity:**
- Time: O(n * maxHeight) → each cell is visited once
- Space: O(1) → in-place printing


---

## 5. Justification / Proof of Optimality

- Correctly prints building heights row-by-row from top to bottom.
- Handles variable heights and tab alignment.
- Efficient for n ≤ 1000 and arr[i] ≤ 1000.

---

## 6. Variants / Follow-Ups

- Print rotated skyline (90-degree rotation)
- Print using different symbols or colors for buildings
- Print hollow buildings (outline only)
- Print mirrored buildings (center-aligned)

<!-- #endregion -->

