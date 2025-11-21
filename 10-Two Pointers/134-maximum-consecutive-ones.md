<!-- #region 134-Maximum Consecutive Ones -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q134: Maximum Consecutive Ones</h1>

## 1. Problem Understanding

- You are given a binary array arr of size n.
- Your task is to find the maximum number of consecutive 1s in the array.
---

## 2. Constraints

- 1 <= n <= 10^5
- 0 <= arr[i] <= 1
- Need an O(n) final solution.
---

## 3. Edge Cases

- All zeros → answer is 0
- All ones → answer is n
- Single element array
- Alternating 1s and 0s
- Large n → brute force may be slow
---

## 4. Examples

```text
Example 1
Input: [1, 0, 1, 1]
Output: 2

Example 2
Input: [1, 1, 1]
Output: 3
```

---

## 5. Approaches

### Approach 1: Brute Force (O(n^2))

**Idea:**
- Check every index i, and count how many consecutive 1s start from there.

**Java Code:**
```java
public static int maxConsecutiveOnes(int[] arr) {
    int n = arr.length;
    int ans = 0;

    for (int i = 0; i < n; i++) {
        int count = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 1) count++;
            else break;
        }
        ans = Math.max(ans, count);
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- For each position, simulate starting a consecutive-ones streak.
- Very slow because we repeatedly scan the same segments.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n^2)
  * Because at each i, inner loop expands until a zero or end.
- 💾 Space Complexity
  * O(1)
  * Only counters.

### Approach 2: Linear Scan with Reset (Optimal & Clean) — O(n)

**Idea:**
- Traverse the array once:
  * Maintain a currentCount of continuous ones
  * Reset it when you hit a 0
  * Track the maximum streak in ans

**Java Code:**
```java
public static int maxConsecutiveOnes(int[] arr) {
    int ans = 0;
    int count = 0;

    for (int x : arr) {
        if (x == 1) {
            count++;
            ans = Math.max(ans, count);
        } else {
            count = 0;
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Consecutive ones naturally form segments.
- Whenever you see a zero, a segment ends → reset count.
- Always update maximum during the scan.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) — every element processed once.
- 💾 Space Complexity
  * O(1) — constant counters.

---

## 6. Justification / Proof of Optimality

- The brute force simulates all possible consecutive streaks, which is unnecessary.
- The optimal linear scan captures the nature of the problem: ones form continuous blocks that can be counted in one pass.
- Given n ≤ 10^5, the linear scan is required and the best solution.
---

## 7. Variants / Follow-Ups

- Count maximum consecutive 0s
- Count maximum consecutive 1s with at most one flip (follow-up variant)
- Count longest subarray of equal elements
---

## 8. Tips & Observations

- Most "consecutive ones" problems use either:
  * Simple counter, or
  * Sliding window if flipping or skipping is allowed.
- Resetting counters at boundaries is a common pattern.
---

<!-- #endregion -->
