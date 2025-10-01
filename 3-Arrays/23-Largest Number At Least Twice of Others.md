<!-- #region 23-Largest Number At Least Twice of Others -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q23: Largest Number At Least Twice of Others</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array nums of size n. The largest integer is unique.
- **Goal:** Determine whether the largest number is at least twice as large as every other number in the array.  If yes, return the index of the largest number; else return -1.
- **Paraphrase:** Find the largest element and compare it with all other elements.  Check if it is ≥ 2 × any other element.

---

## 2. Constraints

- 1 <= n <= 50
- 0 <= nums[i] <= 100
- Largest number in array is unique


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
3 6 1 0
```
Output:
```text
1
Explanation: 6 ≥ 2 × 3, 6 ≥ 2 × 1, 6 ≥ 2 × 0 → condition satisfied
```

**Example 2 (Normal Case):**
Input:
```text
4
1 2 3 4
```
Output:
```text
-1
Explanation: 4 < 2 × 3 → condition fails
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Find the largest element and its index.
  - Iterate through the array and check if largest >= 2 × arr[i] for all other elements.

**Java Code:**
```java
public static int largestAtLeastTwiceBrute(int[] nums) {
    int n = nums.length;
    int maxVal = nums[0];
    int maxIndex = 0;

    // Find largest element
    for (int i = 1; i < n; i++) {
        if (nums[i] > maxVal) {
            maxVal = nums[i];
            maxIndex = i;
        }
    }

    // Check condition
    for (int i = 0; i < n; i++) {
        if (i != maxIndex && maxVal < 2 * nums[i]) {
            return -1;
        }
    }

    return maxIndex;
}
```

**Complexity:**
- Time: O(n) → two passes over array
- Space: O(1) → constant space

### Approach 2: Optimal (Single Pass)

- **Idea:**
  - Track both the largest and second largest elements in a single pass.
  - Compare largest with 2 × second largest.

**Java Code:**
```java
public static int largestAtLeastTwiceOptimal(int[] nums) {
    int n = nums.length;
    int max1 = -1, max2 = -1, maxIndex = -1;

    for (int i = 0; i < n; i++) {
        if (nums[i] > max1) {
            max2 = max1;
            max1 = nums[i];
            maxIndex = i;
        } else if (nums[i] > max2) {
            max2 = nums[i];
        }
    }

    return (max1 >= 2 * max2) ? maxIndex : -1;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → constant space


---

## 5. Justification / Proof of Optimality

- Brute force is simple, intuitive, and works well for small n (≤50).
- Optimal approach reduces the comparison step by tracking the second largest element → still O(n) but slightly faster and elegant.

---

## 6. Variants / Follow-Ups

- Largest number at least k times others
- Largest number at least twice for subarrays
- Return all numbers that satisfy condition instead of just largest
- Extend to non-unique largest elements

<!-- #endregion -->

