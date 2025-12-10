<!-- #region 182-Longest Subsequence With Difference One -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q182: Longest Subsequence With Difference One</h1>

## 1. Problem Statement

- Given an array nums of size N, find the length of the longest subsequence such that the absolute difference between every adjacent element is exactly 1.
- A subsequence does not need to be contiguous, but order must be preserved.
---

## 2. Problem Understanding

- You need to select elements from the array (keeping order) such that:
  * For every adjacent pair in the subsequence
  * abs(a[i] - a[i-1]) == 1
  * Among all such subsequences, return the maximum possible length
- Important:
  * This is a subsequence, not a subarray
  * Negative numbers are allowed
  * Only the length is required
---

## 3. Constraints

- 1 <= N <= 100000
- -10^9 <= nums[i] <= 10^9
- Large constraints → brute force will not scale.
---

## 4. Edge Cases

- Array with one element → answer is 1
- All elements same → answer is 1
- Strictly increasing by 1
- Strictly decreasing by 1
- Mix of positive and negative values
---

## 5. Examples

```text
Example 1

Input:

[4, 2, 1, 5, 6]


Output:

3


Explanation: {4, 5, 6}

Example 2

Input:

[-2, 2, -1, 1, 0, -1]


Output:

4


Explanation: {-2, -1, 0, -1} or {2, 1, 0, -1}
```

---

## 6. Approaches

### Approach 1: Brute Force (Try All Subsequences)

**Idea:**
- Generate all subsequences and check which ones satisfy:
  * abs(a[i] - a[i-1]) == 1
- Track the maximum length.

**Java Code:**
```java
public static int longestSubseqDiffOneBrute(int[] nums) {
    int n = nums.length;
    int maxLen = 1;

    for (int mask = 1; mask < (1 << n); mask++) {
        int prev = Integer.MIN_VALUE;
        int len = 0;
        boolean valid = true;

        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) != 0) {
                if (prev != Integer.MIN_VALUE &&
                    Math.abs(nums[i] - prev) != 1) {
                    valid = false;
                    break;
                }
                prev = nums[i];
                len++;
            }
        }
        if (valid) maxLen = Math.max(maxLen, len);
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(2^N * N) — every subsequence is generated and validated
- Space: O(1) — ignoring recursion/bitmask overhead
- Not feasible for large N.

### Approach 2: Dynamic Programming + HashMap (Optimal ✅)

**Idea:**
- Let dp[x] represent the length of the longest valid subsequence ending with value x.
- For each number num:
  * It can extend subsequences ending with num - 1 or num + 1
  * So:
    * dp[num] = max(dp[num-1], dp[num+1]) + 1

**Java Code:**
```java
public static int longestSubseqDiffOne(int[] nums) {
    Map<Integer, Integer> dp = new HashMap<>();
    int maxLen = 1;

    for (int num : nums) {
        int left = dp.getOrDefault(num - 1, 0);
        int right = dp.getOrDefault(num + 1, 0);

        int curr = Math.max(left, right) + 1;
        dp.put(num, curr);

        maxLen = Math.max(maxLen, curr);
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- Subsequence only depends on previous values, not positions
- We only care about numbers that differ by ±1
- HashMap allows constant-time lookup for large value ranges
- This is similar to Longest Increasing Subsequence, but value-based

**Complexity (Time & Space):**
- Time: O(N) — each element is processed once
- Space: O(N) — HashMap may store up to N unique values

---

## 7. Justification / Proof of Optimality

- Brute force is exponential and infeasible
- DP + HashMap uses optimal transitions
- Handles large value ranges and negatives
- Widely accepted interview solution
---

## 8. Variants / Follow-Ups

- Brute force is exponential and infeasible
- DP + HashMap uses optimal transitions
- Handles large value ranges and negatives
- Widely accepted interview solution
---

## 9. Tips & Observations

- This is a value-based DP, not index-based
- HashMap is essential due to large number constraints
- Order is preserved automatically by iteration
---

## 10. Pitfalls

- Treating this as a subarray problem
- Forgetting absolute difference (both +1 and -1)
- Using array DP instead of HashMap (value range too large)
- Trying to sort the array (breaks subsequence order)
---

<!-- #endregion -->
