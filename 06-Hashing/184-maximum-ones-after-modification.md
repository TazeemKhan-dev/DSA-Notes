<!-- #region 184-Maximum Ones After Modification -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q184: Maximum Ones After Modification</h1>

## 1. Problem Statement

- Given a binary array A and an integer B, find the length of the longest contiguous subsegment of 1s possible by changing at most B zeros to ones.
---

## 2. Problem Understanding

- You are allowed to flip at most B zeros in the array to 1.
- After these changes, you must find the longest contiguous subarray consisting of only 1s.
- Important clarifications:
  * Subsegment = contiguous subarray
  * You don’t need to actually modify the array, only compute the maximum length
  * If B is large, you may flip many zeros
- This is a sliding window with constraint problem.
---

## 3. Constraints

- 1 <= N <= 100000
- A[i] is either 0 or 1
- 1 <= B <= 100000
---

## 4. Edge Cases

- Array contains all 1s
- Array contains all 0s
- B >= number of zeros in array
- Single element array
- B = 0 (no flips allowed)
---

## 5. Examples

```text
Example 1

Input:

A = [1, 0, 0, 1, 1, 0, 1]
B = 1


Output:

4

Example 2

Input:

A = [1, 0, 0, 1, 0, 1, 0, 1, 0, 1]
B = 2


Output:

5
```

---

## 6. Approaches

### Approach 1: Brute Force (Try All Subarrays)

**Idea:**
- Check every subarray and count how many zeros it contains.
- If zeros ≤ B, update the maximum length.

**Java Code:**
```java
public static int maxOnesBrute(int[] A, int B) {
    int n = A.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int zeroCount = 0;
        for (int j = i; j < n; j++) {
            if (A[j] == 0) zeroCount++;
            if (zeroCount > B) break;
            maxLen = Math.max(maxLen, j - i + 1);
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(n^2) — checking all subarrays
- Space: O(1) — no extra data structure
- Not feasible for large N.

### Approach 2: Sliding Window (Optimal ✅)

**Idea:**
- Maintain a window with at most B zeros:
  * Expand right
  * Count zeros in current window
  * If zeros exceed B, shrink from left
  * Track maximum window length

**Java Code:**
```java
public static int maxOnes(int[] A, int B) {
    int left = 0, zeroCount = 0, maxLen = 0;

    for (int right = 0; right < A.length; right++) {
        if (A[right] == 0) zeroCount++;

        while (zeroCount > B) {
            if (A[left] == 0) zeroCount--;
            left++;
        }

        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- A valid window can contain at most B zeros
- Sliding window ensures linear traversal
- Shrinking restores validity when constraint breaks
- This pattern is common in “flip at most K” problems

**Complexity (Time & Space):**
- Time: O(n) — each element enters and exits the window once
- Space: O(1) — only counters are used

---

## 7. Justification / Proof of Optimality

- Brute force does not scale
- Sliding window efficiently enforces the zero constraint
- Handles large inputs and edge cases cleanly
- Standard interview solution
---

## 8. Variants / Follow-Ups

- Max consecutive ones with at most K flips
- Longest subarray with at most K zeros
- Binary array sliding window problems
- Replace at most K characters
---

## 9. Tips & Observations

- Count violations (zeros) instead of valid elements (ones)
- Sliding window with constraint is the key idea
- This is identical to “Longest subarray with at most K bad elements”
---

## 10. Pitfalls

- Treating this as a subsequence problem
- Forgetting to shrink when zero count exceeds B
- Resetting window instead of sliding
- Off-by-one errors in window length
---

<!-- #endregion -->
