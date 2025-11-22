<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q3: Print Number Pattern 3</h1>
<br>


## 1. Input, Output, & Constraints
- **Input:**
```text
5
```
- **Output:**
```text
0
1 1
2 3 5
8 13 21 34
55 89 144 233 377
```
**Constraints:**
- 1 ≤ n ≤ 20
- Target time complexity: O(n²)
- Target space complexity: O(1) if generating on the fly

---

## 2. Examples & Edge Cases

**Example 1 (Single Row):**
Input:
```text
1
```
Output:
```text
0
```

**Example 2 (Two Rows):**
Input:
```text
2
```
Output:
```text
0
1 1
```

---

## 3. Approaches
### Approach 1: Generate On the Fly (Optimal)
**Idea:** Keep track of the last two Fibonacci numbers and generate numbers row by row. Print them immediately or store in a list.

**Pseudocode:**
```
function printFibonacciTriangle(n):
    a = 0, b = 1
    for row = 1 to n:
        for i = 1 to row:
            print a
            c = a + b
            a = b
            b = c

```

**Complexity:**
- Time: O(n²)
- Space: O(1) (no extra storage needed)

---

## 4. Variants / Follow-Ups
- Print the triangle in reverse (largest row first).
- Right-align the triangle for better formatting.
- Generate similar patterns for other sequences (Tribonacci, Lucas numbers).
- Store all numbers in a single-line format for API submission or further processing.


