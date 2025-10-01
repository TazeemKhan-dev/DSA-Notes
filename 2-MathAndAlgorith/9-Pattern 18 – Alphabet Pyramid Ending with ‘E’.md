
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

