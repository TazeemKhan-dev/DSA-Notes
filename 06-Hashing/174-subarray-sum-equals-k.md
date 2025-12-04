<!-- #region 174-Subarray sum equals K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q174: Subarray sum equals K</h1>

## 1. Problem Statement

- You are given an integer array nums and an integer k.
- You need to find the total number of contiguous subarrays whose sum is exactly k.
- Return / print that count.
---

## 2. Problem Understanding

- We’re not just checking if any subarray equals k — we must count all such subarrays.
- A subarray is contiguous, i.e., elements must be side-by-side in the original array.
- Numbers can be negative, zero, or positive.
- Because of negatives, classic two-pointer sliding window won't always work (sum can go up and down).
- Example:
  * nums = [1, 1, 1], k = 2
  * Subarrays with sum 2:
  * nums[0..1] = [1,1]
  * nums[1..2] = [1,1]
  * Answer = 2.
- We need an approach that:
  * Handles negatives.
  * Counts all possible subarrays.
  * Works fast enough for up to ~200 elements (brute force is borderline okay but we’ll still do optimal).
---

## 3. Constraints

- 1 <= nums.length <= 2 * 10^8 (given as 208, likely typo, but we’ll treat logically)
- -1000 <= nums[i] <= 1000
- -10^7 <= k <= 10^7
- Given typical constraints for this problem, we should aim for O(N).
---

## 4. Edge Cases

- No subarray sums to k → return 0.
- All elements are zero, k = 0 → many subarrays contribute.
- Negative numbers present → sliding window fails.
- Single-element subarrays: if nums[i] == k, that’s a valid subarray.
- Large positive and negative mix → prefix sum can repeat.
---

## 5. Examples

```text
nums = [1, 1, 1], k = 2
Valid subarrays:

[1, 1] (indices 0–1)

[1, 1] (indices 1–2)
Answer: 2.

nums = [1, 2, 3], k = 3
Valid subarrays:

[1, 2]

[3]
Answer: 2.
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Subarrays)

**Idea:**
- Generate all possible subarrays, compute their sum, and count how many equal k.

**Steps:**
- Loop i from 0 to n-1:
  * sum = 0
  * Loop j from i to n-1:
    * Add nums[j] to sum
    * If sum == k → increment count

**Java Code:**
```java
public int subarraySumBruteForce(int[] nums, int k) {
    int n = nums.length;
    int count = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += nums[j];
            if (sum == k) {
                count++;
            }
        }
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- You literally test every possible subarray. Simple, straightforward, but not efficient for large N.

**Complexity (Time & Space):**
- Time:
  * O(N^2)
  * We check all N*(N+1)/2 subarrays and compute sums on the fly.
- Space:
  * O(1)
  * Only a few variables.

### Approach 2: Prefix Sum + HashMap (Optimal)

**Idea:**
- Use prefix sums and a frequency map.
- Let:
  * prefixSum[i] = sum of elements from index 0 to i.
- For any subarray nums[l..r]:
  * sum(l..r) = prefixSum[r] - prefixSum[l-1]
  * We want sum(l..r) == k
  * → prefixSum[r] - prefixSum[l-1] = k
  * → prefixSum[l-1] = prefixSum[r] - k
- So, at each index r, if we know how many times prefixSum[r] - k has appeared before, that count is the number of subarrays ending at r with sum k.
- We store frequencies of prefix sums in a HashMap<Integer, Integer>:
  * Key: prefix sum value
  * Value: how many times it has occurred
- We must also initialize:
  * map.put(0, 1) → meaning sum 0 has occurred once before starting (empty prefix), to correctly count subarrays starting at index 0.

**Steps:**
- Create map to store prefixSum → frequency.
- Initialize: map.put(0, 1) and currSum = 0, count = 0.
- Iterate over nums:
  * currSum += nums[i]
  * We want currSum - k to have appeared previously.
  * If map contains currSum - k, add map.get(currSum - k) to count.
  * Then, do map.put(currSum, map.getOrDefault(currSum, 0) + 1).
- Return count

**Java Code:**
```java
public int subarraySum(int[] nums, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();
    map.put(0, 1); // prefix sum 0 occurs once initially

    int currSum = 0;
    int count = 0;

    for (int x : nums) {
        currSum += x;

        int need = currSum - k;
        if (map.containsKey(need)) {
            count += map.get(need);
        }

        map.put(currSum, map.getOrDefault(currSum, 0) + 1);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Instead of recomputing sums for each subarray, we track the running sum and see how many previous positions would make a valid subarray ending here.
- The condition:
  * prefixSum[r] - prefixSum[l-1] = k
  * becomes:
  * “How many times have we seen prefixSum[r] - k before?”
- Each such occurrence represents a valid starting point l.

**Complexity (Time & Space):**
- Time:
  * O(N)
  * Single pass through array, each map operation is amortized O(1).
- Space:
  * O(N)
  * In worst case all prefix sums are distinct and stored.

---

## 7. Justification / Proof of Optimality

- Brute force is conceptually fine but O(N^2) and not scalable when N grows.
- Prefix sum + HashMap directly encodes the math relation between subarray sums and prefix sums.
- Works for:
  * Negative numbers
  * Positive numbers
  * Mixed arrays
- Sliding window fails because it assumes non-negative numbers; this problem explicitly allows negatives.
- So the prefix sum + HashMap is the right optimal solution.
---

## 8. Variants / Follow-Ups

- Count subarrays with sum less than k.
- Count subarrays with sum at least k.
- Longest subarray with sum exactly k.
- Subarray sum equals k but only non-negative numbers → sliding window possible
---

## 9. Tips & Observations

- Always initialize map.put(0, 1) for subarray-sum prefix problems; it handles subarrays starting from index 0.
- Prefix sum technique is a pattern; same trick appears in:
  * “Longest subarray with sum k”
  * “Count subarrays with sum divisible by k”
- Use int for prefix sums here because N * max(|nums[i]|) is safe within int range for constraints given. If constraints were bigger, use long.
---

## 10. Pitfalls

- Forgetting to add map.put(0, 1) → you miss subarrays starting at index 0.
- Using sliding window with negative numbers → wrong logic.
- Overwriting frequencies instead of incrementing them.
- Misunderstanding “subarray” as “subset” (non-contiguous).
---

<!-- #endregion -->
