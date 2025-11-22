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
