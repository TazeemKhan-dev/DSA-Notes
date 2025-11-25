<!-- #region 149-Find Nth root of a number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q149: Find Nth root of a number</h1>

## 1. Problem Understanding

- You're given two integers:
  * N → the root to find
  * M → the number
  * You must find an integer X such that:
  * X^N == M
- If such integer X exists → return it.
- If no such integer exists → return -1.
- Example:
  * 3rd root of 27 = 3
  * 4th root of 69 ≠ integer → return -1
  * 4th root of 81 = 3
- This is a classic Binary Search on Answer problem.
---

## 2. Constraints

- 1 <= N <= 30
- 1 <= M <= 10^9
- Answer always lies in [1, M]
- Power X^N can overflow → must use long or safe multiplication
---

## 3. Edge Cases

- N = 1 → answer is always M
- M = 1 → answer is always 1
- Huge values like M = 10^9 and N = 30
- Non-perfect roots (must return -1)
- Overflow while doing mid^N
---

## 4. Examples

```text
Example 1

Input: N=3, M=27
Output: 3

Example 2

Input: N=4, M=69
Output: -1

Example 3

Input: N=4, M=81
Output: 3
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all numbers from 1 to M and compute i^N.
- If match → return i.

**Steps:**
- Loop i from 1 to M
- Compute i^N until it exceeds M

**Java Code:**
```java
public int nthRoot(int N, int M) {
    for (long i = 1; i <= M; i++) {
        long val = 1;
        for (int j = 0; j < N; j++) val *= i;

        if (val == M) return (int)i;
        if (val > M) break;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Sequential check → guaranteed correct but slow.

**Complexity (Time & Space):**
- O(M * N) → too slow
- Space: O(1)

### Approach 2: Binary Search (l < h)

**Idea:**
- We shrink the range [1, M] until only ONE candidate remains.
- At the end, check if low^N == M.

**Steps:**
- low = 1, high = M
- While low < high:
  * Compute mid
  * Compute mid^N using safe multiplication
  * If mid^N < M → go right
  * Else → go left (keep mid)
- After loop:
  * Check if low^N == M
  * If yes return low
  * Else return -1

**Java Code:**
```java
public int nthRoot(int N, int M) {
    int low = 1, high = M;

    while (low < high) {
        int mid = low + (high - low) / 2;

        long val = power(mid, N, M);

        if (val < M) {
            low = mid + 1;
        } else {
            high = mid; // keep mid, shrink left
        }
    }

    return power(low, N, M) == M ? low : -1;
}

// safe power
private long power(long x, int n, int limit) {
    long ans = 1;
    for (int i = 0; i < n; i++) {
        ans *= x;
        if (ans > limit) return ans; // avoid overflow
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We shrink to the smallest number whose power is >= M
- If power equals M → perfect root
- Else no integer root exists

**Complexity (Time & Space):**
- O(N * log M)
- Space: O(1)

### Approach 3: Binary Search (l <= h)

**Idea:**
- Use classic binary search to check if mid^N == M.

**Steps:**
- Use l <= h
- If mid^N == M → return mid
- If mid^N < M → go right
- Else → go left
- If no equality found → return -1

**Java Code:**
```java
public int nthRoot(int N, int M) {
    int low = 1, high = M;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        long val = power(mid, N, M);

        if (val == M) return mid;

        if (val < M) low = mid + 1;
        else high = mid - 1;
    }

    return -1;
}

private long power(long x, int n, int limit) {
    long ans = 1;
    for (int i = 0; i < n; i++) {
        ans *= x;
        if (ans > limit) return ans;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We perform a direct equality search for mid^N = M.
- Binary Search ensures we check the correct mid values only.

**Complexity (Time & Space):**
- O(N * log M)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Binary search is valid because:
- The function f(x) = x^N is strictly increasing
- This means:
  * If x1 < x2 → x1^N < x2^N
- So the search space is monotonic, allowing binary search.
---

## 7. Variants / Follow-Ups

- Find cube root
- Find square root
- Check if number is a perfect power
- Find nth root with precision
- Floating-point nth root using Newton’s Method
---

## 8. Tips & Observations

- Use safe multiplication to avoid overflow
- mid^N grows VERY fast for N = 30
- Prefer (l <= h) approach in interviews
- Prefer (l < h) approach in competitive programming
- Always cap multiplication when > M
- Search space is always [1, M]
---

<!-- #endregion -->
