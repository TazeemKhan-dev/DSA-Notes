<!-- #region 96-Longest Subarray with Sum K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q96: Longest Subarray with Sum K</h1>

## 1. Problem Understanding

- You are given an array nums of integers and a target sum k.
- You need to find the length of the longest contiguous subarray whose sum equals k.
- If no such subarray exists, return 0.
---

## 2. Constraints

- 1 <= n <= 10⁵
- -10⁵ <= nums[i] <= 10⁵
- -10⁹ <= k <= 10⁹
---

## 3. Edge Cases

- All numbers are positive → sliding window approach works.
- Array contains negative numbers → need prefix sum + HashMap.
- k = 0 (check for subarrays summing to zero).
- No subarray equals k → return 0.
---

## 4. Examples

```text
Input: nums = [10, 5, 2, 7, 1, 9], k = 15
Output: 4
Explanation: [5, 2, 7, 1] has sum = 15.
```

---

## 5. Approaches

### Approach 1: Brute Force (O(n²))

**Idea:**
- Try every possible subarray and compute its sum.
- Keep track of the longest one equal to k.

**Steps:**
- Use two loops — outer for start index, inner for end index.
- Compute sum for each subarray.
- If sum == k → update longest length.

**Java Code:**
```java
int longestSubarraySumK(int[] nums, int k) {
    int n = nums.length;
    int maxLen = 0;
    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += nums[j];
            if (sum == k)
                maxLen = Math.max(maxLen, j - i + 1);
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n²) → nested loops
  * For large n (10⁵), not efficient
- 💾 Space Complexity
  * O(1)

### Approach 2: Prefix Sum + HashMap (Optimized O(n))

**Idea:**
- Use a prefix sum to keep track of cumulative sums, and store the first occurrence of each prefix in a HashMap.
- If prefixSum - k exists in map → a subarray with sum k exists from that index to current.

**Steps:**
- Initialize sum = 0, maxLen = 0, and a HashMap<Integer, Integer> prefixMap.
- For each element:
  * Add it to sum.
  * If sum == k → update maxLen.
  * If sum - k exists in map → subarray found, update maxLen.
  * If sum not in map → store it with current index (only first occurrence).
- Return maxLen.

**Java Code:**
```java
int longestSubarraySumK(int[] nums, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int sum = 0, maxLen = 0;

    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];

        if (sum == k) maxLen = i + 1;

        if (map.containsKey(sum - k)) {
            maxLen = Math.max(maxLen, i - map.get(sum - k));
        }

        if (!map.containsKey(sum)) {
            map.put(sum, i);
        }
    }

    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) → single pass with O(1) HashMap lookups
- 💾 Space Complexity
  * O(n) → HashMap storing prefix sums

### Approach 3: Sliding Window (Only for Non-Negative Numbers)

**Idea:**
- When all elements are non-negative, we can use a sliding window to expand/shrink the subarray.

**Steps:**
- Maintain two pointers l and r, and a running sum.
- Expand r to increase sum.
- If sum > k → move l forward.
- If sum == k → update maxLen.

**Java Code:**
```java
int longestSubarraySumK(int[] nums, int k) {
    int left = 0, right = 0, sum = 0, maxLen = 0;
    int n = nums.length;

    while (right < n) {
        sum += nums[right];

        while (sum > k) {
            sum -= nums[left];
            left++;
        }

        if (sum == k) {
            maxLen = Math.max(maxLen, right - left + 1);
        }

        right++;
    }

    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) → each element added/removed once
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- The HashMap + Prefix Sum approach is the most optimal because:
  * It handles both positive and negative numbers.
  * It gives O(n) time complexity.
---

## 7. Variants / Follow-Ups

- Count of subarrays with sum K (instead of longest length).
- Subarray sum divisible by K
- Longest subarray with sum ≤ K
---

## 8. Tips & Observations

- Always store first occurrence of prefix sum — it gives longest subarray.
- Resetting map or overwriting sums can shorten valid subarrays.
- For strictly positive arrays → sliding window is even simpler and faster.
---

<!-- #endregion -->
