<!-- #region 0-Recursion on Integer -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Recursion on Integer</h1>

## 1. Problem Understanding

- Recursion on integers means performing recursive operations where the changing variable is an integer — typically n, representing size, count, or numerical value.
- Each recursive call usually reduces or divides n until reaching a base case (e.g., n == 0 or n == 1).
---

## 2. Constraints

- Must have a base case to prevent infinite recursion.
- Integer parameter should change in every recursive call.
- Recursion depth ≤ value of n (or log(n) if divided each time).
---

## 3. Edge Cases

- n = 0 or n = 1 (most base cases).
- Negative integers (usually invalid unless explicitly handled).
- Large n may cause StackOverflowError (too deep recursion).
---

## 4. Examples

```text
Print numbers 1 → n

Print numbers n → 1

Sum of first n numbers

Factorial n!

Reverse digits

Count digits

Fibonacci numbers

GCD of two numbers
```

---

## 5. Approaches

### Approach 1: Printing / Counting Numbers

**Java Code:**
```java
a) 1 → n
void print1ToN(int n) {
    if (n == 0) return;
    print1ToN(n - 1);
    System.out.print(n + " ");
}


b) n → 1
void printNTo1(int n) {
    if (n == 0) return;
    System.out.print(n + " ");
    printNTo1(n - 1);
}


c) Count numbers in a range
int countRange(int start, int end) {
    if (start > end) return 0;
    return 1 + countRange(start + 1, end);
}
```

**Complexity (Time & Space):**
- a) print1ToN(int n)
  * ⏱ Time: O(n)
  * 💾 Space: O(n)
- b) printNTo1(int n)
  * ⏱ Time: O(n)
  * 💾 Space: O(n)
- c) countRange(int start, int end)
  * ⏱ Time: O(end − start + 1) ≈ O(n)
  * 💾 Space: O(n)

### Approach 2: Sum / Product

**Java Code:**
```java
a) Sum of first n numbers
int sumN(int n) {
    if (n == 0) return 0;
    return n + sumN(n - 1);
}


b) Sum of digits
int sumDigits(int n) {
    if (n == 0) return 0;
    return (n % 10) + sumDigits(n / 10);
}


c) Product of digits
int productDigits(int n) {
    if (n == 0) return 1;
    return (n % 10) * productDigits(n / 10);
}


d) Sum of squares
int sumSquares(int n) {
    if (n == 0) return 0;
    return n * n + sumSquares(n - 1);
}
```

**Complexity (Time & Space):**
- a) sumN(int n)
  * ⏱ Time: O(n)
  * 💾 Space: O(n)
- b) sumDigits(int n)
  * ⏱ Time: O(log₁₀ n) → proportional to number of digits
  * 💾 Space: O(log₁₀ n)
- c) productDigits(int n)
  * ⏱ Time: O(log₁₀ n)
  * 💾 Space: O(log₁₀ n)
- d) sumSquares(int n)
  * ⏱ Time: O(n)
    * 💾 Space: O(n)

### Approach 3: Factorial / Power

**Java Code:**
```java
a) Factorial
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}


b) Power
long power(long x, long n) {
    if (n == 0) return 1;
    return x * power(x, n - 1);
}


c) Power Optimized (Exponentiation by Squaring)
long power(long x, long n) {
    if (n == 0) return 1;
    long half = power(x, n / 2);
    if (n % 2 == 0) return half * half;
    else return x * half * half;
}


d) Power with modulo
long powerMod(long x, long n, long m) {
    if (n == 0) return 1;
    long half = powerMod(x, n / 2, m);
    long res = (half * half) % m;
    if (n % 2 != 0) res = (res * x) % m;
    return res;
}
```

**Complexity (Time & Space):**
- a) factorial(int n)
- ⏱ Time: O(n)
- 💾 Space: O(n)
- b) power(long x, long n)
  * ⏱ Time: O(n)
  * 💾 Space: O(n)
- c) powerOptimized(long x, long n) (Exponentiation by Squaring)
  * ⏱ Time: O(log n)
  * 💾 Space: O(log n)
- d) powerMod(long x, long n, long m)
  * ⏱ Time: O(log n)
  * 💾 Space: O(log n)

### Approach 4: Reverse / Digit Operations

**Java Code:**
```java
Reverse digits
int rev(int n, int ans) {
    if (n == 0) return ans;
    return rev(n / 10, ans * 10 + n % 10);
}
// call: rev(1234, 0)


b) Count digits
int countDigits(int n) {
    if (n == 0) return 0;
    return 1 + countDigits(n / 10);
}


c) Count digits with property (even/odd)
int countEvenDigits(int n) {
    if (n == 0) return 0;
    int count = (n % 10) % 2 == 0 ? 1 : 0;
    return count + countEvenDigits(n / 10);
}


d) Check palindrome
boolean isPalindrome(int n, int rev) {
    if (n == 0) return true;
    return isPalindrome(n / 10, rev * 10 + n % 10);
}
```

**Complexity (Time & Space):**
- ⏱ Time: O(log₁₀ n)
- 💾 Space: O(log₁₀ n)

### Approach 5: Fibonacci / Sequence

**Java Code:**
```java
Fibonacci number
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}


b) Print Fibonacci sequence
void printFib(int a, int b, int n) {
    if (n == 0) return;
    int c = a + b;
    System.out.print(c + " ");
    printFib(b, c, n - 1);
}
```

**Complexity (Time & Space):**
- a) fib(int n)
  * ⏱ Time: O(2ⁿ)
  * 💾 Space: O(n)
- b) printFib(int a, int b, int n)
  * ⏱ Time: O(n)
  * 💾 Space: O(n)

### Approach 6: GCD / LCM

**Java Code:**
```java
a) GCD (Euclid’s Algorithm)
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}


b) LCM using GCD
int lcm(int a, int b) {
    return (a * b) / gcd(a, b);
}
```

**Complexity (Time & Space):**
- ⏱ Time: O(log(min(a, b)))
- 💾 Space: O(log n)

### Approach 7: Integer Backtracking / Combinatorics

**Java Code:**
```java
a) Generate all subsequences of a number
void subsequences(int n, int curr) {
    if (n == 0) {
        System.out.println(curr);
        return;
    }
    // include last digit
    subsequences(n / 10, curr * 10 + n % 10);
    // exclude last digit
    subsequences(n / 10, curr);
}


b) Generate all k-digit numbers
void printKDigits(int n, int curr) {
    if (n == 0) {
        System.out.println(curr);
        return;
    }
    for (int i = 0; i <= 9; i++) {
        printKDigits(n - 1, curr * 10 + i);
    }
}
```

**Complexity (Time & Space):**
- a) subsequences(int n, int curr)
  * ⏱ Time: O(2ᵈ) (where d = number of digits)
  * 💾 Space: O(d)
- b) printKDigits(int n, int curr)
  * ⏱ Time: O(10ⁿ) (each level has 10 branches)
  * 💾 Space: O(n)

### Approach 8: Special Patterns

**Java Code:**
```java
a) Reverse printing range
void reverseRange(int start, int end) {
    if (start > end) return;
    reverseRange(start + 1, end);
    System.out.print(start + " ");
}


b) Odd / Even numbers in range
void printEven(int n) {
    if (n == 0) return;
    printEven(n - 1);
    if (n % 2 == 0) System.out.print(n + " ");
}
```

**Complexity (Time & Space):**
- a) reverseRange(int start, int end)
  * ⏱ Time: O(end − start + 1) ≈ O(n)
  * 💾 Space: O(n)
- b) printEven(int n)
  * ⏱ Time: O(n)
  * 💾 Space: O(n)

---

## 6. Justification / Proof of Optimality

- Each call reduces the integer, approaching base case.
- Can be linear recursion (O(n)) or branching recursion (subsequence, combinatorial).
- Extra parameters are useful to accumulate results (sum, reversed number, current number).
- Forms the foundation for arrays, strings, trees, and DP recursion problems.
---

## 7. Variants / Follow-Ups

- Sum/product of odd/even digits
- Count numbers with specific digit
- Generate all numbers in a range satisfying a property
- Fibonacci using memoization for efficiency
- Factorial / Power using iterative + recursive combination
---

## 8. Tips & Observations

- Base case is critical. Always define clearly.
- For ascending order, recurse first, then process.
- For descending order, process first, then recurse.
- For branching recursion, number of calls = (branches)^depth.
- Visualize stack frames for debugging.
---

<!-- #endregion -->
