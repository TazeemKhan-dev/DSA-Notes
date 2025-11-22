<!-- #region 2-Recursion Quick Reference Sheet -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q2: Recursion Quick Reference Sheet</h1>

## 1. Problem Understanding

- Recursion on Integers
  * Print 1 → n: print1ToN(n) → O(n) time & space
  * Print n → 1: printNTo1(n) → O(n)
  * Count numbers in range: countRange(start, end) → O(n)
  * Sum of first n numbers: sumN(n) → O(n)
  * Sum of digits: sumDigits(n) → O(log n)
  * Product of digits: productDigits(n) → O(log n)
  * Factorial: factorial(n) → O(n)
  * Power: power(x, n) → O(n) or O(log n) optimized
  * Reverse digits: rev(n, ans) → O(log n)
  * Count digits: countDigits(n) → O(log n)
  * Palindrome check: isPalindrome(n, rev) → O(log n)
  * Fibonacci number: fib(n) → O(2ⁿ) naive / O(n) DP
  * GCD: gcd(a,b) → O(log max(a,b))
  * Subsequences / k-digit numbers → O(2^digits) / O(10^k)
- Recursion on Arrays
  * Forward traversal → fun(arr, i, n) → O(n)
  * Backward traversal → fun(arr, n) → O(n)
  * Sum / Product → sum(arr, i) / product(arr, i) → O(n)
  * Max / Min element → max(arr, i) → O(n)
  * Search element → search(arr, i, key) → O(n)
  * Count occurrences → count(arr, i, key) → O(n)
  * Subsets / Subsequences → subsets(arr, curr, i) → O(2^n)
  * Permutations → permute(arr, used, curr) → O(n!)
- Recursion on Strings
  * Traversal (forward/backward) → O(n)
  * Reverse string → O(n²) naive / O(n) optimized
  * Count characters / occurrences → O(n)
  * Remove / Replace characters → O(n²) naive / O(n) optimized
  * Palindrome check → O(n)
  * Subsequences / Subsets → O(2^n)
  * Permutations → O(n × n!)
  * String → Integer parsing → O(n)
  * Encoding / Decoding → O(2^n) worst-case
- Recursion on 2D Arrays
  * Traversal row-wise → traverse(mat, i, j) → O(m*n)
  * Sum of elements → sumMatrix(mat, i, j) → O(m*n)
  * Search element → searchMatrix(mat, i, j, key) → O(m*n)
  * Row/Column max → rowMax(mat, i, j) → O(m*n)
  * Pathfinding / Maze → O(4^(m*n)) worst-case
  * Print all paths (right/down) → O(2^(m+n))
  * Count paths → O(2^(m+n))
  * Flood fill / DFS → O(m*n)
  * Spiral / Zig-Zag traversal → O(m*n)
- Recursion on ArrayList
  * Forward / Backward traversal → O(n)
  * Sum / Product → O(n)
  * Search / Max / Min → O(n)
  * Remove element recursively → O(n²) worst-case
  * Subsets / Subsequences → O(2^n)
  * Permutations → O(n × n!)
  * Sum with condition (even/odd) → O(n)
  * Reverse ArrayList → O(n)
  * Remove duplicates → O(n²)
- General Notes
  * Always define base case and changing variable
  * Visualize recursion as stack of calls
  * Branching → number of calls = (number of branches) ^ (depth)
  * Avoid excessive string/ArrayList modifications inside recursion
  * Backtracking requires state restoration
---

<!-- #endregion -->
