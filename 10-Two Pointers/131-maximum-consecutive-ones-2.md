<!-- #region 131-Maximum Consecutive Ones 2 -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q131: Maximum Consecutive Ones 2</h1>

## 1. Problem Understanding

- You are given a binary array nums and an integer k.
- You may flip at most k zeroes to ones. Find the maximum length of a contiguous subarray that contains only 1s after at most k flips.
---

## 2. Constraints

- 1 <= nums.length <= 10^5
- 0 <= nums[i] <= 1
- 0 <= k <= nums.length
- Need an O(n) or O(n log n) solution for large n.
---

## 3. Edge Cases

- k == 0: standard maximum consecutive ones (no flips).
- k >= count(0s): you can flip all zeros → answer = n.
- All zeros or all ones arrays.
- n = 1.
- Very large k relative to n.
---

## 4. Examples

```text
Example 1
Input: n=11, k=2
nums = [1,1,1,0,0,0,1,1,1,1,0]
Output: 6
Explanation: flip two zeros to get [1,1,1,0,0,1,1,1,1,1,1] → longest 6.

Example 2
Input: n=4, k=4
nums = [0,0,0,1]
Output: 4
Explanation: flip all zeros → [1,1,1,1].
```

---

## 5. Approaches

### Approach 1: Sliding Window / Two Pointers (Optimal, O(n))

**Idea:**
- Maintain a window [l, r] that contains at most k zeros. Expand r and when zeros exceed k, move l until zeros ≤ k. Track the maximum window length.

**Steps:**
- Initialize l = 0, zeros = 0, ans = 0.
- For r from 0 to n-1:
  * If nums[r] == 0 → zeros++.
  * While zeros > k: if nums[l] == 0 → zeros--; l++.
  * Update ans = max(ans, r - l + 1).
- Return ans.

**Java Code:**
```java
public static int longestOnes(int[] nums, int k) {
    int l = 0;
    int zeros = 0;
    int ans = 0;
    for (int r = 0; r < nums.length; r++) {
        if (nums[r] == 0) zeros++;
        while (zeros > k) {
            if (nums[l] == 0) zeros--;
            l++;
        }
        ans = Math.max(ans, r - l + 1);
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We want the largest contiguous block with ≤ k zeros — that is exactly a variable-length window constrained by a count.
- Expanding r increases candidate length; contracting l maintains feasibility.
- Each index enters and leaves the window at most once → linear time.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) — each r moves once; l moves at most n times.
  * Why: the while loop increments l only when zero count exceeds k; total increments across entire scan ≤ n.
- 💾 Space Complexity
  * O(1) — only counters and pointers.

### Approach 2: Prefix Sum of Zero Indices + Binary Search (O(n log n))

**Idea:**
- Store indices of all zeros.
- To build a window that flips at most k zeros, use the zero-list to quickly compute the farthest right boundary.
- For each zero-index, find the (k+1)-th zero to the right → defines max window

**Steps:**
- Build zeroIdx list of positions where nums[i] == 0.
- If zeroIdx.size() <= k → you can flip all zeros → answer = n.
- Otherwise, for each i:
  * Let leftZero = zeroIdx[i]
  * Let rightZero = zeroIdx[i + k]
  * The max window is between previous zero and next zero boundaries.

**Java Code:**
```java
public static int longestOnesPrefix(int[] nums, int k) {
    ArrayList<Integer> zeros = new ArrayList<>();

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) zeros.add(i);
    }

    if (zeros.size() <= k) return nums.length;

    int ans = 0;

    for (int i = 0; i + k < zeros.size(); i++) {
        int leftZero = zeros.get(i);
        int rightZero = zeros.get(i + k);

        int leftBoundary = (i == 0) ? 0 : zeros.get(i - 1) + 1;
        int rightBoundary = (i + k == zeros.size() - 1) ? nums.length - 1 : zeros.get(i + k + 1) - 1;

        ans = Math.max(ans, rightBoundary - leftBoundary + 1);
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- The array becomes divided by zeros.
- Flipping k consecutive zeros means we take a window between the (i-1)-th zero and (i+k+1)-th zero.
- Zero positions give perfect left/right window boundaries.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Collecting zeros: O(n)
  * Looping over zeros: O(z) where z = number of zeros
  * Total: O(n)
- 💾 Space Complexity
  * O(z) to store zero indices.

### Approach 3: Brute Force (O(n^2), For understanding only)

**Idea:**
- For each start i, expand j and count zeros, stop when zeros exceed k, update maximum length.

**Java Code:**
```java
public static int longestOnesBrute(int[] nums, int k) {
    int ans = 0;

    for (int i = 0; i < nums.length; i++) {
        int zeros = 0;

        for (int j = i; j < nums.length; j++) {
            if (nums[j] == 0) zeros++;
            if (zeros > k) break;

            ans = Math.max(ans, j - i + 1);
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Expands every possible window — explores all choices.
- Too slow for large input but good for conceptual understanding.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n^2)
  * Why: nested loops.
- 💾 Space Complexity
  * O(1).

---

## 6. Justification / Proof of Optimality

- Sliding window is the optimal solution because it naturally models “at most k bad items”.
- Prefix-zero-index solution is useful when analyzing zero boundaries or performing k-group constraints.
- Brute force only for learning correctness pattern.
---

## 7. Variants / Follow-Ups

- Flip exactly k zeros, not at most.
- Longest substring with at most k replacements (string version).
- Max consecutive ones with cost-per-flip scenarios.
- Generalized to non-binary arrays using sum ≤ k constraints.
---

## 8. Tips & Observations

- Anytime you hear “flip at most k items” → sliding window with a count.
- Zeros act as boundaries; analyzing their positions gives alternate solutions.
- When k is 0, the sliding window collapses to simple consecutive ones counting.
---

<!-- #endregion -->
