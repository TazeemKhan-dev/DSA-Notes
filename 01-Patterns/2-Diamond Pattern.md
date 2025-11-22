# 2:Diamond Pattern
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

