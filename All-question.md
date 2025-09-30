<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q1: Pattern 11 — Alternating 1-0 Triangle</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an integer n, print n lines where line i (1-indexed) contains i numbers, alternating 1 and 0, starting with 1 on odd-numbered lines and 0 on even-numbered lines  
- **Goal:**  Recreate the displayed pattern exactly for any n.
- **Paraphrase:**  Paraphrase: For each row i from 1 to n, print i values alternating between 1 and 0; if the row number is odd, start with 1, otherwise start with 0.

---

## 2. Input, Output, & Constraints
- **Input:**  Single integer n (number of rows).
- **Output:**  n lines, line i containing i space-separated digits (1/0) forming the alternating pattern.

**Constraints:**  
- 1 ≤ n ≤ 10^5 (practical limits for printing depend on environment; very large n will be I/O heavy)
- Time complexity target: O(n²) is acceptable because output size is Θ(n²).

---

## 3. Examples & Edge Cases

**Example(n = 5):**  
```
1
0 1
1 0 1
0 1 0 1
1 0 1 0 1
```

**Edge Case Checklist:**  
- n = 1 → prints 1
- small n values (2,3)
- large n → ensure efficient printing (use buffered output)
- check behavior for n = 0 (problem typically assumes n ≥ 1 

---

## 4. Approaches 
### Approach 1:Direct Pattern Generation (Simple & Clear)
**Idea:**  For each row i from 1..n:
Determine starting value start = (i % 2 == 1) ? 1 : 0.
Print i values, toggling (val = 1 - val) after each printed number.

**Pseudocode:**  
```text
for i from 1 to n:
    if i is odd:
        val = 1
    else:
        val = 0
    for j from 1 to i:
        print val (with space if needed)
        val = 1 - val
    print newline

```


**Complexity:**  
- Time: O(n²) — you must print O(n²) numbers (1 + 2 + ... + n).
- Space:O(1) extra (excluding output buffer).


### Approach 2: Using Row Index Parity and j Parity (Alternative formulation)
**Idea:**  You can compute the value at position j in row i as:
        value = (i + j) % 2 == 0 ? 1 : 0 if you want to use an arithmetic formula (check indexing convention).
        This avoids explicit toggling, though performance is equivalent.

**Pseudocode:**  
```text
for i from 1 to n:
    for j from 1 to i:
        val = ((i + j) % 2 == 0) ? 1 : 0
        print val
    newline

```

**Complexity:**  
- Time:O(n²)
- Space: O(1)  


---

## 6. Justification / Proof of Optimality
- Printing every required number is necessary, total output size is Θ(n²) (sum of 1..n). Any correct solution must produce that many tokens, so O(n²) time is optimal up to constant factors for this problem.

- Both approaches produce correct alternating values; the toggle method is straightforward and avoids repeated arithmetic, while the formula method is compact and declarative.

##  7. Variants / Follow-Ups
- Change separators (no spaces, commas).
- Start each row with the opposite bit (i.e., always start with 0).
- Print a similar pattern in a matrix/2D grid shape.
- Convert to characters (A/B or X/O) instead of 1/0.

---
>#
<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q2: Diamond Pattern</h1>
<br>
## 1. Understand the Problem
- **Read & Identify:** Given an odd integer N, print a diamond of stars * with height = N. 
- **Goal:** The pattern should be symmetric vertically and horizontally
- **Paraphrase:** Print the upper pyramid (increasing stars), then the lower pyramid (decreasing stars), forming a diamond.

---

## 2. Input, Output, & Constraints
- **Input:**  odd integer N (height of diamond)
- **Output:**   print the diamond pattern with height N

**Constraints:**  
- 1 ≤ T ≤ 100
- 1 ≤ N ≤ 199 (must be odd)
- Printing size ~ O(N²), which is optimal since output itself is Θ(N²).

---

## 3. Examples & Edge Cases

**Example:**  
Input: 5
Output: 
```
  *  
 ***  
*****  
 ***  
  * 
```


---

## 4. Approaches 
### Approach 1:— Direct Simulation with Two Loops
**Idea:** The diamond can be split into two parts:

- Upper pyramid (1 star → N stars)

- Lower pyramid (N-2 stars → 1 star)

Each row has spaces first, then stars.

Number of spaces = (N - stars)/2.

**Pseudocode:**  
```text
for each test case:
    read N
    mid = N // 2

    // upper half including middle row
    for i from 0 to mid:
        stars = 2 * i + 1
        spaces = (N - stars) / 2
        print spaces + stars

    // lower half
    for i from mid-1 downto 0:
        stars = 2 * i + 1
        spaces = (N - stars) / 2
        print spaces + stars


```


**Complexity:**  
- Time: O(N²) (you must print N²/2 characters)
- Space:O(1) (apart from output buffer)


### Approach 2:Unified Formula
**Idea:** Instead of splitting into two loops, compute stars directly by i row index.

- For row i (0-based, total rows = N):
    - If i ≤ mid: stars = 2*i + 1
    - Else: stars = 2*(N-i-1) + 1
- Spaces = (N - stars)/2

**Pseudocode:**  
```text
for each test case:
    read N
    mid = N // 2
    for i from 0 to N-1:
        if i <= mid:
            stars = 2*i + 1
        else:
            stars = 2*(N-i-1) + 1
        spaces = (N - stars) / 2
        print spaces + stars


```

**Complexity:**  
- Time:O(n²)
- Space: O(1)  


---

## 6. Justification / Proof of Optimality
- You must print O(N²) characters (≈ N²/2 stars + N²/2 spaces).
- Both approaches accomplish this in O(N²) time and O(1) space.
- Splitting into halves or using a unified formula is equivalent in complexity; the unified formula is cleaner

##  7. Variants / Follow-Ups
- Diamond with hollow center (* only on border).
- Diamond of numbers instead of stars.
- Diamond aligned to left/right instead of centered.
- Print multiple diamonds side by side.

---
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



