<!-- #region 74-Climbing Stairs -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q74: Climbing Stairs</h1>

## 1. Problem Understanding

- You are climbing n steps.
- Each move: climb 1 step or 2 steps.
- Find number of distinct ways to reach the top.
---

## 2. Constraints

- 1 <= n <= 45
- Only 1 or 2 steps at a time.
---

## 3. Examples

```text
n = 2 → (1+1), (2) → 2 ways

n = 3 → (1+1+1), (1+2), (2+1) → 3 ways
```

---

## 4. Approaches

### Approach 1: Recursive (Brute Force)

**Idea:**
- Number of ways to reach step n = ways to reach n-1 + ways to reach n-2.
- n == 0 → return 1 (reached top)
- n < 0 → return 0 (invalid path)

**Java Code:**
```java
public int ClimbingStairs(int n) {
    if (n == 0) return 1;
    if (n < 0) return 0;
    return ClimbingStairs(n - 1) + ClimbingStairs(n - 2);
}
```

**Complexity (Time & Space):**
- Time Complexity: O(2^n)
- Space Complexity: O(n) (recursion stack)

### Approach 2: Recursive + Memoization (Top-Down DP)

**Idea:**
- Store results for each n to avoid recomputation.

**Java Code:**
```java
public int ClimbingStairsMemo(int n, int[] dp) {
    if (n == 0) return 1;
    if (n < 0) return 0;
    if (dp[n] != -1) return dp[n];
    dp[n] = ClimbingStairsMemo(n-1, dp) + ClimbingStairsMemo(n-2, dp);
    return dp[n];
}
```

**Complexity (Time & Space):**
- Time Complexity: O(n)
- Space Complexity: O(n)

### Approach 3: Iterative DP (Bottom-Up)

**Idea:**
- Build dp[i] = dp[i-1] + dp[i-2] for all i = 2 to n.

**Java Code:**
```java
public int ClimbingStairsDP(int n) {
    if (n == 1) return 1;
    int[] dp = new int[n+1];
    dp[0] = 1; dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
```

**Complexity (Time & Space):**
- Time Complexity: O(n)
- Space Complexity: O(n)

### Approach 4: Optimized Iterative (Space O(1))

**Idea:**
- Only keep last two results.

**Java Code:**
```java
public int ClimbingStairsOptimized(int n) {
    if (n == 1) return 1;
    int a = 1, b = 1;
    for (int i = 2; i <= n; i++) {
        int temp = a + b;
        a = b;
        b = temp;
    }
    return b;
}
```

**Complexity (Time & Space):**
- Time Complexity: O(n)
- Space Complexity: O(1)

---

## 5. Justification / Proof of Optimality

- Recursive solution works because each step branches into 1-step and 2-step moves.
- Total ways = sum of ways from n-1 and n-2 → Fibonacci sequence.
---

## 6. Variants / Follow-Ups

- Climb with 1, 2, 3 steps at a time.
- Steps with forbidden steps.
- Minimum steps instead of counting ways.
---

## 7. Tips & Observations

- Recursive brute force is easy but slow → use memoization/DP.
- Recognize Fibonacci pattern.
- DP can be implemented top-down or bottom-up.
---

<!-- #endregion -->
