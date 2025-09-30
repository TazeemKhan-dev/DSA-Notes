# 1: Pattern 11 — Alternating 1-0 Triangle
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

