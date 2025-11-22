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

