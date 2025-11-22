<!-- #region 136-Square root of a number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q136: Square root of a number</h1>

## 1. Problem Understanding

- You are given a number x.
- You must return:
- If x is a perfect square → its exact square root
- Otherwise → floor(√x)
- You must not use the built-in sqrt() function.
- Time complexity must be O(log N), which hints binary search.
---

## 2. Constraints

- 1 ≤ x ≤ 10^7
- Input is a single integer
- Output must be an integer (floor value)
---

## 3. Edge Cases

- x = 1 → answer = 1
- Very small values: x = 2, 3 → answer = 1
- Large inputs up to 10^7
- Perfect squares: 4, 9, 16, 25 → return exact root
- Overflow of mid * mid → use long to prevent overflow
---

## 4. Examples

```text
Example 1
Input: 5
Output: 2

Example 2
Input: 36
Output: 6

Example 3
Input: 2
Output: 1
```

---

## 5. Approaches

### Approach 1: Brute Force Approach (Linear Search)

**Idea:**
- Try every number from 1 to x and check the last i such that i*i ≤ x.

**Steps:**
- Loop from i=1 to i*i ≤ x.
- Track the last valid number.
- Return it.

**Java Code:**
```java
public static int mySqrt(int x) {
    int ans = 0;
    for (long i = 1; i * i <= x; i++) {
        ans = (int) i;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Directly simulate what square root means.
- Simple but slow.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(√N)
  * Because loop runs until i*i ≤ x.
- 💾 Space Complexity
  * O(1)
  * Only counters used.

### Approach 2: Using Math Properties (Decreasing Search Range)

**Idea:**
- Instead of checking all numbers from 1 to x, we can optimize by stopping early once i*i crosses x.
- (Slightly better than brute but still worst-case √N)

**Steps:**
- Same as brute but break early.

**Java Code:**
```java
public static int mySqrt(int x) {
    long i = 1;
    while (i * i <= x) i++;
    return (int)(i - 1);
}
```

**💭 Intuition Behind the Approach:**
- Stop as soon as you've gone past the root.
- Still linear in √N but less work than brute.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(√N)
- 💾 Space Complexity
  * O(1)

### Approach 3: Binary Search (Required O(log N))

**Idea:**
- The real square root lies between 1 and x.
- Binary search this space for the greatest mid such that:
- mid * mid ≤ x

**Steps:**
- Let low = 1, high = x
- Compute mid
- If mid*mid ≤ x, store it and move right (search for bigger)
- Else move left
- Return last stored answer

**Java Code:**
```java
public static int mySqrt(int x) {
    if (x == 0) return 0;

    long s = 1;
    long e = x;
    long ans = 1;

    while (s <= e) {
        long mid = (s + e) / 2;

        if (mid * mid <= x) {
            ans = mid;      // mid is valid
            s = mid + 1;    // try for bigger
        } else {
            e = mid - 1;    // mid too big
        }
    }
    return (int) ans;
}
```

**💭 Intuition Behind the Approach:**
- Square root grows slowly, so binary search reduces the search space by half every step.
- We want the largest number whose square is ≤ x → therefore move right on valid mid.
- This ensures correctness for both perfect and non-perfect squares.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log N)
  * Because binary search repeatedly halves the range.
- 💾 Space Complexity
  * O(1)
  * Only variables used.

---

## 6. Justification / Proof of Optimality

- Binary search is optimal because the search range is monotonic (i*i increases as i increases).
- Unlike brute force (√N steps), binary search does it extremely fast in log N steps.
- Works well for values up to 10^7 easily within constraints.
---

## 7. Variants / Follow-Ups

- Calculate ceil of √x
- Return true/false if x is a perfect square
- Return √x without using multiplication (Newton’s Method)
- √x for long or big integers
---

## 8. Tips & Observations

- Always use long for mid * mid to avoid integer overflow.
- When searching for largest valid, use:
  * if (mid*mid <= x) → move right
- For a perfect square, binary search will naturally find it.
- Classic question in binary search category — must be mastered.
---

<!-- #endregion -->
