<!-- #region 148-Find square root of a number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q148: Find square root of a number</h1>

## 1. Problem Understanding

- You are given a non-negative integer n.
- You must return its square root, but if the number is not a perfect square, return the floor value of sqrt(n).
- Example:
  * 36 → 6
  * 28 → floor(5.29…) = 5
  * This is a classic Binary Search on Answer problem.
---

## 2. Constraints

- 0 <= n <= 2^31 - 1
- Large input → cannot use floating operations directly
- Answer fits in int (floor of sqrt always ≤ 46340 for 32-bit)
---

## 3. Edge Cases

- n = 0 → output 0
- n = 1 → output 1
- Very large value like 2^31 - 1
- Perfect squares like 49, 121
- Non-perfect squares like 50, 90
---

## 4. Examples

```text
🧪 Examples
Example 1

Input: 36
Output: 6

Example 2

Input: 28
Output: 5

Example 3

Input: 50
Output: 7
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all x from 1 upward until x*x > n.
- The previous x is the floor sqrt.

**Steps:**
- Loop from 1 to n
- Stop when i*i > n

**Java Code:**
```java
public int mySqrt(int n) {
    int i = 1;
    while (i * 1L * i <= n) {
        i++;
    }
    return i - 1;
}
```

**💭 Intuition Behind the Approach:**
- You keep guessing increasing numbers until you cross n.

**Complexity (Time & Space):**
- Time: O(√n) (because loop runs √n times)
- Space: O(1)

### Approach 2: Binary Search Binary Search (l <= h Classic Search Style)

**Idea:**
- This version explicitly checks if mid*mid == n.
- If not, it finds the floor where mid*mid < n.

**Steps:**
- Use l <= h
- If mid*mid == n → return mid
- If mid*mid < n → answer may be mid, go right
- Else → go left

**Java Code:**
```java
public int mySqrt(int n) {
    if (n == 0 || n == 1) return n;

    int low = 1, high = n;
    int ans = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        long sq = (long) mid * mid;

        if (sq == n) return mid;

        if (sq < n) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We do a classic target search:
  * If mid² equals n → perfect square
  * Else keep the last valid mid as candidate (floor sqrt)

**Complexity (Time & Space):**
- Time: O(log n)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Binary search is valid because the function:
  * f(x) = x*x
- is monotonically increasing.
- So we can search for the boundary where:
  * x*x <= n is true
  * x*x > n is false
- This monotonic structure allows binary search.
---

## 7. Variants / Follow-Ups

- Find cube root (floor)
- Find sqrt using Newton's Method
- Integer sqrt for 64-bit values
- Check if n is a perfect square
- Floating point sqrt with precision
---

## 8. Tips & Observations

- ALWAYS cast to long when checking mid*mid to avoid overflow
- l < h = shrink range → stop at first mid*mid > n
- l <= h = classic search → check equality
- Floor sqrt is always ≤ 46340 for 32-bit integers
- In competitive coding, (l < h) version is preferred
- In interviews, (l <= h) is easier to explain
---

<!-- #endregion -->
