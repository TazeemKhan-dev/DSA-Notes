<!-- #region 1: Pattern 11 — Alternating 1-0 Triangle -->


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
<!-- #endregion -->

<!-- #region 2: Diamond Pattern -->
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
<!-- #endregion -->

<!-- #region 3: Print Number Pattern 3 -->

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

<!-- #endregion -->

<!-- #region 4: Optimus Prime — Print All Primes up to N -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q4: Optimus Prime — Print All Primes up to N</h1>

## 1. Understand the Problem
- **Read & Identify:** Given an integer n, print all prime numbers between 1 and n (inclusive).
- **Goal:** Find all primes ≤ n.
- **Paraphrase:** Numbers greater than 1 that have no divisors other than 1 and themselves.

---

## 2. Input, Output, & Constraints

- **Input:**
```text
8
```

- **Output:**
```text
2 3 5 7
```

**Constraints:**
- 1 ≤ n ≤ 100,000
- Output size ≤ n
- Target time complexity: O(n log log n) with sieve

## 3. Approaches

### Approach 1: Naive Check (Trial Division)

- **Idea:**
  - For every number from 2 to n, check if it’s prime by trying to divide by numbers up to √num.

**Pseudocode:**
```text
function printPrimesNaive(n):
    for i from 2 to n:
        isPrime = true
        for j from 2 to sqrt(i):
            if i % j == 0:
                isPrime = false
                break
        if isPrime:
            print i
```

**Complexity:**
- Time: O(n√n)
- Space: O(1)

### Approach 2: Sieve of Eratosthenes (Optimized)

- **Idea:**
  - Assume all numbers 2..n are prime.
  - Start from 2, mark all its multiples as non-prime.
  - Repeat for next unmarked number.
  - Remaining unmarked numbers are primes.

**Pseudocode:**
```text
function sieve(n):
    isPrime = array[0..n] filled with true
    isPrime[0] = false, isPrime[1] = false
    
    for i from 2 to sqrt(n):
        if isPrime[i]:
            for j from i*i to n step i:
                isPrime[j] = false
    
    for i from 2 to n:
        if isPrime[i]:
            print i
```

**Complexity:**
- Time: O(n log log n)
- Space: O(n)

## 4. Justification / Proof of Optimality

- Naive method is too slow for n up to 10⁵.
- Sieve of Eratosthenes runs in O(n log log n) which is optimal for this range.
- The sieve guarantees correctness by systematically eliminating composites.

## 5. Variants / Follow-Ups

- Print primes between L and R (Segmented Sieve).
- Count primes up to n (Prime Counting Function).
- Find the k-th prime ≤ n.
- Applications in number theory (Goldbach’s conjecture, twin primes).

<!-- #endregion -->

<!-- #region 5: Calculate nCr -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q5: Calculate nCr</h1>

## 1. Understand the Problem
- **Read & Identify:** Given two integers, n (total items) and r (items to choose), the goal is to calculate the binomial coefficient, nCr, which represents the number of distinct combinations.
- **Goal:** Compute the value of nCr using the standard combinatorial formula.
- **Paraphrase:** Find the number of distinct subsets of size r from a set of size n.

---

## 2. Input, Output, & Constraints

- **Input:**
```text
Two non-negative integers, n and r.
```

- **Output:**
```text
A single integer representing the calculated value of nCr.
```

**Constraints:**
- 1≤n≤20
- 1≤r≤n
- The result will fit within a standard 64-bit integer (≈1.8×10 ^19).

## 3. Approaches

### Approach 1: Direct Factorial Calculation (Naive)

- **Idea:**
  - Directly compute the factorials for n, r, and (n−r), then perform the division. nCr=n!/(r!∗(n−r)!)

**Pseudocode:**
```text
function Calculate_nCr(n, r):
    // Requires a data type that can hold 20! (long long/64-bit integer)
    numerator = Factorial(n)
    denominator = Factorial(r) * Factorial(n - r)
    return numerator / denominator
```

**Complexity:**
- Time: O(n)
- Space: O(1) space.

### Approach 2: Optimized Multiplicative Formula (Preferred)

- **Idea:**
  - Simplify the fraction before calculation to minimize the size of intermediate numbers, which is crucial for larger n. The simplified form cancels out the largest factorial term, (n−r)!.

**Pseudocode:**
```text
function Calculate_nCr(n, r):
    // Use nCr = nC(n-r) property for fewer iterations
    if r > n / 2:
        r = n - r

    result = 1
    // Loop r times
    for i from 1 to r:
        // Current calculation: result * (n-i+1) / i
        result = result * (n - i + 1)
        result = result / i // Division is guaranteed to be exact
    return result
```

**Complexity:**
- Time: O(min(r,n−r)), which is O(n)
- Space: O(1) space

## 4. Justification / Proof of Optimality

- Approach 2 is the better solution. While both approaches are O(n) time complexity, Approach 2 keeps the intermediate values much smaller, which minimizes the risk of overflow. It directly computes nCr step-by-step, ensuring each partial product is a valid integer combination value, which is inherently safer than computing three separate, massive factorials (as in Approach 1) and hoping their ratio fits the integer type.

## 5. Variants / Follow-Ups

- Large Constraints (n,r≈10^9): When n and r are very large and the answer must be computed modulo p. This requires number theory techniques like Lucas Theorem or calculating factorials and their modular inverses using Fermat’s Little Theorem.
- Dynamic Programming (Pascal's Identity): For scenarios where many nCr values are needed, the relation nCr=(n−1)C(r−1)+(n−1)Cr allows filling a table (Pascal's Triangle) in O(n ^2) time.
- Permutations (nPr): The problem of calculating nPr=n!/(n−r)! involves a similar multiplicative approach, simply stopping before dividing by r!.
<!-- #endregion -->

<!-- #region 6: Binary To Decimal Conversion -->
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q6: Binary To Decimal Conversion</h1>

## 1. Input, Output, & Constraints

- **Input:**
```text
1011
```

- **Output:**
```text
11
```

**Constraints:**
- Binary number contains only 0 and 1.
- Length of binary number ≤ 32 (or as per system integer limit).

## 2. Approaches

### Approach 1: Positional Value (Iterative)

- **Idea:**
  - Each binary digit represents a power of 2. Starting from the least significant bit (LSB), multiply each bit by 2^position and sum them.

**Pseudocode:**
```text
function binaryToDecimal(binary):
    decimal = 0
    length = len(binary)
    for i in range(0, length):
        bit = int(binary[length - 1 - i])
        decimal += bit * (2^i)
    return decimal
```

**Complexity:**
- Time: O(n), n = number of bits
- Space: O(1)

### Approach 2: Left-to-Right Multiplication (Accumulation)

- **Idea:**
  - Traverse the binary number from left to right, multiply accumulated result by 2, then add current bit.
  - Works for very large binary numbers.
  - Slightly more efficient than computing powers explicitly.

**Pseudocode:**
```text
function binaryToDecimal(binary):
    decimal = 0
    for bit in binary:
        decimal = decimal * 2 + int(bit)
    return decimal
```

**Complexity:**
- Time: O(n)
- Space: O(1)

## 3. Justification / Proof of Optimality

- Optimality: Approach 2 is optimal in terms of simplicity and efficiency.
- Comparison:
- Approach 1: Direct calculation using powers → more verbose.
- Approach 2: Accumulative method → elegant, in-place.

## 4. Variants / Follow-Ups

- Decimal → Binary conversion
- Binary → Hexadecimal conversion
- Large binary strings beyond integer limit → Use BigInt or string manipulation
- Summing multiple binary numbers efficiently

<!-- #endregion -->

<!-- #region 7: Decimal to Binary Conversion -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q7: Decimal to Binary Conversion</h1>

## 1. Input, Output, & Constraints

- **Input:**
```text
11
```

- **Output:**
```text
1011
```

**Constraints:**
- Decimal number ≥ 0
- Decimal number ≤ maximum integer limit of language

## 2. Approaches

### Approach 1: Repeated Division by 2

- **Idea:**
  - Keep dividing the decimal number by 2. The remainder at each step forms the binary digits from least significant bit (LSB) to most significant bit (MSB).

**Pseudocode:**
```text
function decimalToBinary(n):
    if n == 0:
        return "0"
    binary = ""
    while n > 0:
        remainder = n % 2
        binary = str(remainder) + binary
        n = n // 2
    return binary
```

**Complexity:**
- Time: O(log n)
- Space: O(log n) (for storing binary digits)

### Approach 2: Using Bit Manipulation

- **Idea:**
  - Extract bits from decimal number using bitwise AND and right shift operations.

**Pseudocode:**
```text
function decimalToBinary(n):
    if n == 0:
        return "0"
    binary = ""
    while n > 0:
        bit = n & 1
        binary = str(bit) + binary
        n = n >> 1
    return binary
```

**Complexity:**
- Time: O(log n)
- Space: O(log n

## 3. Justification / Proof of Optimality

- Optimality: Approach 1 & 2 are optimal and educational.
- Comparison:
- Approach 1: Classic method using division → easy to understand.
- Approach 2: Bitwise method → faster in low-level operations.

## 4. Variants / Follow-Ups

- Binary → Decimal conversion
- Decimal → Hexadecimal conversion
- Decimal → Binary for negative numbers (2’s complement)
- Fast conversion using recursion or stack

<!-- #endregion -->

<!-- #region 8: Print Continuous Character Pattern -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q8: Print Continuous Character Pattern</h1>

## 1. Input, Output, & Constraints

- **Input:**
```text
5
```

- **Output:**
```text
A
BC
CDE
DEFG
EFGHI
```

## 2. Approaches

### Approach 1: Using ASCII Values

- **Idea:**
  - Use ASCII values of characters. Start from 'A' (ASCII 65), and for each row, print consecutive letters using (ASCII value) % 26 + 65 to handle cyclic behavior.

**Pseudocode:**
```text
function printPattern(n):
    for row in range(1, n+1):
        start_char = 65 + (row - 1)   # 'A' = 65
        for col in range(row):
            char_to_print = chr(65 + ((start_char - 65 + col) % 26))
            print(char_to_print, end="")
        print()   # New line after each row
```

**Complexity:**
- Time: O(n^2) → Each row has up to n letters
- Space: O(1) → Only loop variables

### Approach 2: Using String Arithmetic (Optional)

- **Idea:**
  - Pre-generate the alphabet string "ABCDEFGHIJKLMNOPQRSTUVWXYZ" and use slicing with modulo to handle cyclic letters.

**Pseudocode:**
```text
alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
function printPattern(n):
    for row in range(1, n+1):
        start_index = row - 1
        for col in range(row):
            index = (start_index + col) % 26
            print(alphabet[index], end="")
        print()
```

**Complexity:**
- Time: O(n^2)
- Space: O(1)

## 3. Justification / Proof of Optimality

- Optimality: Both approaches are efficient; Approach 1 is straightforward using ASCII, Approach 2 is more intuitive for beginners.
- Comparison:
- ASCII arithmetic → Less memory, direct computation
- String-based → Easier to read and maintain, especially for cyclic operations

## 4. Variants / Follow-Ups

- Change starting letter for the first row (instead of always 'A')
- Print pattern in reverse order
- Allow lowercase letters or custom alphabet sets
- Print continuous character diamond pattern

<!-- #endregion -->

<!-- #region 9: Pattern 18 – Alphabet Pyramid Ending with ‘E’ -->
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q9: Pattern 18 – Alphabet Pyramid Ending with ‘E’</h1>

## 1. Input, Output, & Constraints

- **Input:**
```text
5
```

- **Output:**
```text
E
D E
C D E
B C D E
A B C D E
```

**Constraints:**
- 1 ≤ n ≤ 26 (English alphabets)

## 2. Approaches

### Approach 1: Using ASCII Values

- **Idea:**
  - 'A' has ASCII value 65.
  - The last letter is 'A' + n - 1.
  - For row i, start printing from (last_letter - i + 1) up to last_letter.

**Pseudocode:**
```text
function printPattern18(n):
    last_char = 65 + n - 1      # ASCII of last letter
    for row in range(1, n+1):
        start_char = last_char - row + 1
        for col in range(start_char, last_char+1):
            print(chr(col), end=" ")
        print()   # new line after each row
```

**Complexity:**
- Time: O(n^2) → Each row prints up to n letters
- Space: O(1) → Only loop variables

### Approach 2: Using String Arithmetic (Optional)

- **Idea:**
  - Pre-generate "ABCDEFGHIJKLMNOPQRSTUVWXYZ" and use slicing.
  - For row i, slice from n-i to n and print letters.

**Pseudocode:**
```text
alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
function printPattern18(n):
    for row in range(1, n+1):
        start_index = n - row
        end_index = n
        for i in range(start_index, end_index):
            print(alphabet[i], end=" ")
        print()
```

**Complexity:**
- Time: O(n^2)
- Space: O(1)

## 3. Justification / Proof of Optimality

- Optimality: ASCII method: direct calculation, no extra memory, simple math.
- String slicing: intuitive and readable, especially for beginners.
- Comparison: Both approaches are O(n²) in time and O(1) in space.
- Use ASCII for efficiency, string for clarity.

## 4. Variants / Follow-Ups

- Change the ending letter to a custom letter
- Reverse the pattern (start at 'A', go up)
- Diagonal or mirrored pyramid patterns
- Use lowercase letters or other character sets

<!-- #endregion -->

<!-- #region 10-Pattern 21 -Hollow Square Pattern -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q10: Pattern 21 -Hollow Square Pattern</h1>
<br>

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


---

## 2. Examples & Edge Cases

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


---

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


---

## 4. Justification / Proof of Optimality

- Optimality: Both approaches are straightforward and efficient for printing a hollow square.
- Comparison:
- Nested loop → Easy to understand for beginners, prints directly
- String concatenation → Slightly more efficient if row strings are reused

---

## 5. Variants / Follow-Ups

- Hollow rectangle (rows ≠ columns)
- Hollow triangle, hollow diamond
- Filled border patterns with different characters
- Hollow square with diagonal * inside

<!-- #endregion -->

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

<!-- #region 12-Count All Digits of a Number -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q12: Count All Digits of a Number</h1>
<br>

## 1. Input, Output, & Constraints

- **Input:**
```text
234
```

- **Output:**
```text
3
```

**Constraints:**
- 0 ≤ n ≤ 5000
- n has no leading zeros except if n = 0


---

## 2. Approaches

### Approach 1: Using Division (Iterative)

- **Idea:**
  - Divide n by 10 repeatedly, counting how many times until n becomes 0.

**Java Code:**
```java
public static int countDigits(int n) {
    if (n == 0) return 1;  // Edge case

    int count = 0;
    while (n > 0) {
        n /= 10;
        count++;
    }
    return count;
}
```

**Complexity:**
- Time: O(log n) → number of digits
- Space: O(1)

### Approach 2: Using String Conversion

- **Idea:**
  - Convert the integer to a string and count the number of characters.

**Java Code:**
```java
public static int countDigits(int n) {
    return String.valueOf(n).length();
}
```

**Complexity:**
- Time: O(log n) → traverses digits to convert to string
- Space: O(log n) → stores string representation

### Approach 3: Using Logarithm (Math.log10)

- **Idea:**
  - The number of digits in a positive integer n is floor(log10(n)) + 1.
  - Edge case: if n = 0, the number of digits is 1.

**Java Code:**
```java
public static int countDigits(int n) {
    if (n == 0) return 1;  // Edge case
    return (int)(Math.log10(n)) + 1;
}
```

**Complexity:**
- Time: O(1) → single mathematical operation
- Space: O(1)


---

## 3. Justification / Proof of Optimality

- Division → simple and memory efficient
- String → concise and intuitive
- Logarithm → fastest for large numbers

---

## 4. Variants / Follow-Ups

- Count digits in negative numbers
- Count digits in very large numbers (BigInteger in Java)
- Count digits in binary, octal, or hexadecimal representation

<!-- #endregion -->

<!-- #region 13- Check for Perfect Number -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q13: Check for Perfect Number</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** Proper divisors = all positive divisors excluding the number itself.  A perfect number is equal to the sum of its proper divisors.

---

## 2. Input, Output, & Constraints

- **Input:**
```text
n
```

- **Output:**
```text
Boolean true or false
```

**Constraints:**
- 1 ≤ n ≤ 5000


---

## 3. Approaches

### Approach 1: Iterating Over Divisors

- **Idea:**
  - Sum all divisors from 1 to n/2 (proper divisors)
  - If sum equals n, return true; else return false

**Java Code:**
```java
public static boolean isPerfectNumber(int n) {
    if (n == 1) return false;  // 1 is not a perfect number

    int sum = 0;
    for (int i = 1; i <= n / 2; i++) {
        if (n % i == 0) {
            sum += i;
        }
    }
    return sum == n;
}
```

**Complexity:**
- Time: O(n) → iterate up to n/2
- Space: O(1)

### Approach 2: Iterating up to √n (Optimized)

- **Idea:**
  - Proper divisors come in pairs (i, n/i)
  - Iterate i from 1 to √n and add both divisors to sum
  - Exclude n itself from the sum

**Java Code:**
```java
public static boolean isPerfectNumber(int n) {
    if (n == 1) return false;  // Edge case

    int sum = 1;  // 1 is always a proper divisor
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            sum += i;
            int pair = n / i;
            if (pair != i) sum += pair;  // Avoid adding sqrt twice
        }
    }
    return sum == n;
}
```

**Complexity:**
- Time: O(√n) → faster for larger numbers
- Space: O(1)


---

## 4. Justification / Proof of Optimality

- Approach 1 is simple and easy to implement.
- Approach 2 is more efficient, especially for larger n, as it avoids unnecessary iterations.

---

## 5. Variants / Follow-Ups

- Check for abundant numbers (sum of divisors > n) or deficient numbers (sum < n)
- Find all perfect numbers up to a given limit
- Handle very large numbers using optimized divisor sum formulas

<!-- #endregion -->

<!-- #region 14-GCD/HCFof Two Numbers   -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q14: GCD/HCFof Two Numbers</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** Find the highest number that both n1 and n2 are divisible by.

---

## 2. Input, Output, & Constraints

- **Input:**
```text
4, 6
```

- **Output:**
```text
Output: 2

Divisors of 4: 1, 2, 4

Divisors of 6: 1, 2, 3, 6

GCD = 2
```

**Constraints:**
- 1 ≤ n1, n2 ≤ 1000


---

## 3. Approaches

### Approach 1: Using Brute Force

- **Idea:**
  - Iterate from min(n1, n2) down to 1
  - First number that divides both is the GCD

**Java Code:**
```java
public static int gcdBruteForce(int n1, int n2) {
    int min = Math.min(n1, n2);
    for (int i = min; i >= 1; i--) {
        if (n1 % i == 0 && n2 % i == 0) {
            return i;
        }
    }
    return 1; // This line is never really reached because 1 always divides
}
```

**Complexity:**
- Time: O(min(n1, n2))
- Space: O(1)

### Approach 2: Using Euclidean Algorithm (Optimized)

- **Idea:**
  - GCD(a, b) = GCD(b, a % b)
  - Repeat until b = 0, then GCD = a

**Java Code:**
```java
public static int gcdEuclidean(int n1, int n2) {
    while (n2 != 0) {
        int temp = n2;
        n2 = n1 % n2;
        n1 = temp;
    }
    return n1;
}
```

**Complexity:**
- Time: O(log(min(n1, n2))) → very efficient
- Space: O(1)


---

## 4. Justification / Proof of Optimality

- Brute force is simple but inefficient for large numbers.
- Euclidean algorithm is optimal, widely used, and handles large inputs efficiently.

---

## 5. Variants / Follow-Ups

- Find LCM using GCD: LCM(a, b) = (a * b) / GCD(a, b)
- Extend to more than two numbers
- Find GCD of an array using pairwise GCD

<!-- #endregion -->

<!-- #region 15-LCM of Two Numbers -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q15: LCM of Two Numbers</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** Find the least number that both n1 and n2 divide evenly.  Can be computed efficiently using GCD: LCM(a, b) = (a * b) / GCD(a, b)

---

## 2. Input, Output, & Constraints

- **Input:**
```text
4, 6
```

- **Output:**
```text
12

Multiples of 4: 4, 8, 12, ...

Multiples of 6: 6, 12, 18, ...

LCM = 12
```


---

## 3. Approaches

### Approach 1: Using Formula LCM = (n1 * n2) / GCD

- **Idea:**
  - Compute GCD first using Euclidean algorithm
  - Then LCM = (n1 * n2) / GCD(n1, n2)

**Java Code:**
```java
public static int lcm(int n1, int n2) {
    int a = n1, b = n2;
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    int gcd = a;
    return (n1 * n2) / gcd;
}
```

**Complexity:**
- Time: O(log(min(n1, n2))) → for computing GCD
- Space: O(1)

### Approach 2: Brute Force Multiples (Less Efficient)

- **Idea:**
  - Start from max(n1, n2) and check each number incrementally until divisible by both

**Java Code:**
```java
public static int lcmBruteForce(int n1, int n2) {
    int lcm = Math.max(n1, n2);
    while (true) {
        if (lcm % n1 == 0 && lcm % n2 == 0) return lcm;
        lcm++;
    }
}
```

**Complexity:**
- Time: O(n1 * n2) → inefficient for large numbers
- Space: O(1)


---

## 4. Justification / Proof of Optimality

- Formula using GCD is efficient and widely used.
- Brute force is simple but slow for larger numbers.

---

## 5. Variants / Follow-Ups

- LCM of more than two numbers (compute pairwise LCM)
- LCM using prime factorization
- LCM of large numbers using BigInteger

<!-- #endregion -->

<!-- #region 16-Rotate Array by K Places -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q16: Rotate Array by K Places</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** For left rotation: [1,2,3,4,5], k=2 → [3,4,5,1,2]  For right rotation: [1,2,3,4,5], k=2 → [4,5,1,2,3]

---

## 2. Input, Output, & Constraints

**Constraints:**
- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- 0 <= k <= 10^5


---

## 3. Examples & Edge Cases

**Example 1 (Left Rotate:):**
Input:
```text
[1,2,3,4,5], k=4
```
Output:
```text
[5,1,2,3,4]
```

**Example 2 (Right Rotate:):**
Input:
```text
[1,2,3,4,5], k=4
```
Output:
```text
[2,3,4,5,1]
```


---

## 4. Approaches

### Approach 1: Using Extra Array (Simple)

- **Idea:**
  - For left rotate: Create a new array, place nums[i] at (i - k + n) % n
  - For right rotate: Place nums[i] at (i + k) % n

**Java Code:**
```java
// Left Rotate
public static void leftRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    int[] res = new int[n];
    for (int i = 0; i < n; i++) {
        res[(i - k + n) % n] = nums[i];
    }
    for (int i = 0; i < n; i++) nums[i] = res[i];
}

// Right Rotate
public static void rightRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    int[] res = new int[n];
    for (int i = 0; i < n; i++) {
        res[(i + k) % n] = nums[i];
    }
    for (int i = 0; i < n; i++) nums[i] = res[i];
}
```

**Complexity:**
- Time: O(n)
- Space: O(n) → extra array

### Approach 2: Using Reverse (Optimal, In-Place)

- **Idea:**
  - Reverse parts of the array to rotate in-place.
  - Left Rotate by k:
  - Reverse first k elements
  - Reverse remaining n-k elements
  - Reverse the whole array
  - Right Rotate by k:
  - Reverse last k elements
  - Reverse first n-k elements
  - Reverse the whole array

**Java Code:**
```java
// Helper to reverse a subarray
private static void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}

// Left Rotate
public static void leftRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    reverse(nums, 0, k - 1);
    reverse(nums, k, n - 1);
    reverse(nums, 0, n - 1);
}

// Right Rotate
public static void rightRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    reverse(nums, n - k, n - 1);
    reverse(nums, 0, n - k - 1);
    reverse(nums, 0, n - 1);
}
```

**Complexity:**
- Time: O(n)
- Space: O(1) → in-place


---

## 5. Justification / Proof of Optimality

- Extra array method is intuitive but uses O(n) extra space.
- Reverse method is optimal, in-place, and widely used in interviews.
- Correctly handles k >= n using modulo.

---

## 6. Variants / Follow-Ups

- Rotate array by one position repeatedly
- Rotate multi-dimensional arrays
- Rotate linked list by k positions
- Rotate circular arrays or arrays with constraints

<!-- #endregion -->

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

<!-- #region 18- Array Adding -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q18: Array Adding</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given two arrays, arr1 and arr2, each representing the digits of a number. The arrays may have different sizes.
- **Goal:** Add the two numbers represented by the arrays and return the result as an array of digits.
- **Paraphrase:** Treat arrays as numbers: most significant digit at index 0.  Perform addition like elementary school addition, handling carry.

---

## 2. Constraints

**Constraints:**
- 0 <= arr1[i], arr2[i] < 10
- Arrays may have different lengths


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
2
2 1
4
1 2 3 4
```
Output:
```text
1 2 5 5
```


---

## 4. Approaches

### Approach 1: Simulate Addition from End

- **Idea:**
  - Start from the last index of both arrays
  - Add corresponding digits and carry
  - Store result in a list or array
  - Reverse the result at the end

**Java Code:**
```java
public static int[] addArrays(int[] arr1, int[] arr2) {
    int n1 = arr1.length, n2 = arr2.length;
    int i = n1 - 1, j = n2 - 1;
    int carry = 0;
    java.util.ArrayList<Integer> resList = new java.util.ArrayList<>();

    while (i >= 0 || j >= 0 || carry != 0) {
        int sum = carry;
        if (i >= 0) sum += arr1[i--];
        if (j >= 0) sum += arr2[j--];
        resList.add(sum % 10);
        carry = sum / 10;
    }

    // Reverse the result
    int[] result = new int[resList.size()];
    for (int k = 0; k < resList.size(); k++) {
        result[k] = resList.get(resList.size() - 1 - k);
    }
    return result;
}
```

**Complexity:**
- Time: O(max(n1, n2)) → each digit processed once
- Space: O(max(n1, n2) + 1) → for result

### Approach 2: Convert Arrays to Numbers Using long (Limited)

- **Idea:**
  - Convert arrays to numbers using long
  - Add them
  - Convert the sum back to array

**Java Code:**
```java
static int[] calSum(int a[], int b[], int n, int m) {
    // your code here
    long n1=0,n2=0,res=0;
    for(int i=0;i<n;i++){
        n1=n1*10+(long)a[i];
    }
    for(int i=0;i<m;i++){
        n2=n2*10+(long)b[i];
    }
    res=n1+n2;

    int s=0;
    long res1=0;
    while(res>0){
      res1=res1*10+res%10;
      res/=10;
      s++;
    }
    int [] z =new int[s];
    for(int i=0;i<s;i++){
      z[i]=(int)(res1%10);
      res1/=10;
    }
    return z;

  }
Limitations:

Fails for large arrays (overflow of long)

Works only if number fits in long
```

**Complexity:**
- Time: O(n+m+max(n,m)+max(n,m))=O(n+m)
- Space: res1 and other long/int variables → O(1)  Result array z[] of size s ≈ max(n, m) + 1 → O(max(n, m))  ✅ Overall Space Complexity:  𝑂 ( max ⁡ ( 𝑛 , 𝑚 ) ) O(max(n,m))


---

## 5. Justification / Proof of Optimality

- Approach 1 is optimal and works for arrays of any size.
- Approach 2 is simpler but limited by long size.
- Both approaches correctly handle carry and array lengths.

---

## 6. Variants / Follow-Ups

- Subtract two numbers represented by arrays
- Multiply two numbers represented by arrays
- Add multiple arrays of digits
- Handle arrays in reverse order (least significant digit first)

<!-- #endregion -->

<!-- #region 19-Array Subtracting -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q19: Array Subtracting</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given two arrays, arr1 and arr2, representing digits of two numbers. The arrays may have different sizes.
- **Goal:** Subtract the number represented by arr2 from the number represented by arr1 (arr1 - arr2) and return the result as an array of digits.
- **Paraphrase:** Perform subtraction digit by digit from the end (least significant digit).  Handle borrow if needed.  Return the resulting number as an array.

---

## 2. Constraints

- 1 <= n1, n2 <= 10
- 0 <= arr1[i], arr2[i] < 10
- arr1 represents a number greater than or equal to arr2


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
1 2 3 4
2
2 1
```
Output:
```text
1 2 1 3
```


---

## 4. Approaches

### Approach 1: Digit-by-Digit Subtraction

- **Idea:**
  - Start from the last index of both arrays (least significant digit).
  - Subtract corresponding digits, handling borrow.
  - Store result in a list, reverse at the end, and remove leading zeros.

**Java Code:**
```java
public static int[] subtractArrays(int[] arr1, int[] arr2) {
    int i = arr1.length - 1;
    int j = arr2.length - 1;
    int borrow = 0;
    java.util.ArrayList<Integer> resList = new java.util.ArrayList<>();

    while (i >= 0) {
        int diff = arr1[i] - borrow;
        if (j >= 0) diff -= arr2[j--];

        if (diff < 0) {
            diff += 10;
            borrow = 1;
        } else {
            borrow = 0;
        }

        resList.add(diff);
        i--;
    }

    // Remove leading zeros
    while (resList.size() > 1 && resList.get(resList.size() - 1) == 0) {
        resList.remove(resList.size() - 1);
    }

    // Reverse to correct order
    int[] result = new int[resList.size()];
    for (int k = 0; k < resList.size(); k++) {
        result[k] = resList.get(resList.size() - 1 - k);
    }
    return result;
}
```

**Complexity:**
- Time: O(n1) → each digit processed once
- Space: O(n1) → for result

### Approach 2: Convert Arrays to Numbers (Limited)

- **Idea:**
  - Convert both arrays to numbers
  - Subtract numbers
  - Convert result back to array
  - Limitation:
  - Works only if numbers fit in int or long.
  - Not recommended for large arrays.


---

## 5. Justification / Proof of Optimality

- Optimal approach: digit-by-digit subtraction works for any array size.
- Correctly handles borrow and leading zeros.
- Approach 2 is simple but limited by numeric type size.

---

## 6. Variants / Follow-Ups

- Subtract multiple numbers represented by arrays
- Subtract numbers in reverse order arrays
- Implement array multiplication or division similarly
- Handle negative results if arr2 > arr1

<!-- #endregion -->

<!-- #region 20-Maximum Difference Between Two Elements -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q20: Maximum Difference Between Two Elements</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an array of positive integers. We need to find the maximum absolute difference between any two elements in the array.
- **Goal:** Find max(|arr[i] - arr[j]|) for all i, j pairs.
- **Paraphrase:** Maximum difference occurs between the largest and smallest element in the array.  There’s no need to check every pair individually.

---

## 2. Constraints

- 2 <= n <= 10^6
- 0 < arr[i] <= 10^9
- Elements are positive integers


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
16 24 89 35
```
Output:
```text
73
Explanation: max(89-16) = 73
```


---

## 4. Approaches

### Approach 1: Optimal Approach (Single Scan)

- **Idea:**
  - Maximum difference is max(arr) - min(arr)
  - Scan array once to find min and max

**Java Code:**
```java
public static long maxDifference(int[] arr) {
    int n = arr.length;
    int minVal = arr[0], maxVal = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] < minVal) minVal = arr[i];
        if (arr[i] > maxVal) maxVal = arr[i];
    }
    return (long)maxVal - (long)minVal;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant extra space

### Approach 2: Brute Force (Check All Pairs)

- **Idea:**
  - Iterate over all pairs (i, j) and compute |arr[i]-arr[j]|
  - Track the maximum

**Complexity:**
- Time: O(n^2) → not feasible for large n
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approach is clearly better: O(n) time, O(1) space, handles large arrays.
- Brute force works for small arrays only and is inefficient.

---

## 6. Variants / Follow-Ups

- Maximum difference with constraint i < j
- Maximum difference after sorting the array
- Maximum difference for subarrays
- Maximum difference modulo some number

<!-- #endregion -->

<!-- #region 21-Shortest Distance Between Two Even Positive Integers -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q21: Shortest Distance Between Two Even Positive Integers</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an array of integers. We need to find the shortest distance between any two even positive integers.
- **Goal:** Find two even positive integers arr[i] and arr[j] with minimum |i - j|.  Return -1 if there is zero or one even positive integer.
- **Paraphrase:** Iterate through the array, track positions of even positive numbers.  Calculate distance to the previous even positive number.  Keep the minimum distance.

---

## 2. Constraints

- 2 <= n <= 10^6
- 0 < arr[i] <= 10^9
- Elements are positive integers


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
2
1 2
```
Output:
```text
-1
Explanation: Only 1 even positive number.
```

**Example 2 (Normal Case):**
Input:
```text
5
2 4 1 6 7
```
Output:
```text
1
Explanation:

Distance(2,4) = 1

Distance(2,6) = 3

Distance(4,6) = 2

Shortest = 1
```


---

## 4. Approaches

### Approach 1: Single Scan (Optimal)

- **Idea:**
  - Keep track of the index of the previous even positive integer.
  - For each even positive integer, compute distance to previous and update minimum.

**Java Code:**
```java
public static int shortestEvenDistance(int[] arr) {
    int prevIndex = -1;
    int minDist = Integer.MAX_VALUE;
    
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] > 0 && arr[i] % 2 == 0) { // even positive
            if (prevIndex != -1) {
                minDist = Math.min(minDist, i - prevIndex);
            }
            prevIndex = i;
        }
    }
    
    return (minDist == Integer.MAX_VALUE) ? -1 : minDist;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → only index and minDist

### Approach 2: Store All Even Indices (Not Needed)

- **Idea:**
  - Store indices of all even positive numbers in a list
  - Iterate through list to compute distances
  - Limitation:
  - Extra space O(k) where k = number of even positive numbers
  - Slower in practice for very large n


---

## 5. Justification / Proof of Optimality

- Optimal approach uses single pass and O(1) space.
- Handles all edge cases: no even numbers, single even number, consecutive even numbers.

---

## 6. Variants / Follow-Ups

- Shortest distance between odd positive integers
- Shortest distance for all positive numbers divisible by k
- Shortest distance with array in circular form
- Find all pairs with minimum distance instead of just distance

<!-- #endregion -->

<!-- #region 22-Sum of Array Except Self -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q22: Sum of Array Except Self</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an array of integers. We need to return a new array such that each element is the sum of all other elements except itself.
- **Goal:** For each index i, calculate sum(arr) - arr[i] and store in result array.
- **Paraphrase:** Compute total sum of array once.  Subtract arr[i] from total sum for each index to get output[i].  Avoid nested loops for efficiency.

---

## 2. Constraints

- 1 <= n <= 5000
- 1 <= arr[i] <= 1000000


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
4 3 2 10
```
Output:
```text
15 16 17 9
Explanation:

totalSum = 4 + 3 + 2 + 10 = 19

output[0] = 19 - 4 = 15

output[1] = 19 - 3 = 16

output[2] = 19 - 2 = 17

output[3] = 19 - 10 = 9
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - For each element, iterate through the array and sum all other elements.

**Java Code:**
```java
public static long[] sumExceptSelfBrute(int[] arr) {
    int n = arr.length;
    long[] result = new long[n];

    for (int i = 0; i < n; i++) {
        long sum = 0;
        for (int j = 0; j < n; j++) {
            if (i != j) sum += arr[j];
        }
        result[i] = sum;
    }
    return result;
}
```

**Complexity:**
- Time: O(n^2) → nested loops
- Space: O(n) → result array

### Approach 2: Optimal (Total Sum Subtraction)

- **Idea:**
  - Compute total sum of array.
  - For each index, subtract current element from total sum to get output.

**Java Code:**
```java
public static long[] sumExceptSelfOptimal(int[] arr) {
    int n = arr.length;
    long totalSum = 0;
    for (int i = 0; i < n; i++) {
        totalSum += arr[i];
    }

    long[] result = new long[n];
    for (int i = 0; i < n; i++) {
        result[i] = totalSum - arr[i];
    }
    return result;
}
```

**Complexity:**
- Time: O(n) → two passes
- Space: O(n) → result array


---

## 5. Justification / Proof of Optimality

- Optimal approach is better for large n and avoids unnecessary nested loops.
- Brute force is intuitive but inefficient.
- Both approaches handle positive integers correctly.

---

## 6. Variants / Follow-Ups

- Product of array except self
- Sum except self modulo k
- Arrays containing negative integers
- Sparse arrays with zeros

<!-- #endregion -->

<!-- #region 23-Largest Number At Least Twice of Others -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q23: Largest Number At Least Twice of Others</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array nums of size n. The largest integer is unique.
- **Goal:** Determine whether the largest number is at least twice as large as every other number in the array.  If yes, return the index of the largest number; else return -1.
- **Paraphrase:** Find the largest element and compare it with all other elements.  Check if it is ≥ 2 × any other element.

---

## 2. Constraints

- 1 <= n <= 50
- 0 <= nums[i] <= 100
- Largest number in array is unique


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
3 6 1 0
```
Output:
```text
1
Explanation: 6 ≥ 2 × 3, 6 ≥ 2 × 1, 6 ≥ 2 × 0 → condition satisfied
```

**Example 2 (Normal Case):**
Input:
```text
4
1 2 3 4
```
Output:
```text
-1
Explanation: 4 < 2 × 3 → condition fails
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Find the largest element and its index.
  - Iterate through the array and check if largest >= 2 × arr[i] for all other elements.

**Java Code:**
```java
public static int largestAtLeastTwiceBrute(int[] nums) {
    int n = nums.length;
    int maxVal = nums[0];
    int maxIndex = 0;

    // Find largest element
    for (int i = 1; i < n; i++) {
        if (nums[i] > maxVal) {
            maxVal = nums[i];
            maxIndex = i;
        }
    }

    // Check condition
    for (int i = 0; i < n; i++) {
        if (i != maxIndex && maxVal < 2 * nums[i]) {
            return -1;
        }
    }

    return maxIndex;
}
```

**Complexity:**
- Time: O(n) → two passes over array
- Space: O(1) → constant space

### Approach 2: Optimal (Single Pass)

- **Idea:**
  - Track both the largest and second largest elements in a single pass.
  - Compare largest with 2 × second largest.

**Java Code:**
```java
public static int largestAtLeastTwiceOptimal(int[] nums) {
    int n = nums.length;
    int max1 = -1, max2 = -1, maxIndex = -1;

    for (int i = 0; i < n; i++) {
        if (nums[i] > max1) {
            max2 = max1;
            max1 = nums[i];
            maxIndex = i;
        } else if (nums[i] > max2) {
            max2 = nums[i];
        }
    }

    return (max1 >= 2 * max2) ? maxIndex : -1;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant space


---

## 5. Justification / Proof of Optimality

- Brute force is simple, intuitive, and works well for small n (≤50).
- Optimal approach reduces the comparison step by tracking the second largest element → still O(n) but slightly faster and elegant.

---

## 6. Variants / Follow-Ups

- Largest number at least k times others
- Largest number at least twice for subarrays
- Return all numbers that satisfy condition instead of just largest
- Extend to non-unique largest elements

<!-- #endregion -->

<!-- #region 24-Subarray Sum Divisible by k -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q24: Subarray Sum Divisible by k</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array nums and an integer k. We need to find the number of non-empty contiguous subarrays whose sum is divisible by k.
- **Goal:** Count all subarrays nums[i..j] such that (sum(nums[i..j]) % k) == 0.
- **Paraphrase:** Check every possible subarray for divisibility by k.  Optimize using prefix sum and modulo properties.

---

## 2. Constraints

- 1 <= n <= 5000
- -10^4 <= nums[i] <= 10^4
- 2 <= k <= 10^4


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
6 5
4 5 0 -2 -3 1
```
Output:
```text
7
Explanation:

Subarrays divisible by 5: [4,5,0,-2,-3,1], [5], [5,0], [5,0,-2,-3], [0], [0,-2,-3], [-2,-3]
```

**Example 2 (Normal Case):**
Input:
```text
4 2
4 5 0 -2
```
Output:
```text
4
Explanation:

Subarrays divisible by 2: [4], [0], [0,-2], [-2]

```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Iterate over all subarrays (i..j)
  - Compute sum and check divisibility by k

**Java Code:**
```java
public static int subarraySumDivisibleByKBrute(int[] nums, int k) {
    int n = nums.length;
    int count = 0;
    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += nums[j];
            if (sum % k == 0) count++;
        }
    }
    return count;
}
```

**Complexity:**
- Time: O(n^2) → nested loops
- Space: O(1) → constant space

### Approach 2: Optimal (Prefix Sum + HashMap)

- **Idea:**
  - Use prefix sum and modulo property: (sum[j] - sum[i-1]) % k == 0 → sum[j] % k == sum[i-1] % k
  - Track frequency of modulo values in a hashmap
  - Count pairs with same modulo

**Java Code:**
```java
import java.util.*;

public static int subarraySumDivisibleByKOptimal(int[] nums, int k) {
    Map<Integer, Integer> modCount = new HashMap<>();
    modCount.put(0, 1); // empty prefix
    int sum = 0;
    int count = 0;

    for (int num : nums) {
        sum += num;
        int mod = ((sum % k) + k) % k; // handle negative numbers
        count += modCount.getOrDefault(mod, 0);
        modCount.put(mod, modCount.getOrDefault(mod, 0) + 1);
    }

    return count;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(k) → store modulo frequencies


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n²) → inefficient for large n.
- Optimal approach leverages prefix sum + modulo properties → O(n) time.
- Correctly handles negative numbers and large arrays.

---

## 6. Variants / Follow-Ups

- Subarray sum divisible by any number k
- Count subarrays with sum divisible by k and of length ≥ m
- Count continuous subarrays with sum exactly equal to k

<!-- #endregion -->

<!-- #region 25-Find Geometric Triplets -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q25: Find Geometric Triplets</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given a sorted array of integers. We need to find all triplets (a, b, c) such that they form a geometric progression (GP).
- **Goal:** Print all triplets (arr[i], arr[j], arr[k]) where arr[j]/arr[i] == arr[k]/arr[j] → common ratio same.  Output triplets in lexicographic order (sorted by first, then second, then third element).
- **Paraphrase:** For every possible triplet in the array, check if it forms a geometric progression.  Print them sorted automatically since the array is already sorted.

---

## 2. Constraints

- 1 <= N <= 10
- Array elements are positive integers


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
6
1 2 6 10 18 54
```
Output:
```text
2 6 18
6 18 54
```

**Example 2 (Normal Case):**
Input:
```text
8
2 8 10 15 16 30 32 64
```
Output:
```text
2 8 32
8 16 32
16 32 64
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Iterate all triplets (i < j < k) using 3 nested loops
  - Check if arr[j]^2 == arr[i] * arr[k] → avoids floating point division
  - Print valid triplets

**Java Code:**
```java
public static void findGPTripletsBrute(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 2; i++) {
        for (int j = i + 1; j < n - 1; j++) {
            for (int k = j + 1; k < n; k++) {
                if ((long)arr[j] * arr[j] == (long)arr[i] * arr[k]) {
                    System.out.println(arr[i] + " " + arr[j] + " " + arr[k]);
                }
            }
        }
    }
}
```

**Complexity:**
- Time: O(n³) → three nested loops
- Space: O(1) → no extra space

### Approach 2: Optimal (Using Hashing / Map)

- **Idea:**
  - Use a HashSet for fast lookup
  - For each pair (i, j) as first two elements, compute expectedThird = arr[j]*arr[j]/arr[i]
  - Check if expectedThird exists in the array using HashSet

**Java Code:**
```java
import java.util.*;

public static void findGPTripletsOptimal(int[] arr) {
    int n = arr.length;
    Set<Integer> set = new HashSet<>();
    for (int num : arr) set.add(num);

    for (int i = 0; i < n - 2; i++) {
        for (int j = i + 1; j < n - 1; j++) {
            if ((arr[j] * arr[j]) % arr[i] == 0) { // ensure integer division
                int expectedThird = (arr[j] * arr[j]) / arr[i];
                if (set.contains(expectedThird) && expectedThird > arr[j]) {
                    System.out.println(arr[i] + " " + arr[j] + " " + expectedThird);
                }
            }
        }
    }
}
```

**Complexity:**
- Time: O(n²) → iterate all pairs (i, j)
- Space: O(n) → for HashSet lookup


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n³) → acceptable since n <= 10.
- Optimal approach reduces complexity using a HashSet lookup for third element → O(n²).
- Lexicographic order is naturally satisfied because the array is already sorted.

---

## 6. Variants / Follow-Ups

- Count number of GP triplets instead of printing
- Triplets for negative numbers or zeros
- Triplets with common ratio = r given, instead of any ratio
- Longest GP subsequence instead of triplets

<!-- #endregion -->

<!-- #region 26-Maximum Sum Subarray -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q26: Maximum Sum Subarray</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array arr. We need to find the maximum sum of any contiguous subarray.
- **Goal:** Return the sum of the subarray that has the largest sum among all contiguous subarrays.
- **Paraphrase:** Find subarray [i..j] with sum arr[i]+arr[i+1]+...+arr[j] maximized.

---

## 2. Constraints

- 1 <= n <= 10^4
- -100 <= arr[i] <= 100


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
5
2 3 1 -1 0
```
Output:
```text
6
Explanation: Maximum subarray sum = 2 + 3 + 1 = 6
```

**Example 2 (Normal Case):**
Input:
```text
8
-2 -3 4 -1 -2 1 5 -3
```
Output:
```text
7
Explanation: Maximum subarray sum = 4 + -1 + -2 + 1 + 5 = 7
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Check all possible subarrays
  - Calculate sum for each subarray
  - Track maximum sum

**Java Code:**
```java
public static int maxSubarraySumBrute(int[] arr) {
    int n = arr.length;
    int maxSum = Integer.MIN_VALUE;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum > maxSum) maxSum = sum;
        }
    }

    return maxSum;
}
```

**Complexity:**
- Time: O(n²) → nested loops
- Space: O(1) → constant space

### Approach 2: Optimal (Kadane’s Algorithm)

- **Idea:**
  - Maintain a running sum (currentSum)
  - If currentSum < 0, reset to 0
  - Track maximum sum seen so far

**Java Code:**
```java
public static int maxSubarraySumOptimal(int[] arr) {
    int maxSum = arr[0];
    int currentSum = arr[0];

    for (int i = 1; i < arr.length; i++) {
        currentSum = Math.max(arr[i], currentSum + arr[i]);
        maxSum = Math.max(maxSum, currentSum);
    }

    return maxSum;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant space

### Approach 3: 2b: Optimal with Long (Handling Overflow)

- **Idea:**
  - Use long for sum and maxi to avoid overflow with large integers
  - Same logic as Kadane’s algorithm: maintain running sum, reset if negative

**Java Code:**
```java
public static int maxSubarraySumOptimalLong(int[] nums) {
    long maxi = Long.MIN_VALUE;
    long sum = 0;

    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];   // safe sum using long
        if (sum > maxi) maxi = sum;

        if (sum < 0) sum = 0;
    }

    return (int) maxi;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant space, Safe for large integers → avoids int overflow


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n²) → inefficient for large arrays
- Kadane’s algorithm works in O(n) and handles negative numbers correctly
- Correctly identifies subarray with maximum sum without storing all subarrays

---

## 6. Variants / Follow-Ups

- Maximum product subarray
- Maximum sum subarray of length exactly k
- Maximum sum subarray in circular array
- Return start and end indices of maximum subarray

<!-- #endregion -->

<!-- #region 27-Divisible Sum Pairs -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q27: Divisible Sum Pairs</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array arr and a positive integer k. We need to find all pairs (i, j) with i < j such that (arr[i] + arr[j]) % k == 0.
- **Goal:** Count all valid pairs where the sum is divisible by k.
- **Paraphrase:** Iterate through all possible pairs (i,j) with i < j.  Check divisibility and count the pairs.  Optimize using modulo frequencies.

---

## 2. Constraints

- 1 <= n <= 10^3
- 1 <= arr[i] <= 10^6
- 1 <= k <= 10^6


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
6 3
1 3 2 6 1 2
```
Output:
```text
5
Explanation:
Valid pairs: (0,2),(0,5),(1,3),(2,4),(4,5)
```

**Example 2 (Normal Case):**
Input:
```text
4 5
1 3 2 6
```
Output:
```text
1
Explanation:
Valid pair: (1,2)
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Iterate all pairs (i,j) with i<j
  - Check (arr[i]+arr[j]) % k == 0
  - Count valid pairs

**Java Code:**
```java
public static int divisibleSumPairsBrute(int[] arr, int k) {
    int n = arr.length;
    int count = 0;
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if ((arr[i] + arr[j]) % k == 0) count++;
        }
    }
    return count;
}
```

**Complexity:**
- Time: O(n²) → nested loops
- Space: O(1) → constant

### Approach 2: Optimal (Using Modulo Frequency)

- **Idea:**
  - Use frequency array of mod k values
  - For each element, compute mod = arr[i] % k
  - Number of valid pairs = frequency of (k - mod) % k seen so far

**Java Code:**
```java
public static int divisibleSumPairsOptimal(int[] arr, int k) {
    int[] freq = new int[k];
    int count = 0;

    for (int num : arr) {
        int mod = num % k;
        int complement = (k - mod) % k; // pair sum divisible by k
        count += freq[complement];
        freq[mod]++;
    }

    return count;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(k) → store modulo frequencies


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n²) → acceptable only for small n
- Optimal approach leverages modulo complement counting → O(n) and safe for larger arrays
- Correctly counts all (i,j) with i < j without checking all pairs explicitly

---

## 6. Variants / Follow-Ups

- Count pairs with sum divisible by any number k
- Find all actual pairs instead of count
- Consider triplets instead of pairs
- Allow negative numbers and handle modulo correctly

<!-- #endregion -->

<!-- #region 28-Transpose of Matrix -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q28: Transpose of Matrix</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given a square matrix of size N x N. We need to transpose it, i.e., switch rows and columns.
- **Goal:** Convert matrix[i][j] to matrix[j][i].  Do it in-place without using extra space.
- **Paraphrase:** Swap elements above the diagonal with the corresponding elements below the diagonal.

---

## 2. Constraints

- 1 <= N <= 100
- -10^3 <= mat[i][j] <= 10^3


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
1 1 1 1
2 2 2 2
3 3 3 3
4 4 4 4
```
Output:
```text
1 2 3 4
1 2 3 4
1 2 3 4
1 2 3 4
```


---

## 4. Approaches

### Approach 1: Brute Force (Using Extra Matrix)

- **Idea:**
  - Create a new matrix transpose[N][N]
  - Set transpose[j][i] = matrix[i][j]
  - Print the new matrix

**Java Code:**
```java
public static void transposeMatrixBrute(int[][] mat) {
    int N = mat.length;
    int[][] trans = new int[N][N];
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            trans[j][i] = mat[i][j];
        }
    }

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            System.out.print(trans[i][j] + " ");
        }
        System.out.println();
    }
}
```

**Complexity:**
- Time: O(N²)
- Space: O(N²)

### Approach 2: Optimal (In-Place Swap)

- **Idea:**
  - Only swap elements above the diagonal with elements below the diagonal
  - For all i < j, swap mat[i][j] with mat[j][i]

**Java Code:**
```java
public static void transposeMatrixOptimal(int[][] mat) {
    int N = mat.length;

    for (int i = 0; i < N; i++) {
        for (int j = i + 1; j < N; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[j][i];
            mat[j][i] = temp;
        }
    }

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            System.out.print(mat[i][j] + " ");
        }
        System.out.println();
    }
}
```

**Complexity:**
- Time: O(N²) → every element visited once
- Space: O(1) → in-place


---

## 5. Justification / Proof of Optimality

- Brute force works but uses extra space
- Optimal approach modifies the matrix in-place → meets the expected space complexity
- Swapping above diagonal with below diagonal ensures all elements are transposed

---

## 6. Variants / Follow-Ups

- Transpose non-square matrix → requires extra matrix
- Rotate matrix 90°, 180° → transpose + reverse operations
- Compute transpose without changing original matrix
- Use in algorithms like matrix multiplication optimization

<!-- #endregion -->

<!-- #region 29-Rotate a Matrix by 90° (Clockwise & Anti-Clockwise) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q29: Rotate a Matrix by 90° (Clockwise & Anti-Clockwise)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an n x n square matrix. We need to rotate it 90° clockwise or 90° anti-clockwise.
- **Goal:** Clockwise: top-left → top-right → bottom-right → bottom-left  Anti-clockwise: top-left → bottom-left → bottom-right → top-right
- **Paraphrase:** Rotate the matrix in-place using minimal extra space, ideally O(1).

---

## 2. Constraints

- 1 <= n <= 100
- In-place solution required


---

## 3. Examples & Edge Cases

**Example 1 (Clockwise 90° Rotation:):**
Input:
```text
3 3
7 2 3
2 3 4
5 6 1
```
Output:
```text
5 2 7
6 3 2
1 4 3
```

**Example 2 (Anti-Clockwise 90° Rotation:):**
Input:
```text
3 3
7 2 3
2 3 4
5 6 1
```
Output:
```text
3 4 1
2 3 6
7 2 5
```


---

## 4. Approaches

### Approach 1: Brute Force (Using Extra Matrix)

- **Idea:**
  - Clockwise: rotated[j][n-1-i] = matrix[i][j]
  - Anti-clockwise: rotated[n-1-j][i] = matrix[i][j]

**Java Code:**
```java
// Clockwise
public static int[][] rotateMatrixClockwiseBrute(int[][] mat) {
    int n = mat.length;
    int[][] rotated = new int[n][n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            rotated[j][n-1-i] = mat[i][j];
        }
    }
    return rotated;
}

// Anti-Clockwise
public static int[][] rotateMatrixAntiClockwiseBrute(int[][] mat) {
    int n = mat.length;
    int[][] rotated = new int[n][n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            rotated[n-1-j][i] = mat[i][j];
        }
    }
    return rotated;
}
```

**Complexity:**
- Time: O(N²)
- Space: O(n²) → extra matrix

### Approach 2: Optimal In-Place (Transpose + Reverse)

- **Idea:**
  - Idea (Clockwise):
  - Transpose matrix in-place
  - Reverse each row
  - Idea (Anti-Clockwise):
  - Transpose matrix in-place
  - Reverse each column

**Java Code:**
```java
// Clockwise 90°
public static void rotateMatrixClockwise(int[][] mat) {
    int n = mat.length;
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[j][i];
            mat[j][i] = temp;
        }
    }
    // Reverse rows
    for (int i = 0; i < n; i++) {
        int left = 0, right = n-1;
        while (left < right) {
            int temp = mat[i][left];
            mat[i][left] = mat[i][right];
            mat[i][right] = temp;
            left++; right--;
        }
    }
}

// Anti-Clockwise 90°
public static void rotateMatrixAntiClockwise(int[][] mat) {
    int n = mat.length;
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[j][i];
            mat[j][i] = temp;
        }
    }
    // Reverse columns
    for (int j = 0; j < n; j++) {
        int top = 0, bottom = n-1;
        while (top < bottom) {
            int temp = mat[top][j];
            mat[top][j] = mat[bottom][j];
            mat[bottom][j] = temp;
            top++; bottom--;
        }
    }
}
```

**Complexity:**
- Time: O(n²) → transpose + reverse
- Space: O(1) → in-place


---

## 5. Justification / Proof of Optimality

- Brute force is simple but uses extra space
- Optimal approach rotates in-place → meets constraints
- Works for any square matrix, handles maximum n efficiently

---

## 6. Variants / Follow-Ups

- Rotate 180° → two 90° rotations
- Rotate by arbitrary angle (multiple of 90°)
- Rotate non-square matrix → requires extra matrix
- Combine rotation with matrix reflection operations

<!-- #endregion -->

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

<!-- #region 31-Spirally Traversing a Matrix (Clockwise & Anti-clockwise) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q31: Spirally Traversing a Matrix (Clockwise & Anti-clockwise)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** You are given a matrix of size m x n.  Task is to print all elements in spiral order:  Clockwise → top row → right column → bottom row → left column → shrink boundaries, repeat.  Anti-clockwise → left column → bottom row → right column → top row → shrink boundaries, repeat.
- **Goal:** Print traversal order of the matrix elements.
- **Paraphrase:** Keep four boundaries (top, bottom, left, right) and traverse them layer by layer until all elements are covered.

---

## 2. Constraints

- 1 <= m, n <= 100
- -10^3 <= mat[i][j] <= 10^3


---

## 3. Examples & Edge Cases

**Example 1 (Clockwise):**
Input:
```text
3 3
1 2 3
4 5 6
7 8 9
```
Output:
```text
1 2 3 6 9 8 7 4 5
```

**Example 2 (Anti-clockwise):**
Input:
```text
3 3
1 2 3
4 5 6
7 8 9
```
Output:
```text
1 4 7 8 9 6 3 2 5
```


---

## 4. Approaches

### Approach 1: Brute Force (Simulate Layer-by-Layer)

- **Idea:**
  - Maintain top, bottom, left, right indices.
  - Clockwise order: top → right → bottom → left.
  - Anti-clockwise order: left → bottom → right → top.
  - After traversing a boundary, shrink it (top++, bottom--, left++, right--).

**Java Code:**
```java
Java Code (Clockwise Spiral Traversal)

public static void spiralClockwise(int[][] mat) {
    int m = mat.length, n = mat[0].length;
    int top = 0, bottom = m - 1, left = 0, right = n - 1;

    while (top <= bottom && left <= right) {
        // Traverse top row
        for (int j = left; j <= right; j++)
            System.out.print(mat[top][j] + " ");
        top++;

        // Traverse right column
        for (int i = top; i <= bottom; i++)
            System.out.print(mat[i][right] + " ");
        right--;

        // Traverse bottom row
        if (top <= bottom) {
            for (int j = right; j >= left; j--)
                System.out.print(mat[bottom][j] + " ");
            bottom--;
        }

        // Traverse left column
        if (left <= right) {
            for (int i = bottom; i >= top; i--)
                System.out.print(mat[i][left] + " ");
            left++;
        }
    }
}


Java Code (Anti-clockwise Spiral Traversal)

public static void spiralAntiClockwise(int[][] mat) {
    int m = mat.length, n = mat[0].length;
    int top = 0, bottom = m - 1, left = 0, right = n - 1;

    while (top <= bottom && left <= right) {
        // Traverse left column
        for (int i = top; i <= bottom; i++)
            System.out.print(mat[i][left] + " ");
        left++;

        // Traverse bottom row
        for (int j = left; j <= right; j++)
            System.out.print(mat[bottom][j] + " ");
        bottom--;

        // Traverse right column
        if (left <= right) {
            for (int i = bottom; i >= top; i--)
                System.out.print(mat[i][right] + " ");
            right--;
        }

        // Traverse top row
        if (top <= bottom) {
            for (int j = right; j >= left; j--)
                System.out.print(mat[top][j] + " ");
            top++;
        }
    }
}
```

**Complexity:**
- Time: O(m*n) → Every element is visited once.
- Space: O(1) → in-place


---

## 5. Variants / Follow-Ups

- Return spiral traversal as an array instead of printing.
- Zigzag spiral traversal.
- Multi-layered spiral (spiral inward, then outward).

<!-- #endregion -->

<!-- #region 32-Sum of Upper and Lower Triangles (Primary & Secondary Diagonals) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q32: Sum of Upper and Lower Triangles (Primary & Secondary Diagonals)</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an n x n square matrix.  Task: Calculate sum of upper triangle and lower triangle w.r.t a diagonal.  Upper Triangle: diagonal + elements above it.  Lower Triangle: diagonal + elements below it.  Can be asked for:  Primary diagonal (↘ top-left → bottom-right).  Secondary diagonal (↙ top-right → bottom-left).

---

## 2. Constraints

- 1 ≤ n ≤ 1000
- -10³ ≤ mat[i][j] ≤ 10³


---

## 3. Examples & Edge Cases

**Example 1 (Primary Diagonal):**
Input:
```text
3
1 2 3
1 5 3
4 5 6
```
Output:
```text
20 22
Explanation:

Upper (primary): 1 + 2 + 3 + 5 + 3 + 6 = 20

Lower (primary): 1 + 1 + 4 + 5 + 5 + 6 = 22
```

**Example 2 (Secondary Diagona):**
Input:
```text
3
1 2 3
4 5 6
7 8 9
```
Output:
```text
23 21
Explanation:

Upper (secondary): includes diagonal + above it → 3 + 2 + 1 + 5 + 7 + 9 = 23

Lower (secondary): includes diagonal + below it → 3 + 6 + 9 + 5 + 8 = 21
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Traverse entire matrix twice:
  - First time → compute upper triangle sum.
  - Second time → compute lower triangle sum.
  - Works but has unnecessary double traversal.

**Java Code:**
```java
public static void triangleSumsBrute(int n, int[][] mat, boolean primary) {
    int upper = 0, lower = 0;

    // Upper Triangle
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (primary) { // primary diagonal
                if (j >= i) upper += mat[i][j];
            } else { // secondary diagonal
                if (i + j <= n - 1) upper += mat[i][j];
            }
        }
    }

    // Lower Triangle
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (primary) {
                if (j <= i) lower += mat[i][j];
            } else {
                if (i + j >= n - 1) lower += mat[i][j];
            }
        }
    }

    System.out.println(upper + " " + lower);
}
```

**Complexity:**
- Time: O(n²) × 2 = O(n²).
- Space: O(1).

### Approach 2: Optimal (Single Traversal)

- **Idea:**
  - Traverse matrix once.
  - Use conditions:
  - Primary Diagonal:
  - j >= i → upper, j <= i → lower.
  - Secondary Diagonal:
  - i + j <= n-1 → upper, i + j >= n-1 → lower.

**Java Code:**
```java
public static void triangleSumsOptimal(int n, int[][] mat, boolean primary) {
    int upper = 0, lower = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (primary) {  // Primary Diagonal
                if (j >= i) upper += mat[i][j];
                if (j <= i) lower += mat[i][j];
            } else {        // Secondary Diagonal
                if (i + j <= n - 1) upper += mat[i][j];
                if (i + j >= n - 1) lower += mat[i][j];
            }
        }
    }

    System.out.println(upper + " " + lower);
}
```

**Complexity:**
- Time: O(n²).
- Space: O(1).


---

## 5. Justification / Proof of Optimality

- Brute Force
- Traverse twice → once for upper, once for lower.
- Simple to understand, easy to debug.
- Works for both primary & secondary diagonals by just changing the condition.
- But redundant (two passes).
- Optimal (Single Traversal)
- Compute both sums in one loop.
- Reduces traversal overhead → still O(n²) but constant factors smaller.
- Cleaner & more efficient in practice.

---

## 6. Variants / Follow-Ups

- Variant A: Primary Diagonal (with diagonal) ✅ (default problem).
- Variant B: Secondary Diagonal (with diagonal) ✅ (extension).
- Variant C: Both diagonals at once ✅ (extended version).
- Variant D: Excluding diagonals ❌ (less common, but sometimes asked).

<!-- #endregion -->

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

<!-- #region 34-Diagonal Traversal Function (Top-Right & Top-Left) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q34: Diagonal Traversal Function (Top-Right & Top-Left)</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** We start from either top-right corner or top-left corner, and move diagonal by diagonal until the end.

---

## 2. Constraints

- 1 <= N <= 500
- -10^4 <= mat[i][j] <= 10^4


---

## 3. Examples & Edge Cases

**Example 1 (Top-Right → Bottom-Left):**
Input:
```text
3
1 2 3
4 5 6
7 8 9
topRight
```
Output:
```text
3 2 6 1 5 9 4 8 7
```

**Example 2 (Top-Left → Bottom-Right):**
Input:
```text
3
1 2 3
4 5 6
7 8 9
topLeft
```
Output:
```text
1 2 4 3 5 7 6 8 9
```


---

## 4. Approaches

### Approach 1: Brute Force Using Extra Storage

- **Idea:**
  - Store each diagonal in a list and print later.

**Java Code:**
```java
List<Integer> result = new ArrayList<>();
// Traverse diagonals, store elements in result
// Print result at end
```

**Complexity:**
- Time: O(N^2) — every element visited once.
- Space: O(N^2) — storing traversal in a list.

### Approach 2: Optimal In-Place Traversal

- **Idea:**
  - Idea: Print elements while traversing diagonals without extra memory.
  - Steps:
  - For Top-Right → Bottom-Left:
  - Start from top row, last column → traverse diagonals.
  - Then start from first column, row 1 → traverse remaining diagonals.
  - For Top-Left → Bottom-Right:
  - Start from top row, first column → traverse diagonals.
  - Then start from last column, row 1 → traverse remaining diagonals.

**Java Code:**
```java
public static void diagonalTraversalInPlace(int[][] mat, int n, String direction) {
    if(direction.equalsIgnoreCase("topRight")) {
        for(int col=n-1; col>=0; col--) {
            int i=0, j=col;
            while(i<n && j<n) { System.out.print(mat[i][j]+" "); i++; j++; }
        }
        for(int row=1; row<n; row++) {
            int i=row, j=0;
            while(i<n && j<n) { System.out.print(mat[i][j]+" "); i++; j++; }
        }
    } else if(direction.equalsIgnoreCase("topLeft")) {
        for(int col=0; col<n; col++) {
            int i=0, j=col;
            while(i<n && j>=0) { System.out.print(mat[i][j]+" "); i++; j--; }
        }
        for(int row=1; row<n; row++) {
            int i=row, j=n-1;
            while(i<n && j>=0) { System.out.print(mat[i][j]+" "); i++; j--; }
        }
    }
}
```

**Complexity:**
- Time: O(N^2)
- Space: O(1) (no extra list)


---

## 5. Justification / Proof of Optimality

- In-place solution is optimal: no extra memory, linear traversal of all elements.
- Brute-force using extra list is unnecessary for large N.
- Complexity is optimal for N x N matrix: O(N^2) time, O(1) space.

---

## 6. Variants / Follow-Ups

- Non-square matrix (M x N) — can adapt traversal loops.
- Anti-diagonal traversal — starting from bottom-right instead of top corners.
- Diagonal sum instead of traversal — compute sums during traversal.

<!-- #endregion -->

<!-- #region 35-Second Largest Element in an Array -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q35: Second Largest Element in an Array</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** Traverse the array once (or optimally) to determine the largest and second-largest distinct elements. If only one distinct element exists, return -1.

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- Array may contain duplicate elements.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[8, 8, 7, 6, 5]
```
Output:
```text
7
```

**Example 2 (Normal Case):**
Input:
```text
[10, 10, 10, 10, 10]
```
Output:
```text
-1
```


---

## 4. Approaches

### Approach 1: Brute Force (Sort + Unique)

- **Idea:**
  - Remove duplicates.
  - Sort array descending.
  - Return second element if exists, else -1.

**Java Code:**
```java
public static int secondLargestBrute(int[] nums) {
    TreeSet<Integer> set = new TreeSet<>();
    for(int num : nums) set.add(num); // automatically sorts and keeps unique
    if(set.size() < 2) return -1;
    set.pollLast(); // remove largest
    return set.last(); // second largest
}
```

**Complexity:**
- Time: O(n log n) (due to sorting)
- Space: O(n) (for storing unique elements)

### Approach 2: Optimal (Single Pass)

- **Idea:**
  - Track two variables: largest and secondLargest.
  - Traverse the array once.
  - Update largest and secondLargest as you encounter higher values.

**Java Code:**
```java
public static int secondLargestOptimal(int[] nums) {
    int largest = Integer.MIN_VALUE;
    int secondLargest = Integer.MIN_VALUE;
    for(int num : nums){
        if(num > largest){
            secondLargest = largest;
            largest = num;
        } else if(num < largest && num > secondLargest){
            secondLargest = num;
        }
    }
    return secondLargest == Integer.MIN_VALUE ? -1 : secondLargest;
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approach traverses the array once (O(n)) and does not require extra space, making it better than the sorting approach.
- Handles duplicates and negative values correctly.

---

## 6. Variants / Follow-Ups

- Find third-largest, fourth-largest, etc. → can extend with more variables.
- Handle kth largest element using min-heap for large arrays.
- Return indices of the largest and second-largest elements instead of values.

<!-- #endregion -->

<!-- #region 36-Remove Duplicates from Sorted Array -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q36: Remove Duplicates from Sorted Array</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given a sorted array, remove duplicates in-place so that each unique element appears only once.
- **Goal:** Return the count of unique elements and modify the array so that the first k elements contain the unique values in order.
- **Paraphrase:** Iterate through the array, skip duplicates, and shift unique elements to the front.

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- Array is sorted in non-decreasing order.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[0, 0, 3, 3, 5, 6]
```
Output:
```text
4, Array = [-2, 2, 4, 5, _, _, _, _]
```

**Example 2 (Normal Case):**
Input:
```text
[-30, -30, 0, 0, 10, 20, 30, 30]
```
Output:
```text
5, Array = [-30, 0, 10, 20, 30, _, _, _]
```


---

## 4. Approaches

### Approach 1: Brute Force (Using Extra Array)

- **Idea:**
  - Traverse the array and add unique elements to a new array.
  - Copy the unique elements back to the original array.

**Java Code:**
```java
public static int removeDuplicatesBrute(int[] nums) {
    int n = nums.length;
    if(n == 0) return 0;
    int[] temp = new int[n];
    int index = 0;
    temp[index++] = nums[0];
    for(int i = 1; i < n; i++){
        if(nums[i] != nums[i-1]){
            temp[index++] = nums[i];
        }
    }
    for(int i = 0; i < index; i++){
        nums[i] = temp[i];
    }
    return index;
}
```

**Complexity:**
- Time: O(n)
- Space: O(n)

### Approach 2: Optimal (Two Pointers, In-place)

- **Idea:**
  - Use two pointers: i for iteration, j for placing unique elements.
  - If nums[i] != nums[i-1], copy nums[i] to nums[j].
  - Return j+1 as count of unique elements.

**Java Code:**
```java
public static int removeDuplicatesOptimal(int[] nums) {
    if(nums.length == 0) return 0;
    int j = 0; // pointer for placing unique elements
    for(int i = 1; i < nums.length; i++){
        if(nums[i] != nums[j]){
            j++;
            nums[j] = nums[i];
        }
    }
    return j + 1; // number of unique elements
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approach only traverses the array once and does not require extra space.
- Preserves relative order of unique elements.
- Works for negative numbers, duplicates, and arrays of size 1.

---

## 6. Variants / Follow-Ups

- Remove duplicates from unsorted array → requires HashSet or sorting first.
- Count frequency of unique elements while removing duplicates.
- Return array of unique elements instead of modifying in-place.

<!-- #endregion -->

<!-- #region 37-Find Missing Number -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q37: Find Missing Number</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array of distinct integers of size n containing values from 0 to n inclusive, one number is missing in the range.
- **Paraphrase:** The array has n elements, but the complete range 0..n has n+1 elements. Identify the number not present in the array.

---

## 2. Constraints

- n == nums.length
- 1 <= n <= 10^4
- 0 <= nums[i] <= n
- All numbers in nums are unique.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[1, 2, 3]
```
Output:
```text
0
```

**Example 2 (Normal Case):**
Input:
```text
[0, 2, 3, 1, 4]
```
Output:
```text
5
```

**Example 3 (Normal Case):**
Input:
```text
[0, 1, 2, 4, 5, 6]
```
Output:
```text
3
```


---

## 4. Approaches

### Approach 1: Brute Force (Check Each Number)

- **Idea:**
  - Traverse numbers 0..n.
  - For each number, check if it exists in the array.
  - Return the number which is missing.

**Java Code:**
```java
public static int missingNumberBrute(int[] nums) {
    int n = nums.length;
    for(int num = 0; num <= n; num++){
        boolean found = false;
        for(int i = 0; i < n; i++){
            if(nums[i] == num){
                found = true;
                break;
            }
        }
        if(!found) return num;
    }
    return -1; // should not happen
}
```

**Complexity:**
- Time: O(n^2)
- Space: O(1)

### Approach 2: Optimal (Sum Formula / XOR)

- **Idea:**
  - Idea 1 (Sum Formula):
  - Sum of numbers from 0..n = n*(n+1)/2.
  - Subtract sum of elements in nums.
  - Remaining value = missing number.
  - Idea 2 (XOR):
  - XOR all numbers 0..n.
  - XOR all elements in nums.
  - XOR of the two results = missing number (because duplicates cancel out).

**Java Code:**
```java
Java Code (Sum Formula):

public static int missingNumberOptimal(int[] nums) {
    int n = nums.length;
    int sum = n * (n + 1) / 2;
    int arrSum = 0;
    for(int num : nums){
        arrSum += num;
    }
    return sum - arrSum;
}


Java Code (XOR):

public static int missingNumberXOR(int[] nums) {
    int n = nums.length;
    int xor = 0;
    for(int i = 0; i <= n; i++){
        xor ^= i;
    }
    for(int num : nums){
        xor ^= num;
    }
    return xor;
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approaches are O(n) and O(1) extra space.
- Works regardless of array order.
- Sum formula may risk integer overflow for large n, XOR avoids overflow.

---

## 6. Variants / Follow-Ups

- Array may have duplicates → find missing number or repeated numbers.
- Numbers not in range [0, n] → adjust formula or XOR range.
- Find all missing numbers in [0, n] if multiple missing.

<!-- #endregion -->

<!-- #region 38-Union of Two Sorted Arrays -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q38: Union of Two Sorted Arrays</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** Combine two sorted arrays into one sorted array containing all distinct elements.

---

## 2. Constraints

- 1 <= n, m <= 10^5
- -10^6 <= arr1[i], arr2[i] <= 10^6
- Arrays are sorted.


---

## 3. Examples & Edge Cases

**Example 1 (Edge Case):**
Input:
```text
arr1 = [], arr2 = [1, 2, 3]
```
Output:
```text
0[1, 2, 3]
```

**Example 2 (Normal Case):**
Input:
```text
arr1 = [1, 2, 3], arr2 = [2, 3, 4]
```
Output:
```text
[1, 2, 3, 4]
```


---

## 4. Approaches

### Approach 1: Brute Force (Merge & Set)

- **Idea:**
  - Merge both arrays into one array.
  - Use a HashSet to remove duplicates.
  - Convert HashSet to sorted array.

**Java Code:**
```java
public static int[] unionBrute(int[] arr1, int[] arr2) {
    Set<Integer> set = new HashSet<>();
    for(int num : arr1) set.add(num);
    for(int num : arr2) set.add(num);
    int[] result = set.stream().sorted().mapToInt(i -> i).toArray();
    return result;
}
```

**Complexity:**
- Time: O((n + m) log(n + m)) → sorting after merge.
- Space: O(n + m) → for merged array and HashSet.

### Approach 2: Optimal (Two Pointer Technique)

- **Idea:**
  - Use two pointers i and j for arr1 and arr2.
  - Compare elements:
  - If arr1[i] < arr2[j] → add arr1[i] and move i.
  - If arr1[i] > arr2[j] → add arr2[j] and move j.
  - If equal → add one element and move both pointers.
  - Skip duplicates by checking the last element added.
  - Add remaining elements from both arrays.

**Java Code:**
```java
public static List<Integer> unionOptimal(int[] arr1, int[] arr2) {
    List<Integer> union = new ArrayList<>();
    int i = 0, j = 0;
    while(i < arr1.length && j < arr2.length){
        int val;
        if(arr1[i] < arr2[j]){
            val = arr1[i++];
        } else if(arr1[i] > arr2[j]){
            val = arr2[j++];
        } else {
            val = arr1[i];
            i++; j++;
        }
        if(union.isEmpty() || union.get(union.size() - 1) != val){
            union.add(val);
        }
    }
    while(i < arr1.length){
        if(union.get(union.size() - 1) != arr1[i])
            union.add(arr1[i]);
        i++;
    }
    while(j < arr2.length){
        if(union.get(union.size() - 1) != arr2[j])
            union.add(arr2[j]);
        j++;
    }
    return union;
}
```

**Complexity:**
- Time: O(n + m) → single pass through both arrays.
- Space: O(n + m) → for result array.


---

## 5. Justification / Proof of Optimality

- Optimal approach avoids sorting after merge → faster O(n+m)
- Maintains sorted order naturally.
- Handles duplicates efficiently.

---

## 6. Variants / Follow-Ups

- Intersection of two sorted arrays → instead of union.
- k-sorted arrays union → merge k arrays using priority queue.
- Unsorted arrays → sort first or use HashSet.
- Find union without extra space if arrays are mutable.

<!-- #endregion -->

<!-- #region 39-Leaders in an Array -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q39: Leaders in an Array</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array of integers, identify elements that are strictly greater than all elements to their right.
- **Goal:** Return a list of all such leaders in the order they appear in the array.
- **Paraphrase:** : A leader is any element that is bigger than all elements on its right, including the last element (which is always a leader).

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4


---

## 3. Examples & Edge Cases

**Example 1 (Edge Case):**
Input:
```text
[10]
```
Output:
```text
[10]
```

**Example 2 (Normal Case):**
Input:
```text
[-3, 4, 5, 1, -4, -5]
```
Output:
```text
[5, 1, -4, -5]
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - For each element nums[i], check all elements to its right.
  - If nums[i] is greater than all right elements, add to result.

**Java Code:**
```java
public static List<Integer> leadersBrute(int[] nums) {
    List<Integer> leaders = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        boolean isLeader = true;
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] <= nums[j]) {
                isLeader = false;
                break;
            }
        }
        if (isLeader) leaders.add(nums[i]);
    }
    return leaders;
}
```

**Complexity:**
- Time: O(n^2)
- Space: O(1)

### Approach 2: Optimal (Right-to-Left Scan)

- **Idea:**
  - Initialize maxFromRight = nums[n-1] → last element is always a leader.
  - Traverse array from right to left:
  - If nums[i] > maxFromRight, then nums[i] is a leader.
  - Update maxFromRight = max(maxFromRight, nums[i]).
  - Reverse result list at the end to maintain original order.

**Java Code:**
```java
public static List<Integer> leadersOptimal(int[] nums) {
    List<Integer> leaders = new ArrayList<>();
    int n = nums.length;
    int maxFromRight = nums[n - 1];
    leaders.add(maxFromRight); // last element is always a leader
    
    for (int i = n - 2; i >= 0; i--) {
        if (nums[i] > maxFromRight) {
            leaders.add(nums[i]);
            maxFromRight = nums[i];
        }
    }
    
    Collections.reverse(leaders); // to maintain order of appearance
    return leaders;
}
```

**Complexity:**
- Time: O(n) → single pass.
- Space: O(n) → for result list.


---

## 5. Justification / Proof of Optimality

- Optimal approach is linear time and efficient, scanning the array once from right.
- Avoids nested loops → much faster for large arrays (n <= 10^5).
- Always maintains order by reversing the result at the end.

---

## 6. Variants / Follow-Ups

- Find all leaders except the last element → modified right-to-left scan.
- Find leaders in a circular array → consider array wraps around.
- Find minimum leaders → elements smaller than all right elements.
- Count the number of leaders instead of listing them.

<!-- #endregion -->
<!-- #region 40-Rearrange Array Elements by Sign -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q40: Rearrange Array Elements by Sign</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array with equal numbers of positive and negative integers, rearrange it such that every consecutive pair has opposite signs.
- **Goal:** Return the rearranged array starting with a positive number while maintaining relative order of positive and negative numbers.
- **Paraphrase:** Interleave positive and negative integers while preserving their original order, starting with a positive integer.

---

## 2. Constraints

- 2 <= nums.length <= 10^5
- 1 <= |nums[i]| <= 10^4
- Array length is even and positives = negatives.


---

## 3. Examples & Edge Cases

**Example 1 (Edge Case):**
Input:
```text
[1, -1]
```
Output:
```text
[1, -1]
```

**Example 2 (Normal Case):**
Input:
```text
[2, 4, 5, -1, -3, -4]
```
Output:
```text
[2, -1, 4, -3, 5, -4]
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Create two separate lists: positives and negatives.
  - Traverse the array and fill the two lists.
  - Interleave elements from positives and negatives into a new array.

**Java Code:**
```java
public static int[] rearrangeBrute(int[] nums) {
    List<Integer> pos = new ArrayList<>();
    List<Integer> neg = new ArrayList<>();
    
    for (int num : nums) {
        if (num > 0) pos.add(num);
        else neg.add(num);
    }
    
    int[] result = new int[nums.length];
    int p = 0, n = 0;
    
    for (int i = 0; i < nums.length; i++) {
        if (i % 2 == 0) result[i] = pos.get(p++);
        else result[i] = neg.get(n++);
    }
    
    return result;
}
```

**Complexity:**
- Time: O(n) → single pass for separation + single pass for merging.
- Space: O(n) → extra arrays for positive and negative numbers.

### Approach 2: Optimal (In-Place Using Extra Indices)

- **Idea:**
  - Use two pointers p and n to track positions of next positive and negative numbers.
  - Traverse the array and place positive and negative numbers alternately in a new array (or can do in-place with rotation logic).
  - Maintains relative order without nested loops.

**Java Code:**
```java
public static int[] rearrangeOptimal(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    int p = 0, q = 0; // indices for positives and negatives
    
    // find first positive index
    for (int i = 0; i < n; i++) {
        if (nums[i] > 0) p++;
        else q++;
    }
    
    List<Integer> pos = new ArrayList<>();
    List<Integer> neg = new ArrayList<>();
    
    for (int num : nums) {
        if (num > 0) pos.add(num);
        else neg.add(num);
    }
    
    int pi = 0, ni = 0;
    for (int i = 0; i < n; i++) {
        if (i % 2 == 0) result[i] = pos.get(pi++);
        else result[i] = neg.get(ni++);
    }
    
    return result;
}
```

**Complexity:**
- Time: O(n) → single pass.
- Space: O(n) → result array needed (in-place is more complex but possible).


---

## 5. Justification / Proof of Optimality

- Optimal approach ensures O(n) time and preserves relative order of positives and negatives.
- Brute force is easier to implement but uses extra space.
- In-place rearrangement is possible but increases code complexity significantly.

---

## 6. Variants / Follow-Ups

- Array length odd → more positives or negatives → start with dominant sign.
- Rearrange without extra space → in-place cyclic replacement.
- Rearrange to alternate starting with negative.
- Extend to k-alternating signs (e.g., 2 positives, 1 negative pattern).

<!-- #endregion -->


<!-- #region 41-Two Sum -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q41: Two Sum</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array of integers nums and a target integer target, find indices of two numbers in nums that sum up to target.
- **Paraphrase:** Find exactly one pair (i, j) such that nums[i] + nums[j] = target

---

## 2. Constraints

- 2 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- -10^5 <= target <= 10^5
- Exactly one solution exists
- Each element used at most once


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[1,6,2,10,3], target 7
```
Output:
```text
[0,1]
```

**Example 2 (Normal Case):**
Input:
```text
[1,3,5,-7,6,-3], target 0
```
Output:
```text
[1,5]
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Check all possible pairs (i,j) where i<j to see if nums[i]+nums[j] == target.

**Java Code:**
```java
public int[] twoSumBrute(int[] nums, int target) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{-1, -1}; // should never reach here
}
```

**Complexity:**
- Time: O(n^2) → two nested loops.
- Space: O(1) → no extra space.

### Approach 2: Optimal (HashMap)

- **Idea:**
  - Traverse array and store value → index in a HashMap.
  - For each element num, check if target - num exists in the map.
  - Return indices immediately when found.

**Java Code:**
```java
import java.util.*;

public int[] twoSumOptimal(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return new int[]{-1, -1}; // should never reach here
}
```

**Complexity:**
- Time: O(n) → single traversal.
- Space: O(n) → for the HashMap.


---

## 5. Justification / Proof of Optimality

- Brute Force: Simple, but inefficient for large arrays (n^2).
- HashMap Approach: Efficient, single pass, handles negative numbers and duplicates, optimal solution.

---

## 6. Variants / Follow-Ups

- Find all pairs summing to target (multiple solutions).
- Array is sorted → can use two-pointer approach instead of HashMap.
- Return values instead of indices.
- Target sum for more than two numbers → generalizes to 3-sum, 4-sum problems.

<!-- #endregion -->

<!-- #region 42-Sort an Array of 0's, 1's, and 2's -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q42: Sort an Array of 0's, 1's, and 2's</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array containing only 0, 1, and 2, sort the array in non-decreasing order.
- **Paraphrase:** Arrange all 0’s first, followed by 1’s, then 2’s in the original array.

---

## 2. Constraints

- 2 <= nums.length <= 10^5
- 1 <= |nums[i]| <= 10^4
- Array length is even and positives = negatives.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[1,0,2,1,0]
```
Output:
```text
[0,0,1,1,2]
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Count number of 0’s, 1’s, and 2’s.
  - Overwrite the array with 0’s, 1’s, 2’s according to counts.

**Java Code:**
```java
public static void sortColorsCounting(int[] nums) {
    int count0 = 0, count1 = 0, count2 = 0;
    for (int num : nums) {
        if (num == 0) count0++;
        else if (num == 1) count1++;
        else count2++;
    }
    
    int i = 0;
    while (count0-- > 0) nums[i++] = 0;
    while (count1-- > 0) nums[i++] = 1;
    while (count2-- > 0) nums[i++] = 2;
}
```

**Complexity:**
- Time: O(n) → single pass to count + single pass to fill.
- Space: O(1) → 3 counters only.

### Approach 2: Optimal (Dutch National Flag Algorithm)

- **Idea:**
  - Use three pointers: low, mid, high.
  - low → next position for 0, mid → current element, high → next position for 2.
  - Traverse array with mid:
  - If nums[mid] == 0 → swap with nums[low], low++, mid++
  - If nums[mid] == 1 → mid++
  - If nums[mid] == 2 → swap with nums[high], high--

**Java Code:**
```java
public static void sortColorsDutchFlag(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;
    while (mid <= high) {
        if (nums[mid] == 0) {
            int temp = nums[low];
            nums[low] = nums[mid];
            nums[mid] = temp;
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else { // nums[mid] == 2
            int temp = nums[mid];
            nums[mid] = nums[high];
            nums[high] = temp;
            high--;
        }
    }
}
```

**Complexity:**
- Time: O(n) → single traversal.
- Space: O(1) → in-place.


---

## 5. Justification / Proof of Optimality

- Dutch National Flag Algorithm ensures single pass and in-place sorting, optimal for large arrays.
- Counting method is simple and uses constant space, but requires two passes.

---

## 6. Variants / Follow-Ups

- Array contains more than 3 distinct elements → can extend with counting sort.
- Sort array in decreasing order instead of non-decreasing.
- Array elements are not consecutive integers → use hashmap or quicksort.
- Online streaming input → maintain three queues instead of array.

<!-- #endregion -->

<!-- #region 43-Move Zeros to End -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q43: Move Zeros to End</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array nums, move all the 0s to the end of the array without changing the relative order of non-zero elements.
- **Paraphrase:** Shift all zero elements to the right while keeping non-zero elements in their original sequence.

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- Must be in-place (no extra array allowed).


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[0, 1, 4, 0, 5, 2]
```
Output:
```text
[1, 4, 5, 2, 0, 0]
```


---

## 4. Approaches

### Approach 1: Brute Force (Shift Zeros Iteratively)

- **Idea:**
  - For each zero, shift all elements on its right one step left and append zero at the end.

**Java Code:**
```java
public void moveZerosBrute(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        if (nums[i] == 0) {
            for (int j = i; j < n - 1; j++) {
                nums[j] = nums[j + 1];
            }
            nums[n - 1] = 0;
        }
    }
}
```

**Complexity:**
- Time: O(n^2) → shifting elements for each zero.
- Space: O(1) → in-place.

### Approach 2: Optimal (Two Pointers)

- **Idea:**
  - Use a pointer pos to track the position to place the next non-zero element.
  - Traverse the array: if element is non-zero, swap it with nums[pos] and increment pos.
  - All zeros naturally move to the end.

**Java Code:**
```java
public void moveZerosOptimal(int[] nums) {
    int pos = 0; // position for the next non-zero
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            int temp = nums[i];
            nums[i] = nums[pos];
            nums[pos] = temp;
            pos++;
        }
    }
}
```

**Complexity:**
- Time: O(n) → single traversal.
- Space: O(1) → in-place.


---

## 5. Justification / Proof of Optimality

- Brute Force: Works but inefficient for large n due to repeated shifting.
- Two-Pointer Approach:
- Only swaps non-zero elements, maintains relative order,
- linear time complexity, minimal swaps → optimal solution.

---

## 6. Variants / Follow-Ups

- Move all non-zero elements to the start instead.
- Move all zeros to the beginning.
- Count minimum swaps required to move zeros to end.
- Multiple arrays merged → move zeros in combined array.

<!-- #endregion -->

<!-- #region 44-Special Matrix -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q44: Special Matrix</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Determine whether a given square matrix is a special matrix.  Definition:  Diagonal elements (matrix[i][i]) are non-zero.  All non-diagonal elements (matrix[i][j] where i ≠ j) are zero.
- **Goal:** Return true if the matrix satisfies the above conditions, otherwise false.
- **Paraphrase:** Only diagonal elements are non-zero; everything else must be zero.

---

## 2. Constraints

- 1 <= T <= 10
- 1 <= N <= 200
- 0 <= A[i][j] <= 10^6


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
1
3
1 0 2
0 2 0
3 0 1
```
Output:
```text
true
```

**Example 2 (Normal Case):**
Input:
```text
1
3
1 0 1
1 2 0
2 0 3
```
Output:
```text
false
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Traverse the entire matrix.
  - For each element matrix[i][j]:
  - If i == j → check non-zero
  - Else → check zero

**Java Code:**
```java
public boolean isSpecialMatrix(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i == j) {
                if (matrix[i][j] == 0) return false;
            } else {
                if (matrix[i][j] != 0) return false;
            }
        }
    }
    return true;
}
```

**Complexity:**
- Time: O(N^2) → visit all elements.
- Space: O(1) → in-place.

### Approach 2: Optimized (Early Exit)

- **Idea:**
  - Traverse row by row.
  - As soon as a non-diagonal element is non-zero or a diagonal element is zero → return false.
  - Otherwise, after traversal → return true.

**Java Code:**
```java
public boolean isSpecialMatrixOptimized(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n; i++) {
        if (matrix[i][i] == 0) return false; // diagonal must be non-zero
        for (int j = 0; j < n; j++) {
            if (i != j && matrix[i][j] != 0) return false; // non-diagonal must be zero
        }
    }
    return true;
}
```

**Complexity:**
- Time: O(N^2) in worst-case, but can exit early.
- Space: O(1) → in-place.


---

## 5. Justification / Proof of Optimality

- Both approaches correctly check all diagonal and non-diagonal conditions.
- Optimized approach can exit early, saving some comparisons when matrix is not special.

---

## 6. Variants / Follow-Ups

- Check if a matrix is diagonal (diagonal elements can be zero).
- Check for identity matrix (diagonal elements must be 1, others 0).
- Special matrices with non-square matrices → check main diagonal only.

<!-- #endregion -->

