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
