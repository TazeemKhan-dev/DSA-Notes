<!-- #region 73-Number of ways to form Natural Number using Recursion -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q73: Number of ways to form Natural Number using Recursion</h1>

## 1. Problem Understanding

- You need to find how many ways an integer N can be represented as the sum of unique natural numbers.
- Order does not matter (e.g., 1+2+3 and 3+2+1 are the same).
---

## 2. Constraints

- 0≤N≤120
- Natural numbers: 1,2,3,...
- Each number can be used at most once.
---

## 3. Edge Cases

- N=0: 1 way (empty sum).
- N<0: 0 ways (invalid sum).
- N=1: 1 way (1 itself).
---

## 4. Examples

```text
Input:
6
Output:
4
Explanation:
6 = (1+2+3), (1+5), (2+4), (6)
```

---

## 5. Approaches

### Approach 1: Recursion with Backtracking

**Idea:**
- At every step, decide whether to include the current number or not.

**Steps:**
- Maintain a variable curr = current natural number to consider.
- Base Cases:
  * If n == 0, return 1 (valid combination found).
  * If n < 0, return 0 (invalid sum).
  * If curr > n, return 0 (no more numbers left to use).
- Recursive Cases:
  * Include curr: call for n - curr, curr + 1
  * Exclude curr: call for n, curr + 1
- Add both results.

**Java Code:**
```java
int countWays(int n, int curr) {
    if (n == 0) return 1;
    if (n < 0 || curr > n) return 0;

    // include current number + exclude current number
    return countWays(n - curr, curr + 1) + countWays(n, curr + 1);
}
```

**Complexity (Time & Space):**
- Time Complexity: O(2^N) — each number has two choices (include/exclude).
- Space Complexity: O(N) — recursion depth proportional to N.

---

## 6. Justification / Proof of Optimality

- Each recursive branch explores one subset of natural numbers that sum to N.
- Avoids repetition because each natural number is considered only once.
---

## 7. Variants / Follow-Ups

- Return actual combinations: instead of counting, store lists of numbers forming N.
- Allow repetition of numbers: modify recursion to allow including the same number again.
- Use memoization/DP to optimize from O(2^N) to O(N^2).
- Find combinations with exactly K numbers: add extra parameter to track count.
- Subset sum for array input: generalizes this approach to arbitrary arrays.
---

## 8. Tips & Observations

- This is similar to the Subset Sum Problem but with numbers 1..N.
- You can optimize it using memoization if needed (DP).
- The recursion tree grows exponentially, but N ≤ 120 makes it feasible for moderate inputs.
---

<!-- #endregion -->
