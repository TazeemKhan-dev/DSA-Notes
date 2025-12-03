<!-- #region 167-Kth Missing Positive Number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q167: Kth Missing Positive Number</h1>

## 1. Problem Statement

- You are given an array arr of strictly increasing positive integers and an integer k.
- You must return the k-th positive integer that is missing from the array.
- Missing means numbers starting from 1 upward that do not appear in the array.
---

## 2. Problem Understanding

- This problem wants you to find the k-th number that does NOT exist in the given sorted list.
  * We start counting natural numbers: 1, 2, 3, 4, …
  * Some of these numbers appear in arr, some do not.
  * We collect all missing numbers.
  * We return the k-th one.
- You must imagine a stream of positive integers starting from 1.
- Each time you see a number that is not inside arr, you count it as a missing positive.

```text

Example:
arr = [2, 3, 4, 7, 11]
Positive numbers: 1 2 3 4 5 6 7 8 9 10 11 …
Missing ones: 1, 5, 6, 8, 9, 10, 12, …

The 5th missing is 9.
```
---

## 3. Constraints

- 1 <= arr.length <= 1000
- 1 <= arr[i] <= 1000
- 1 <= k <= 1000
- arr is strictly increasing
- Array values are positive integers only
---

## 4. Edge Cases

- If arr[0] > 1 → missing numbers start from 1
- If k is small and misses occur before first element
- If k-th missing lies beyond the last element of array
- All arr elements are continuous → missing starts after last element
---

## 5. Examples

```text
Example 1
Input: arr = [2, 3, 4, 7, 11], k = 5
Missing: [1, 5, 6, 8, 9, 10, ...]
Output: 9

Example 2
Input: arr = [1, 2, 3, 4], k = 2
Missing: [5, 6, 7, ...]
Output: 6
```

---

## 6. Approaches

### Approach 1: Brute Force (Count Missing One-by-One)

**Idea:**
- Start from num = 1
- Check if num is inside arr
- If not → missingCount++
- Stop when missingCount == k

**Steps:**
- Iterate through positive integers
- Track missing count
- Use pointer on arr to skip matched elements
- Return k-th missing

**Java Code:**
```java
public int findKthPositive(int[] arr, int k) {
    int i = 0, num = 1;

    while (true) {
        if (i < arr.length && arr[i] == num) {
            i++;               // number exists, move array pointer
        } else {
            k--;               // missing number found
            if (k == 0) return num;
        }
        num++;
    }
}
```

**💭 Intuition Behind the Approach:**
- You simulate natural numbers from 1 upwards.
- Every time you find a number not in arr, this is missing.
- When missing count hits k, that number is the answer.

**Complexity (Time & Space):**
- O(k + n)
  * We iterate until reaching the k-th missing.
- O(1) space
  * Only pointers used.

### Approach 2: Compute Missing Count Using Formula

**Idea:**
- At index i, array value is arr[i].
- Missing numbers before arr[i]:
  * missing = arr[i] - (i + 1)

**Steps:**
- For each index, compute how many numbers are missing up to that index.
- If missing >= k → answer lies before arr[i].
- Otherwise subtract missing and continue.

**Java Code:**
```java
public int findKthPositive(int[] arr, int k) {
    int n = arr.length;

    for (int i = 0; i < n; i++) {
        int missing = arr[i] - (i + 1);

        if (missing >= k) {
            return k + i;
        }
    }

    return k + n;
}
```

**💭 Intuition Behind the Approach:**
- If numbers were not missing, the i-th index should hold value = i+1
- Difference tells how many missing exist so far
- Once that missing count reaches or passes k, we can calculate answer directly

**Complexity (Time & Space):**
- O(n) time — one pass.
- O(1) space.

### Approach 3: Binary Search

**Idea:**
- Use binary search on the index to find first position where
- missing numbers become ≥ k.
- Missing until index mid:
  * missing = arr[mid] - (mid + 1)
- Binary search condition:
  * If missing < k → move right
  * Else → move left

**Steps:**
- Binary search for smallest index where missing >= k

**Java Code:**
```java
public int findKthPositive(int[] arr, int k) {
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        int missing = arr[mid] - (mid + 1);

        if (missing < k) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return low + k;
}
```

**💭 Intuition Behind the Approach:**
- Missing count increases as numbers grow.
- This forms a monotonically increasing function, perfect for binary search.
- Binary search finds the first index where missing >= k.
- Answer is simply (low + k).

**Complexity (Time & Space):**
- Time: O(log n) — optimal
- Space: O(1)

---

## 7. Justification / Proof of Optimality

- Binary search is optimal because:
  * Missing count increases as index increases (monotonic)
  * We can directly compute missing count using formula
  * No need to count manually → faster than brute force
---

## 8. Variants / Follow-Ups

- K-th missing number in a list containing duplicates
- K-th missing number in an unsorted array
- K-th missing negative number
- K-th missing number in a continuous range [L, R]
---

## 9. Tips & Observations

- The formula missing = arr[i] - (i + 1) is key
- If all missing numbers are before arr[0], just return k
- If k is larger than all internal gaps, answer lies after arr[n-1]
---

## 10. Pitfalls

- Forgetting to consider numbers before arr[0] when arr[0] > 1
- Off-by-one errors in missing formula
- Not handling case where k-th missing is beyond last element
- Using a loop up to infinity without careful condition
---

<!-- #endregion -->
