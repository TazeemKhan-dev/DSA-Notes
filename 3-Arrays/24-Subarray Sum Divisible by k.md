<!-- #region 24-Subarray Sum Divisible by k -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q24: Subarray Sum Divisible by k</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array nums and an integer k. We need to find the number of non-empty contiguous subarrays whose sum is divisible by k.
- **Goal:** Count all subarrays nums[i..j] such that (sum(nums[i..j]) % k) == 0.
- **Paraphrase:** Check every possible subarray for divisibility by k.  Optimize using prefix sum and modulo properties.

---

## 2. Constraints

- 1 <= n <= 5000
- -10^4 <= nums[i] <= 10^4
- 2 <= k <= 10^4


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
6 5
4 5 0 -2 -3 1
```
Output:
```text
7
Explanation:

Subarrays divisible by 5: [4,5,0,-2,-3,1], [5], [5,0], [5,0,-2,-3], [0], [0,-2,-3], [-2,-3]
```

**Example 2 (Normal Case):**
Input:
```text
4 2
4 5 0 -2
```
Output:
```text
4
Explanation:

Subarrays divisible by 2: [4], [0], [0,-2], [-2]

```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Iterate over all subarrays (i..j)
  - Compute sum and check divisibility by k

**Java Code:**
```java
public static int subarraySumDivisibleByKBrute(int[] nums, int k) {
    int n = nums.length;
    int count = 0;
    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += nums[j];
            if (sum % k == 0) count++;
        }
    }
    return count;
}
```

**Complexity:**
- Time: O(n^2) → nested loops
- Space: O(1) → constant space

### Approach 2: Optimal (Prefix Sum + HashMap)

- **Idea:**
  - Use prefix sum and modulo property: (sum[j] - sum[i-1]) % k == 0 → sum[j] % k == sum[i-1] % k
  - Track frequency of modulo values in a hashmap
  - Count pairs with same modulo

**Java Code:**
```java
import java.util.*;

public static int subarraySumDivisibleByKOptimal(int[] nums, int k) {
    Map<Integer, Integer> modCount = new HashMap<>();
    modCount.put(0, 1); // empty prefix
    int sum = 0;
    int count = 0;

    for (int num : nums) {
        sum += num;
        int mod = ((sum % k) + k) % k; // handle negative numbers
        count += modCount.getOrDefault(mod, 0);
        modCount.put(mod, modCount.getOrDefault(mod, 0) + 1);
    }

    return count;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(k) → store modulo frequencies


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n²) → inefficient for large n.
- Optimal approach leverages prefix sum + modulo properties → O(n) time.
- Correctly handles negative numbers and large arrays.

---

## 6. Variants / Follow-Ups

- Subarray sum divisible by any number k
- Count subarrays with sum divisible by k and of length ≥ m
- Count continuous subarrays with sum exactly equal to k

<!-- #endregion -->

