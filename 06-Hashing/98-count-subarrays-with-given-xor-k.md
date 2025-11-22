<!-- #region 98-Count subarrays with given xor K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q98: Count subarrays with given xor K</h1>

## 1. Problem Understanding

- You are given an integer array nums and an integer k.
- You need to count all subarrays whose XOR of elements equals k.
- XOR (^) is bitwise exclusive OR.
---

## 2. Constraints

- 1 <= nums.length <= 10^5 → O(n²) brute force may TLE, need O(n).
- 1 <= nums[i] <= 10^9 → elements are positive integers.
- 1 <= k <= 10^9.
---

## 3. Edge Cases

- Single element equal to k → counts as one subarray.
- Multiple subarrays with same XOR.
- Large numbers → ensure XOR calculations are correct (int is fine in Java).
---

## 4. Examples

```text
Input: [4, 2, 2, 6, 4], k = 6 → Output: 4 → [4,2], [4,2,2,6,4], [2,2,6], [6]

Input: [5, 6, 7, 8, 9], k = 5 → Output: 2 → [5], [5,6,7,8,9]

Input: [5, 2, 9], k = 7 → Output: 0
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check XOR for all subarrays.

**Steps:**
- Initialize count = 0
- Loop i from 0 to n-1
- Loop j from i to n-1 → xor = XOR(nums[i..j])
- If xor == k → count++

**Java Code:**
```java
public int subarrayXor(int[] nums, int k) {
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        int xor = 0;
        for (int j = i; j < nums.length; j++) {
            xor ^= nums[j];
            if (xor == k) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n²) → TLE for large n
- 💾 Space Complexity:
  * O(1)

### Approach 2: Prefix XOR + HashMap (Optimized) ✅

**Idea:**
- Let prefixXOR[i] = XOR of all elements from 0 to i.
- For subarray nums[l..i], XOR = prefixXOR[i] ^ prefixXOR[l-1]
- If prefixXOR[i] ^ k exists in map → there exists a subarray ending at i with XOR k.

**Steps:**
- Initialize map = {0:1}, xor = 0, count = 0
- Loop over each element in nums:
  * xor ^= nums[i]
  * If (xor ^ k) exists in map → count += map.get(xor ^ k)
  * map[xor] = map.getOrDefault(xor, 0) + 1

**Java Code:**
```java
import java.util.HashMap;

public class Solution {
    public int subarrayXor(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1); // base case
        int xor = 0, count = 0;
        for (int num : nums) {
            xor ^= num;
            count += map.getOrDefault(xor ^ k, 0);
            map.put(xor, map.getOrDefault(xor, 0) + 1);
        }
        return count;
    }
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n) → single pass over array
- 💾 Space Complexity:
  * O(n) → hashmap storing prefix XOR counts

---

## 6. Justification / Proof of Optimality

- The hashmap stores frequency of prefix XORs.
- xor ^ k in map → there exists a previous prefix XOR that will form a subarray with XOR = k.
- Works for large positive integers.
---

## 7. Variants / Follow-Ups

- Count subarrays with XOR ≤ k → needs trie-based approach.
- Find longest subarray with XOR k → use first occurrence of prefix XOR in map.
---

## 8. Tips & Observations

- XOR is reversible: a ^ b = c → a = b ^ c
- Initialize map.put(0,1) → handles subarrays starting from index 0.
- Similar template to prefix sum with hashmap, just replace + with ^.
---

<!-- #endregion -->
