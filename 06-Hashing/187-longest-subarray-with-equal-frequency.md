<!-- #region 187-Longest subarray with equal frequency -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q187: Longest subarray with equal frequency</h1>

## 1. Problem Statement

- You are given an array containing only 0, 1, and 2.
- Find the length of the longest contiguous subarray such that the frequency of 0, 1, and 2 is equal.
---

## 2. Problem Understanding

- You need to find a subarray (contiguous) where:
  * Count of 0 = count of 1 = count of 2
  * Order matters
  * Only the length of such subarray is required
- This is not a sliding window problem because the condition is not monotonic.
- It is a prefix-difference + hashmap problem.
---

## 3. Constraints

- 1 <= n <= 100000
- Array contains only 0, 1, 2
---

## 4. Edge Cases

- No valid subarray → return 0
- Entire array is valid
- Array with only one or two distinct values
- Subarray starting from index 0
---

## 5. Examples

```text
Example 1

Input:

[0, 1, 0, 2, 0, 1, 0]


Output:

3

Example 2

Input:

[0, 1, 1, 2, 0, 2]


Output:

6
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Subarrays)

**Idea:**
- Generate all subarrays and count frequency of 0, 1, and 2.
- If all three counts are equal, update the maximum length.

**Java Code:**
```java
public static int longestEqual012Brute(int[] arr) {
    int n = arr.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int c0 = 0, c1 = 0, c2 = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 0) c0++;
            else if (arr[j] == 1) c1++;
            else c2++;

            if (c0 == c1 && c1 == c2) {
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(n^2) — checking all subarrays
- Space: O(1) — only counters used
- Not feasible for large n.

### Approach 2: Prefix Difference + HashMap (Optimal ✅)

**Idea:**
- Instead of tracking three counts, track relative differences:
  * Let
    * d1 = count1 - count0
    * d2 = count2 - count1
- If the same (d1, d2) pair occurs again at index j, it means the subarray between previous index and j has equal 0s, 1s, and 2s.

**Steps:**
- Maintain running counts of 0, 1, 2
- Compute (d1, d2) at each index
- Store the first occurrence of each (d1, d2) in a HashMap
- When the same pair repeats, compute subarray length

**Java Code:**
```java
public static int longestEqual012(int[] arr) {
    int c0 = 0, c1 = 0, c2 = 0;
    int maxLen = 0;

    Map<String, Integer> map = new HashMap<>();
    map.put("0#0", -1); // base case

    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == 0) c0++;
        else if (arr[i] == 1) c1++;
        else c2++;

        int d1 = c1 - c0;
        int d2 = c2 - c1;

        String key = d1 + "#" + d2;

        if (map.containsKey(key)) {
            maxLen = Math.max(maxLen, i - map.get(key));
        } else {
            map.put(key, i);
        }
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- Equal counts mean relative differences cancel out
- Prefix differences convert a 3-variable condition into a hashmap lookup
- Same prefix state appearing again implies balanced subarray in between
- This technique generalizes to similar frequency-equality problems

**Complexity (Time & Space):**
- Time: O(n) — single traversal
- Space: O(n) — hashmap stores prefix states

---

## 7. Justification / Proof of Optimality

- Brute force is too slow for large inputs
- Prefix difference reduces the problem to hashmap lookup
- HashMap ensures earliest index is preserved for maximum length
- This is a classic and optimal interview approach
---

## 8. Variants / Follow-Ups

- Longest subarray with equal 0s and 1s
- Longest subarray with equal frequency of k values
- Prefix sum + hashmap problems
- Balanced subarray problems
---

## 9. Tips & Observations

- Always store the first occurrence of a prefix state
- Use a composite key for multiple differences
- Base case is crucial (0#0 at index -1)
---

## 10. Pitfalls

- Forgetting base case in hashmap
- Using absolute counts instead of differences
- Overwriting first occurrence in hashmap
- Treating this as sliding window (it is not)
---

<!-- #endregion -->
