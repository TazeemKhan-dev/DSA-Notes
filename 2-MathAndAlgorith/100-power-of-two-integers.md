<!-- #region 100-Power of Two Integers -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q100: Power of Two Integers</h1>

## 1. Problem Understanding

- You are given a positive integer N (≤ 10⁹).
- You must determine if there exist two integers A > 0 and P > 1 such that:
- A^P=N
- If yes → print 1,
- else → print 0.
---

## 2. Constraints

- 0≤N≤10^9
- A>0
- P>1
- A and P are integers
- Must fit in 32-bit signed integer
---

## 3. Edge Cases

- N = 0 → cannot be expressed → 0
- N = 1 → 1^𝑃=1 for any P → 1
- Perfect powers like 4 (=2²), 8 (=2³), 9 (=3²), 16 (=4²) → 1
- Non-perfect powers like 10, 12, 20 → 0
---

## 4. Examples

```text
Input:

64


Output:

1


Explanation:
4^3=64 and also 8^2=64
```

---

## 5. Approaches

### Approach 1: Brute Force (Iterative)

**Idea:**
- Try every possible base A from 2 to √N.
- For each A, multiply repeatedly (A * A, A * A * A, etc.)
- until you exceed or match N.

**Java Code:**
```java
public static int isPower(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;

    for (int a = 2; a * a <= n; a++) {
        long p = a; // use long to avoid overflow
        while (p <= n) {
            p *= a;
            if (p == n) return 1;
        }
    }
    return 0;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Outer loop: √N
  * Inner loop: logₐ(N)
  * Overall: O(√N × logN)
- 💾 Space Complexity
  * O(1)

### Approach 2: Using Logarithms (Mathematical)

**Idea:**
- We know:
- N=A^P⟹P=log(A)/log(N) ​
- If P is an integer, then N can be expressed as A^P.
- Use math and rounding to check if P is close to an integer.

**Java Code:**
```java
public static int isPowerLog(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;

    for (int a = 2; a * a <= n; a++) {
        double p = Math.log(n) / Math.log(a);
        if (Math.abs(p - Math.round(p)) < 1e-10 && Math.round(p) > 1)
            return 1;
    }
    return 0;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
- O(√N)
- 💾 Space Complexity
- O(1)
- Note
- Floating-point precision can cause issues for large N.
- To reduce errors, use Math.round() with tolerance (1e-10).

### Approach 3: Recursive Power Check

**Idea:**
- Recursively try multiplying the base until the power exceeds N.
- For each base A, define:
  * check(A, current, N):
      * if current == N → true
      * if current > N → false
      * else → check(A, current * A, N)

**Java Code:**
```java
public static boolean check(long base, long current, long n) {
    if (current == n) return true;
    if (current > n) return false;
    return check(base, current * base, n);
}

public static int isPowerRecursive(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;

    for (int a = 2; a * a <= n; a++) {
        if (check(a, a, n)) return 1;
    }
    return 0;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Similar to iterative: O(√N × logN)
- 💾 Space Complexity
  * O(logN) (due to recursion depth)

### Approach 4: Precompute Powers (Optimization)

**Idea:**
- Instead of computing on the fly,
- store all possible powers of 2–9 that are ≤ 10⁹ in a HashSet.
- Then simply check if N exists in the set.

**Java Code:**
```java
public static int isPowerPrecompute(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;

    HashSet<Long> powers = new HashSet<>();
    for (int a = 2; a <= 31622; a++) { // 31622^2 ≈ 10^9
        long val = a * a;
        while (val <= 1_000_000_000L) {
            powers.add(val);
            val *= a;
        }
    }
    return powers.contains((long)n) ? 1 : 0;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Precomputation: O(√N × logN)
  * Query: O(1)
- 💾 Space Complexity
  * O(M) where M = number of precomputed powers (~few thousand)

---
### Approach 5 — Optimized Root-Based Method (Best One)

**Idea:**
-For a given exponent p, compute the integer base a = round(n^(1/p)).
Then verify if a^p == n.
- We only need to check exponents p from 2 to 31 (since 2^31 > 2,147,483,647).

**Why Approach 5 Works (Reason):**
- Instead of iterating all possible bases (which is huge for large n),
we iterate over small exponents (2–31) — just 30 iterations total.
- Computing n^(1/p) gives a direct estimate of the base,
avoiding billions of multiplications.
- Using long and rounding ensures accuracy and no overflow.
- For max input 2147483647, it completes instantly.
**Steps:**
- Handle base cases: if n == 0 → return 0, if n == 1 → return 1.
- Loop p from 2 to 31.
- Compute approximate a = n^(1/p) using Math.pow.
- Round to nearest integer, then multiply a p times (using long to avoid overflow).
- If the result equals n, return 1.
- After loop ends, return 0.


**Java Code:**
```java
public static int isPowerOptimized(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;

    for (int p = 2; p <= 31; p++) {
        double a = Math.pow(n, 1.0 / p);
        int base = (int) Math.round(a);

        long val = 1;
        for (int i = 0; i < p; i++) {
            val *= base;
            if (val > n) break;
        }

        if (val == n) return 1;
    }
    return 0;
}

```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(31 × logN) → practically O(1)
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Each approach ensures both A and P are integers.
- Brute-force and recursive approaches rely purely on integer arithmetic (no rounding errors).
- Logarithmic approach offers speed, but may have floating-point inaccuracies.
- Precomputation is best suited for multiple queries, trading off space for speed.
---

## 7. Variants / Follow-Ups

- Find All (A, P) pairs
- → Instead of returning 1, store or print all valid pairs where A^P=N.
- Find largest P for a given N
- → Find the highest exponent such that  A^P=N.
- Check Power of Base K
- → Restrict A to a given base K (e.g., only check if N is a power of 2).
- For multiple queries
- → Use precomputation + HashSet lookup for O(1) query per number.
---

## 8. Tips & Observations

- Avoid using Math.pow() in integer problems (floating-point rounding errors).
- Always use long while multiplying to avoid integer overflow.
- For recursion, stop early when product exceeds N — this prunes large search space.
- Logarithmic method is fastest for single queries but less reliable for precision-critical code.
- Precomputation is perfect for repeated inputs in large datasets.
- A need only go up to √N because A^2 must be ≤ N for P ≥ 2.
---

<!-- #endregion -->
