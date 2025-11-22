<!-- #region 11- Pattern 22 : Number Square with Decreasing Layers -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q11: Pattern 22 : Number Square with Decreasing Layers</h1>
<br>

## 1. Input, Output, & Constraints

- **Input:**
```text
5
```

- **Output:**
```text
5 5 5 5 5 5 5 5 5
5 4 4 4 4 4 4 4 5
5 4 3 3 3 3 3 4 5
5 4 3 2 2 2 3 4 5
5 4 3 2 1 2 3 4 5
5 4 3 2 2 2 3 4 5
5 4 3 3 3 3 3 4 5
5 4 4 4 4 4 4 4 5
5 5 5 5 5 5 5 5 5
```

**Constraints:**
- 1 ≤ n ≤ 26 (English alphabets)


---

## 2. Examples & Edge Cases

**Example 1 (edge case):**
Input:
```text
2
```
Output:
```text
2 2 2
2 1 2
2 2 2
```


---

## 3. Approaches

### Approach 1: Using Distance from Edges

- **Idea:**
  - For a position (i, j) in the square, the value = n - min(min(i, j), min(size-1-i, size-1-j))
  - Here, size = 2*n - 1

**Java Code:**
```java
public static void printPattern22(int n) {
    int size = 2 * n - 1;  // Total rows and columns

    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            int top = i;
            int left = j;
            int right = size - 1 - j;
            int bottom = size - 1 - i;

            int minDistance = Math.min(Math.min(top, bottom), Math.min(left, right));
            int value = n - minDistance;

            System.out.print(value + " ");
        }
        System.out.println();  // Move to next row
    }
}
```

**Complexity:**
- Time: O(n²) → Double loop for (2*n-1) x (2*n-1) elements
- Space: O(1) → Only loop variables


---

## 4. Justification / Proof of Optimality

- Optimality: Each element is computed in O(1) using distance from edges, so the approach is efficient.
- Symmetry: Works for any n and automatically handles center and layers.

---

## 5. Variants / Follow-Ups

- Use letters instead of numbers
- Print pattern in hollow style (only borders of layers)
- Diagonal or rotated versions of the pattern

<!-- #endregion -->

