<!-- #region 26-Maximum Sum Subarray -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q26: Maximum Sum Subarray</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array arr. We need to find the maximum sum of any contiguous subarray.
- **Goal:** Return the sum of the subarray that has the largest sum among all contiguous subarrays.
- **Paraphrase:** Find subarray [i..j] with sum arr[i]+arr[i+1]+...+arr[j] maximized.

---

## 2. Constraints

- 1 <= n <= 10^4
- -100 <= arr[i] <= 100


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
5
2 3 1 -1 0
```
Output:
```text
6
Explanation: Maximum subarray sum = 2 + 3 + 1 = 6
```

**Example 2 (Normal Case):**
Input:
```text
8
-2 -3 4 -1 -2 1 5 -3
```
Output:
```text
7
Explanation: Maximum subarray sum = 4 + -1 + -2 + 1 + 5 = 7
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Check all possible subarrays
  - Calculate sum for each subarray
  - Track maximum sum

**Java Code:**
```java
public static int maxSubarraySumBrute(int[] arr) {
    int n = arr.length;
    int maxSum = Integer.MIN_VALUE;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum > maxSum) maxSum = sum;
        }
    }

    return maxSum;
}
```

**Complexity:**
- Time: O(n²) → nested loops
- Space: O(1) → constant space

### Approach 2: Optimal (Kadane’s Algorithm)

- **Idea:**
  - Maintain a running sum (currentSum)
  - If currentSum < 0, reset to 0
  - Track maximum sum seen so far

**Java Code:**
```java
public static int maxSubarraySumOptimal(int[] arr) {
    int maxSum = arr[0];
    int currentSum = arr[0];

    for (int i = 1; i < arr.length; i++) {
        currentSum = Math.max(arr[i], currentSum + arr[i]);
        maxSum = Math.max(maxSum, currentSum);
    }

    return maxSum;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant space

### Approach 3: 2b: Optimal with Long (Handling Overflow)

- **Idea:**
  - Use long for sum and maxi to avoid overflow with large integers
  - Same logic as Kadane’s algorithm: maintain running sum, reset if negative

**Java Code:**
```java
public static int maxSubarraySumOptimalLong(int[] nums) {
    long maxi = Long.MIN_VALUE;
    long sum = 0;

    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];   // safe sum using long
        if (sum > maxi) maxi = sum;

        if (sum < 0) sum = 0;
    }

    return (int) maxi;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant space, Safe for large integers → avoids int overflow


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n²) → inefficient for large arrays
- Kadane’s algorithm works in O(n) and handles negative numbers correctly
- Correctly identifies subarray with maximum sum without storing all subarrays

---

## 6. Variants / Follow-Ups

- Maximum product subarray
- Maximum sum subarray of length exactly k
- Maximum sum subarray in circular array
- Return start and end indices of maximum subarray

<!-- #endregion -->

