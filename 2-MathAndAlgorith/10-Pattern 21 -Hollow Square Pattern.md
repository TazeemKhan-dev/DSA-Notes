
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q10: Pattern 21 -Hollow Square Pattern</h1>

## 1. Input, Output, & Constraints

- **Input:**
```text
5
```

- **Output:**
```text
*****
*   *
*   *
*   *
*****
```

**Constraints:**
- 1 ≤ n ≤ 26 (English alphabets)

## 2. Examples

**Example 1 (edge case):**
Input:
```text
2
```
Output:
```text
**
**
```

## 3. Approaches

### Approach 1: Using Nested Loops

- **Idea:**
  - Loop through each row
  - If row is first or last → print all *
  - Otherwise → print * at first and last column, spaces in between

**Pseudocode:**
```text
function printHollowSquare(n):
    for i in range(1, n+1):
        for j in range(1, n+1):
            if i == 1 or i == n or j == 1 or j == n:
                print("*", end="")
            else:
                print(" ", end="")
        print()   # New line after each row
```

**Complexity:**
- Time: O(n^2) → Nested loops for n rows × n columns
- Space: O(1) → Only loop variables

### Approach 2: String Concatenation (Optional)

- **Idea:**
  - Precompute strings for first/last row and middle rows
  - Print first/last row directly, print middle row n-2 times

**Pseudocode:**
```text
function printHollowSquare(n):
    full_row = "*" * n
    middle_row = "*" + " " * (n-2) + "*" if n > 1 else "*"
    
    print(full_row)
    for i in range(1, n-1):
        print(middle_row)
    if n > 1:
        print(full_row)
```

**Complexity:**
- Time: O(n^2)  → Still iterating over n rows × n columns
- Space: O(1)→ For storing row strings

## 4. Justification / Proof of Optimality

- Optimality: Both approaches are straightforward and efficient for printing a hollow square.
- Comparison:
- Nested loop → Easy to understand for beginners, prints directly
- String concatenation → Slightly more efficient if row strings are reused

## 5. Variants / Follow-Ups

- Hollow rectangle (rows ≠ columns)
- Hollow triangle, hollow diamond
- Filled border patterns with different characters
- Hollow square with diagonal * inside

