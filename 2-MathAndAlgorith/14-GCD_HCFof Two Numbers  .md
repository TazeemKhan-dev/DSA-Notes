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

