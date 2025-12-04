<!-- #region 172-Largest Subarray with 0 Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q172: Largest Subarray with 0 Sum</h1>

## 1. Problem Statement

- Given an integer array arr of length N, find the length of the longest contiguous subarray whose sum is exactly 0.
- You must return/print the maximum length of any such subarray.
---

## 2. Problem Understanding

- We need the longest continuous segment inside the array that adds up to 0.
- Example:
  * 15 -2 2 -8 1 7 10 23
  * Subarray [-2, 2, -8, 1, 7] gives sum = 0 and length = 5.
- The brute-force way is to try all subarrays and compute sums, which is slow (O(N^2)).
- The optimal way uses prefix sums + hashmap:
  * If prefix_sum[j] == prefix_sum[i], then the subarray (i+1 to j) has sum 0.
  * Store the earliest index for each prefix sum in a map.
  * Whenever the same sum reappears, compute length.
- This avoids recomputing subarray sums repeatedly.
---

## 3. Constraints

- -100000 <= N <= 100000
- 0 <= arr[i] <= 100000
- Large N → must use O(N) solution.
---

## 4. Edge Cases

- Array with no 0-sum subarray → answer is 0.
- If the prefix sum is 0 at index i, then subarray from 0...i is valid.
- Multiple repeated prefix sums → only store earliest index.
---

## 5. Examples

```text
Example 1
Input:
15 -2 2 -8 1 7 10 23
Output: 5

Example 2
Input:
1 2 3
Output: 0
```

---

## 6. Approaches

### Approach 1: Brute Force (Too Slow)

**Idea:**
- Try every subarray and check if its sum is 0.

**Java Code:**
```java
public int longestZeroSum(int[] arr) {
    int n = arr.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum == 0) {
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(N^2)
- Space: O(1)

### Approach 2: Prefix Sum + HashMap (Optimal)

**Idea:**
- Use prefix sums:
- Let prefix[i] = sum of elements from 0 to i.
- If prefix[j] == prefix[i], the subarray from i+1 to j has sum 0.
- Use a HashMap to store the first time each prefix sum appears.

**Steps:**
- Create map: sum → earliest index.
- Maintain running sum currSum.
- If currSum == 0, update max length.
- If sum already exists in map, compute subarray length.
- Otherwise store current sum with index.

**Java Code:**
```java
public int longestZeroSum(int[] arr) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int currSum = 0;
    int maxLen = 0;

    for (int i = 0; i < arr.length; i++) {
        currSum += arr[i];

        if (currSum == 0) {
            maxLen = i + 1;
        }

        if (map.containsKey(currSum)) {
            int length = i - map.get(currSum);
            maxLen = Math.max(maxLen, length);
        } else {
            map.put(currSum, i);
        }
    }

    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- If a prefix sum repeats, the segment between the repeated indices cancels out to zero.
- Storing the first occurrence gives the longest possible subarray.

**Complexity (Time & Space):**
- Time: O(N)
  * Why: Each element processed once.
- Space: O(N)
  * Why: Worst case all prefix sums are unique.

---

## 7. Justification / Proof of Optimality

- Prefix sum repetition is the most efficient way to detect zero-sum subarrays.
- Only prefix sum logic guarantees a linear solution.
---

## 8. Variants / Follow-Ups

- Longest subarray with sum = K.
- Count number of subarrays with sum = 0.
- Shortest subarray with sum = 0.
---

## 9. Tips & Observations

- Always store prefix sums only once (earliest index).
- If prefix sum becomes zero at index i, full range 0..i is valid.
- Negative values do not break the prefix sum logic.
---

## 10. Pitfalls

- Storing repeated sums again → breaks longest logic.
- Forgetting case when prefix sum itself becomes 0.
- Using long if values might overflow (safe to use int for constraints here).
---

<!-- #endregion -->
