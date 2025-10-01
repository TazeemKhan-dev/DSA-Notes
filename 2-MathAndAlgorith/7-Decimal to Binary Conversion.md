
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

