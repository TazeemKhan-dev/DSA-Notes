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

