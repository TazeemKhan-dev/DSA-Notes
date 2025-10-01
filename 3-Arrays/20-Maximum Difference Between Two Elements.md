<!-- #region 20-Maximum Difference Between Two Elements -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q20: Maximum Difference Between Two Elements</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an array of positive integers. We need to find the maximum absolute difference between any two elements in the array.
- **Goal:** Find max(|arr[i] - arr[j]|) for all i, j pairs.
- **Paraphrase:** Maximum difference occurs between the largest and smallest element in the array.  There’s no need to check every pair individually.

---

## 2. Constraints

- 2 <= n <= 10^6
- 0 < arr[i] <= 10^9
- Elements are positive integers


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
16 24 89 35
```
Output:
```text
73
Explanation: max(89-16) = 73
```


---

## 4. Approaches

### Approach 1: Optimal Approach (Single Scan)

- **Idea:**
  - Maximum difference is max(arr) - min(arr)
  - Scan array once to find min and max

**Java Code:**
```java
public static long maxDifference(int[] arr) {
    int n = arr.length;
    int minVal = arr[0], maxVal = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] < minVal) minVal = arr[i];
        if (arr[i] > maxVal) maxVal = arr[i];
    }
    return (long)maxVal - (long)minVal;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant extra space

### Approach 2: Brute Force (Check All Pairs)

- **Idea:**
  - Iterate over all pairs (i, j) and compute |arr[i]-arr[j]|
  - Track the maximum

**Complexity:**
- Time: O(n^2) → not feasible for large n
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approach is clearly better: O(n) time, O(1) space, handles large arrays.
- Brute force works for small arrays only and is inefficient.

---

## 6. Variants / Follow-Ups

- Maximum difference with constraint i < j
- Maximum difference after sorting the array
- Maximum difference for subarrays
- Maximum difference modulo some number

<!-- #endregion -->

