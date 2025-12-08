<!-- #region 97-Count subarrays with given sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q97: Count subarrays with given sum</h1>

## 1. Problem Understanding

- You are given an integer array nums and an integer k.
- You need to count all subarrays whose sum equals k.
- Subarrays are contiguous sequences within the array.
---

## 2. Constraints

- 1 <= nums.length <= 10^5 → implies O(n²) brute force may TLE, we need O(n).
- -1000 <= nums[i] <= 1000 → elements can be negative.
- -10^7 <= k <= 10^7.
---

## 3. Edge Cases

- Array contains negative numbers → prefix sum with hashmap works.
- Array has all zeros, k = 0 → multiple subarrays sum to 0.
- Single element array equal to k.
---

## 4. Examples

```text
Input: [1, 1, 1], k = 2 → Output: 2 → [1,1], [1,1]

Input: [1, 2, 3], k = 3 → Output: 2 → [1,2], [3]

Input: [3, 1, 2, 4], k = 6 → Output: 1 → [2,4]
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check all possible subarrays and sum them.

**Steps:**
- Initialize count = 0
- Loop i from 0 to n-1
- Loop j from i to n-1
- Sum nums[i..j], if sum == k → count++

**Java Code:**
```java
public int subarraySum(int[] nums, int k) {
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        int sum = 0;
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
            if (sum == k) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n²)
- 💾 Space Complexity:
  * O(1)

### Approach 2: Prefix Sum + HashMap (Optimized) ✅

**Idea:**
- Use a prefix sum and store counts of prefix sums in a hashmap.
- For each new prefix sum currSum, check if (currSum - k) exists in the map → add its frequency to result.

**Steps:**
- Initialize map = {0:1}, sum = 0, count = 0
- Loop over each element in nums:
- sum += nums[i]
- If (sum - k) exists in map → count += map.get(sum - k)
- map[sum] = map.getOrDefault(sum, 0) + 1

**Java Code:**
```java
import java.util.HashMap;

public class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);  // base case
        int sum = 0, count = 0;
        for (int num : nums) {
            sum += num;
            if (map.containsKey(sum - k)) {
                count += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n) → single pass over array
- 💾 Space Complexity:
  * O(n) → hashmap storing prefix sums

---

## 6. Justification / Proof of Optimality

- The hashmap keeps track of how many times a prefix sum occurred.
- sum - k exists → there exists a previous prefix sum that forms a subarray summing to k.
---

## 7. Variants / Follow-Ups

- Count subarrays with sum ≤ k → need sliding window for positive numbers.
- Longest subarray with sum = k → similar prefix sum + hashmap, store first index.
---

## 8. Tips & Observations

- Always initialize map.put(0,1) → handles subarrays starting from index 0.
- Works for negative numbers too.
- For longest subarray, store first occurrence of each prefix sum.
---

<!-- #endregion -->
