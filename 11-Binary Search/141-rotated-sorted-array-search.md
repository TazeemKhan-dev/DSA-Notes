<!-- #region 141-Rotated Sorted Array Search -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q141: Rotated Sorted Array Search</h1>

## 1. Problem Understanding

- You are given an array that was originally sorted in non-decreasing order but then rotated at some pivot. Example:
  * Original: 0 1 2 4 5 6 7
  * Rotated : 4 5 6 7 0 1 2
- Your task:
- Find the index of target B in this rotated sorted array.
- If not present → return -1.
- No duplicates exist.
- Goal: O(log n) → must use binary search.
---

## 2. Constraints

- 1 ≤ N ≤ 1e6
- 1 ≤ A[i] ≤ 1e9
- Array was sorted then rotated
- All elements are unique
- Must achieve O(log n)
---

## 3. Edge Cases

- Target is at pivot
- Array is not rotated at all (normal sorted)
- Target smaller than all elements
- Target larger than all elements
- Single-element array
- Pivot at index 0 or last index
- Target at boundaries
---

## 4. Examples

```text
Example 1

Input

8
4 5 6 7 0 1 2 3
4


Output

0

Example 2

Input

4
5 17 100 3
6


Output

-1
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Loop from 0 to N-1
- If element equals target → return index
- Else return -1

### Approach 2: Find Pivot then Binary Search Both Halves (Better)

**Idea:**
- In a rotated sorted array, the smallest element is the pivot.
- Example:
  * 4 5 6 7 0 1 2 → pivot is at index 4.
- Steps:
  * Find pivot using binary search
  * Decide whether target is in left side or right side
  * Apply binary search normally in that half

**Steps:**
- Binary search to find smallest element index (pivot).
- If target lies in interval [arr[pivot], arr[n-1]] → search in right half.
- Else search in left half.

**Java Code:**
```java
public int search(int[] arr, int target) {
    int n = arr.length;

    // Step 1: find pivot (index of smallest element)
    int low = 0, high = n - 1;
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] > arr[high]) low = mid + 1;
        else high = mid;
    }
    int pivot = low;

    // Step 2: decide which half to search
    if (target >= arr[pivot] && target <= arr[n - 1]) {
        return binarySearch(arr, pivot, n - 1, target);
    } else {
        return binarySearch(arr, 0, pivot - 1, target);
    }
}

private int binarySearch(int[] arr, int low, int high, int target) {
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- A rotated sorted array is two sorted halves.
- Find pivot → pick correct half → normal binary search.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Pivot search: O(log n)
  * Binary search in correct half: O(log n)
  * Total: O(log n)
- 💾 Space Complexity
  * O(1)

### Approach 3: Single-Pass Binary Search (Optimal)

**Idea:**
- We modify binary search logic itself.
- At any mid, one side is always sorted:
- Either:
  * Left half is sorted
  * OR right half is sorted
- We check which half is sorted and decide where to move.

**Steps:**
- Standard binary search loop
- If arr[mid] == target → return mid
- Check which half is sorted
  * If left half sorted (arr[low] ≤ arr[mid]):
    * Check if target lies in left sorted range
    * → move right or left accordingly
  * Else right half sorted:
    * Check if target lies in right sorted range
    * → move accordingly

**Java Code:**
```java
public int search(int[] arr, int target) {
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;

        // Left half sorted
        if (arr[low] <= arr[mid]) {
            if (target >= arr[low] && target < arr[mid])
                high = mid - 1;
            else
                low = mid + 1;
        }
        // Right half sorted
        else {
            if (target > arr[mid] && target <= arr[high])
                low = mid + 1;
            else
                high = mid - 1;
        }
    }

    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Every mid splits the array into two halves, and one half is guaranteed to be sorted.
- By checking:
  * whether target lies inside that sorted half
  * or in the unsorted half
- we discard half of the array each time → classic binary search.
- This avoids finding pivot separately.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * We halve the search space every iteration.
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Binary search works perfectly here because even after rotation, one part of the array remains sorted at every division.
- This property ensures we can always make the correct decision about which half to search.
---

## 7. Variants / Follow-Ups

- Search element in rotated array with duplicates
- Search min element in rotated array
- Search max element in rotated array
- Count rotations
- Find pivot index
- Find peak element
---

## 8. Tips & Observations

- At every mid, one half is sorted → the key insight
- If arr[low] <= arr[mid] → left sorted
- Else → right sorted
- Use ranges carefully when comparing target
- Avoid infinite loops by updating low and high properly
---

<!-- #endregion -->
