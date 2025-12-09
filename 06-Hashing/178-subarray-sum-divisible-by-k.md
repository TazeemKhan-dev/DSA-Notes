<!-- #region 178-Subarray sum divisible by k -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q178: Subarray sum divisible by k</h1>

## 1. Problem Statement

- You are given an integer array nums and an integer k.
- Find the count of non-empty subarrays whose sum is divisible by k.
- A subarray is a contiguous portion of the array.
---

## 2. Problem Understanding

- We need to count how many subarrays (i…j) satisfy:
  * (sum from i to j) % k == 0
- A direct brute force would check all possible (i, j) pairs, but that is too slow for n = 5000 (≈12.5 million checks).
- We need an efficient way using prefix sums + remainder frequency.
---

## 3. Constraints

- n ≤ 5000
- nums[i] can be negative
- k ≥ 2
- Must run faster than O(n²) ideally.
---

## 4. Edge Cases

- k divides 0 → a prefix sum being exactly divisible by k counts a subarray.
- Negative numbers → remainders can be negative → must normalize.
- Single element divisible by k.
---

## 5. Examples

```text
Example 1

Input:

6 5
4 5 0 -2 -3 1


Output: 7

Example 2

Input:

4 2
4 5 0 -2


Output: 4
```

---

## 6. Approaches

### Approach 1: Brute Force (O(n³))

**Idea:**
- Enumerate every (i, j) pair.
- Compute sum for each subarray.
- Check if divisible by k.

**Steps:**
- For each start index i
- For each end index j >= i
- Compute sum inside inner loop
- Check divisibility

**Java Code:**
```java
public int subarraysDivByK(int[] nums, int k) {
    int n = nums.length;
    int count = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int sum = 0;
            for (int t = i; t <= j; t++) {
                sum += nums[t];
            }
            if (sum % k == 0) count++;
        }
    }
    return count;
}
```

**💭 Intuition Behind the Approach:**
- Very direct: try all subarrays and compute each sum manually.
- But extremely slow.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n³)
  * Sum recomputed many times.
- 💾 Space Complexity
  * O(1)

### Approach 2: Prefix Sum Optimization (O(n²))

**Idea:**
- Instead of recomputing sums, use prefix sum:
  * sum(i…j) = prefix[j] - prefix[i-1]
- Then check if divisible by k.

**Steps:**
- Build prefix-sum array
- Enumerate (i, j) pairs
- Compute sum = prefix[j] − prefix[i-1]
- Check divisibility

**Java Code:**
```java
public int subarraysDivByK(int[] nums, int k) {
    int n = nums.length;
    int[] pref = new int[n];
    pref[0] = nums[0];

    for (int i = 1; i < n; i++) {
        pref[i] = pref[i - 1] + nums[i];
    }

    int count = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {

            int sum = pref[j] - (i > 0 ? pref[i - 1] : 0);

            if (sum % k == 0) count++;
        }
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Avoid repeated sum calculation by caching sums up to each index.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n²)
  * Why?
  * Every (i, j) pair is checked once.
- 💾 Space Complexity
  * O(n) for prefix array.

### Approach 3: Prefix Remainder Frequency (Optimal — O(n))

**Idea:**
- If two prefix sums have the same remainder mod k, the subarray between them has sum divisible by k.
- We maintain:
  * freq[remainder] = count of prefix sums with that remainder
- When a remainder repeats, new valid subarrays form.

**Steps:**
- Initialize:
  * freq[0] = 1 (handles subarrays starting at index 0)
  * sum = 0, count = 0
- For each number:
  * sum += num
  * rem = (sum % k + k) % k (handle negatives)
  * Add freq[rem] to answer (those many subarrays end here)
  * Increment freq[rem]

**Java Code:**
```java
public int subarraysDivByK(int[] nums, int k) {
    HashMap<Integer, Integer> freq = new HashMap<>();
    freq.put(0, 1);

    int sum = 0;
    int count = 0;

    for (int num : nums) {
        sum += num;

        int rem = sum % k;
        if (rem < 0) rem += k;

        if (freq.containsKey(rem)) {
            count += freq.get(rem);
        }

        freq.put(rem, freq.getOrDefault(rem, 0) + 1);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- If:
  * prefix[j] % k == prefix[i-1] % k
- Then:
  * (prefix[j] - prefix[i-1]) % k == 0
- Meaning the subarray (i…j) is divisible by k.
- We only need to count matching remainders.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
    * Single pass, hash lookups are O(1).
- 💾 Space Complexity
  * O(k) or O(n) (depending on distribution of remainders).

---

## 7. Justification / Proof of Optimality

- The optimal approach is correct due to the mathematical fact:
  * (a % k == b % k)  →  (a - b) % k == 0
- Prefix sums convert subarray sum checking to matching remainders.
- Remainder frequency allows counting all valid subarrays ending at the current index in constant time.
- This reduces the problem from checking O(n²) subarrays to processing remainders in O(n).
---

## 8. Variants / Follow-Ups

- Variant 1 — Longest subarray with sum divisible by k
  * Track first occurrence of each remainder to compute max length.
- Variant 2 — Count subarrays with sum equal to k
  * Use:
  * sum - k = existing prefix sum
- Variant 3 — Count subarrays with sum divisible by k but also bounded
  * Use sliding window + mod tricks.
- Variant 4 — Subarray sums divisible by k with negative numbers
  * Same technique but requires remainder normalization.
- Variant 5 — Count subarrays divisible by multiple mod values
  * Maintain maps for each k.
---

## 9. Tips & Observations

- Normalizing remainder with:
  * rem = (sum % k + k) % k
- is extremely important with negative numbers.
- Always initialize:
  * freq[0] = 1
- to allow subarrays starting at index 0.
- Remainder frequency tricks show up in many problems (equal 0s & 1s, sums divisible by k, etc.).
- HashMap works better than array when k is large.
- If all nums are multiples of k → every subarray works.
---

## 10. Pitfalls

- Forgetting remainder normalization → WRONG answers.
- Missing initialization of freq[0].
- Using brute force on large n → will timeout.
- Treating subsequences like subarrays (order matters!).
- Using array remainder frequency when k is very large → memory issues.
---

<!-- #endregion -->
