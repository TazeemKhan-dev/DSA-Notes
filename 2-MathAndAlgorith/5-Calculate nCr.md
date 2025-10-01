
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q5: Calculate nCr</h1>

## 1. Understand the Problem
- **Read & Identify:** Given two integers, n (total items) and r (items to choose), the goal is to calculate the binomial coefficient, nCr, which represents the number of distinct combinations.
- **Goal:** Compute the value of nCr using the standard combinatorial formula.
- **Paraphrase:** Find the number of distinct subsets of size r from a set of size n.

---

## 2. Input, Output, & Constraints

- **Input:**
```text
Two non-negative integers, n and r.
```

- **Output:**
```text
A single integer representing the calculated value of nCr.
```

**Constraints:**
- 1≤n≤20
- 1≤r≤n
- The result will fit within a standard 64-bit integer (≈1.8×10 ^19).

## 3. Approaches

### Approach 1: Direct Factorial Calculation (Naive)

- **Idea:**
  - Directly compute the factorials for n, r, and (n−r), then perform the division. nCr=n!/(r!∗(n−r)!)

**Pseudocode:**
```text
function Calculate_nCr(n, r):
    // Requires a data type that can hold 20! (long long/64-bit integer)
    numerator = Factorial(n)
    denominator = Factorial(r) * Factorial(n - r)
    return numerator / denominator
```

**Complexity:**
- Time: O(n)
- Space: O(1) space.

### Approach 2: Optimized Multiplicative Formula (Preferred)

- **Idea:**
  - Simplify the fraction before calculation to minimize the size of intermediate numbers, which is crucial for larger n. The simplified form cancels out the largest factorial term, (n−r)!.

**Pseudocode:**
```text
function Calculate_nCr(n, r):
    // Use nCr = nC(n-r) property for fewer iterations
    if r > n / 2:
        r = n - r

    result = 1
    // Loop r times
    for i from 1 to r:
        // Current calculation: result * (n-i+1) / i
        result = result * (n - i + 1)
        result = result / i // Division is guaranteed to be exact
    return result
```

**Complexity:**
- Time: O(min(r,n−r)), which is O(n)
- Space: O(1) space

## 4. Justification / Proof of Optimality

- Approach 2 is the better solution. While both approaches are O(n) time complexity, Approach 2 keeps the intermediate values much smaller, which minimizes the risk of overflow. It directly computes nCr step-by-step, ensuring each partial product is a valid integer combination value, which is inherently safer than computing three separate, massive factorials (as in Approach 1) and hoping their ratio fits the integer type.

## 5. Variants / Follow-Ups

- Large Constraints (n,r≈10^9): When n and r are very large and the answer must be computed modulo p. This requires number theory techniques like Lucas Theorem or calculating factorials and their modular inverses using Fermat’s Little Theorem.
- Dynamic Programming (Pascal's Identity): For scenarios where many nCr values are needed, the relation nCr=(n−1)C(r−1)+(n−1)Cr allows filling a table (Pascal's Triangle) in O(n ^2) time.
- Permutations (nPr): The problem of calculating nPr=n!/(n−r)! involves a similar multiplicative approach, simply stopping before dividing by r!.

