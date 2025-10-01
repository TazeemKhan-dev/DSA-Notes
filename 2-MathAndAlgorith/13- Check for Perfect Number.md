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

