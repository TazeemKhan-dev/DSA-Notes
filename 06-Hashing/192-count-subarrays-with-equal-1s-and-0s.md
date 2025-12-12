<!-- #region 192-Count Subarrays with Equal 1's and 0's -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q192: Count Subarrays with Equal 1's and 0's</h1>

## 1. Problem Statement

- You are given an array of size n containing only 0 and 1.
- You must find how many subarrays contain an equal number of 0s and 1s.
---

## 2. Problem Understanding

- A subarray with equal 0s and 1s has:
  * #1s - #0s = 0
- Trick:
  * Convert 0 → -1
  * Keep 1 as it is
- Then the problem becomes:
  * Count subarrays whose sum is 0.
- This becomes identical to counting zero-sum subarrays using prefix-sum frequency.
- Let prefix[i] = sum of transformed elements from 0 ... i.
- If prefix sum repeats:
  * prefix[j] == prefix[i]  → sum(i+1...j) = 0
- Hence, total pairs of equal prefix sums give total valid subarrays.
---

## 3. Constraints

- 1 ≤ N ≤ 10^6 (very large → must be O(N))
- Values are only 0 or 1
---

## 4. Edge Cases

- All zeros → after converting to -1, no sum-zero except possible pairs
- All ones → no valid subarrays
- Large N → must use O(N) map-based approach
- Single element → no possible equal subarray
---

## 5. Examples

```text
Example 1
Input

3
1 0 1
Output

2
Explanation

the subarrays from index 0 to 1 and 1 to 2 have equal number of 1's and 0's.

Example 2
Input

2
1 1
Output

0
Explanation

No such subarray is possible with equal number of 1's and 0's.
```

---

## 6. Approaches

### Approach 1: Brute Force (Too Slow)

**Idea:**
- Try all subarrays, count 0s and 1s.

**Java Code:**
```java
public int countSubarrays(int[] arr) {
    int n = arr.length;
    int count = 0;
    for (int i = 0; i < n; i++) {
        int zeros = 0, ones = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 0) zeros++;
            else ones++;
            if (zeros == ones) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N^2)
- Space: O(1)

### Approach 2: Convert 0 → -1 + Prefix Sum + HashMap (Optimal)

**Idea:**
- Transform array:
  * 0 → -1
  * 1 → 1
- Count subarrays with sum = 0 using:
  * prefix sum
  * HashMap storing freq of each prefix sum
- Rule:
  * If prefix sum x appears f times:
    * f choose 2  (pairs) = f*(f-1)/2
- Plus cases where prefix sum is 0 itself.

**Java Code:**
```java
public long countSubarrays(int[] arr) {
    int n = arr.length;

    Map<Integer, Long> map = new HashMap<>();
    map.put(0, 1L); // prefix sum 0 exists initially

    int prefix = 0;
    long count = 0;

    for (int x : arr) {
        if (x == 0) prefix += -1;
        else prefix += 1;

        if (map.containsKey(prefix)) {
            count += map.get(prefix);
        }

        map.put(prefix, map.getOrDefault(prefix, 0L) + 1);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Converting 0 → -1 makes equal 0s and 1s become a zero-sum subarray.
- If prefix sum repeats, the intermediate subarray has sum 0.
- Counting frequencies of prefix sums automatically counts all valid subarrays.

**Complexity (Time & Space):**
- Time: O(N)
  * One pass + constant-time map operations
- Space: O(N)
  * Worst-case all prefix sums unique

---

## 7. Justification / Proof of Optimality

- Convert 0 → -1 so that equal 0s and 1s means the subarray sum becomes 0.
- If a prefix sum repeats, the subarray between those two indices sums to 0, which means equal 0s and 1s.
- Counting how many times each prefix sum occurs automatically counts all valid subarrays.
- This approach is O(N) and optimal because checking every subarray (O(N²)) is impossible for large N.
---

## 8. Variants / Follow-Ups

- Count subarrays with sum = K
- Largest subarray with equal 0s and 1s
- Replace elements to make balanced subarrays
- Zero-sum subarrays in general arrays
---

## 9. Tips & Observations

- Always put map.put(0, 1) initially
- Use long for counting (can overflow int)
- Prefix-sum hashing is universal for subarray equality problems
---

## 10. Pitfalls

- Forgetting to convert 0 → -1
- Forgetting prefix sum = 0 case
- Incorrectly pairing prefix sums
- Using int instead of long for count
---

<!-- #endregion -->
