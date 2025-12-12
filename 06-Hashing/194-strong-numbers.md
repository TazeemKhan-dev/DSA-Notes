<!-- #region 194-Strong numbers -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q194: Strong numbers</h1>

## 1. Problem Statement

- A number N is called strong if for every prime factor X of N, the number X² also divides N.
- You must return:
  * 1 (true) → if N is strong
  * 0 (false) → if N is not strong
---

## 2. Problem Understanding

- A number is strong only when every prime factor appears with exponent ≥ 2 in its prime factorization.
- Example:
  * 72 = 2^3 * 3^2
  * Prime 2 exponent = 3 → ≥ 2
  * Prime 3 exponent = 2 → ≥ 2
  * → strong
  * 7 = 7^1
  * Prime 7 exponent = 1 → < 2
  * → not strong
  * So the problem reduces to:
- ✔️ Check if each prime factor of N divides N at least twice.
---

## 3. Constraints

- 1 ≤ N ≤ 10000
- Very small limit → prime factorization is trivial and fast.
---

## 4. Edge Cases

- N = 1 → strong (no prime factors fail the rule)
- Prime numbers → always not strong
- Squares → usually strong
- Mixed primes with exponent 1 → fail
---

## 5. Examples

```text
Example 1:

N = 7  
Prime factor: 7^1 → exponent < 2 → output 0


Example 2:

N = 72  
Prime factors: 2^3, 3^2 → all exponents ≥ 2 → output 1
```

---

## 6. Approaches

### Approach 1: Prime Factorization + Check Exponents (Optimal)

**Idea:**
- Factorize N using trial division up to √N.
- For every prime factor, count exponent.
- If any exponent < 2 → return false.
- Else true.

**Steps:**
- For each i from 2 to √N:
  * Count exponent of i in N.
  * If i divides N but exponent < 2 → return false.
- After loop, if remaining N > 1 → that remaining N is a prime factor with exponent 1 → return false.
- Else return true.

**Java Code:**
```java
public boolean isStrong(int N) {
    if (N == 1) return true; 

    int num = N;

    for (int p = 2; p * p <= num; p++) {
        if (num % p == 0) {
            int count = 0;
            while (num % p == 0) {
                num /= p;
                count++;
            }
            if (count < 2) return false;
        }
    }

    if (num > 1) return false;

    return true;
}
```

**💭 Intuition Behind the Approach:**
- Prime factorization exposes each prime's exponent.
- A prime factor contributes at least two copies only if it divides the number twice.
- Thus checking exponent ≥ 2 for every prime factor is enough.

**Complexity (Time & Space):**
- Time: O(√N) — trial division up to square root
- Space: O(1) — only counters used

---

## 7. Justification / Proof of Optimality

- A number is strong if and only if every prime factor appears at least twice in its prime factorization.
- Why? Because:
  * If a prime factor p divides the number only once (p¹), then p² cannot divide the number → fails the condition.
  * If a prime factor appears twice or more (p², p³, ...), then p² divides the number → condition satisfied.
  * Prime factorization uniquely determines exponents, so checking exponent ≥ 2 is both necessary and sufficient.
- Thus, verifying that each prime factor has exponent ≥ 2 gives a mathematically complete and optimal solution.
---

## 8. Variants / Follow-Ups

- Check if number is perfect square
- Check if number is powerful (same definition — this is the classical "Powerful Number" problem)
- Check if number is squareful
---

## 9. Tips & Observations

- This problem is identical to checking if N is a Powerful Number.
- Remaining N > 1 after factor loop means a leftover prime with exponent 1 → fail.
- Works efficiently up to 10k with ease.
---

## 10. Pitfalls

- Forgetting that N > 1 after factorization implies exponent 1
- Not handling N = 1
- Checking divisibility only once instead of counting exponent
---

<!-- #endregion -->
