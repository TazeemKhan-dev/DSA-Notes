<!-- #region 143-Search in a Bitonic Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q143: Search in a Bitonic Array</h1>

## 1. Problem Understanding

- You’re given a bitonic array — strictly increasing then strictly decreasing.
- You need to find the index of a target element.
- If not present → return -1.
---

## 2. Constraints

- 1 ≤ N ≤ 10^5
- Array elements: -10^6 ≤ arr[i] ≤ 10^6
- Array is bitonic, not rotated.
- Must run better than O(N) → aim for O(log N).
---

## 3. Edge Cases

- Array of size 1
- Target is at:
  * Peak
  * Beginning
  * End
- All elements increasing (no decreasing part)
- All elements decreasing (no increasing part)
- Target not present
---

## 4. Examples

```text
Example 1

Input:
[-3, 9, 18, 20, 17, 5, 1], target = 20
Output: 3

Example 2

Input:
[3, 4, 1], target = 5
Output: -1
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Search)

**Idea:**
- Scan every element and check if it equals target.

**Steps:**
- Loop from 0 to N-1
- If arr[i] == target → return i
- Else return -1

**Java Code:**
```java
public int findInBitonic(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

**Complexity (Time & Space):**
- ⏱️ Complexity
- Time: O(N)
  * Because we may scan the entire array.
- Space: O(1)
  * No extra memory used.

### Approach 2: Find Peak + Two Binary Searches

**Idea:**
- First, find the peak of the bitonic array using binary search.
- Perform ascending binary search on left side.
- Perform descending binary search on right side.

**Steps:**
- Step 1 — Find Peak
  * Binary search to find index peak where:
    * arr[mid] > arr[mid-1] && arr[mid] > arr[mid+1]
- Step 2
  * Binary search (ascending) from 0 to peak.
- Step 3
  * Binary search (descending) from peak to N-1.

**Java Code:**
```java
public int findInBitonic(int[] arr, int target) {
    int peak = findPeak(arr);

    int left = ascBinary(arr, 0, peak, target);
    if (left != -1) return left;

    return descBinary(arr, peak + 1, arr.length - 1, target);
}

private int findPeak(int[] arr) {
    int low = 0, high = arr.length - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] < arr[mid + 1]) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return low;
}

private int ascBinary(int[] arr, int low, int high, int target) {
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}

private int descBinary(int[] arr, int low, int high, int target) {
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;
        else if (arr[mid] > target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- A bitonic array has only one peak. You can find it using binary search in O(log N).
- Once you know the peak:
  * Left part is sorted ascending.
  * Right part is sorted descending.
  * Binary search works on both separately — guaranteeing logarithmic time.

**Complexity (Time & Space):**
- Time:
  * Peak search → O(log N)
  * Two binary searches → O(log N)
  * Total → O(log N)
  * Why?
    * All operations are binary searches that divide the search space in half each iteration.
- Space:
  * O(1)
  * Why?
    * No extra data structures used.

### Approach 3: Single Binary Search Without Explicit Peak Search

**Idea:**
- Perform binary search directly using bitonic property:
- At each mid:
- If arr[mid] == target → return mid
- Determine whether you're in increasing or decreasing part:
  * If arr[mid] < arr[mid+1] → you're in increasing part
  * Else → in decreasing part
- Based on direction + value relation with target, move left or right.

**Steps:**
- Standard binary search bounds low = 0, high = N-1
- Use slope (increasing/decreasing) to decide direction

**Java Code:**
```java
public int findInBitonicOptimal(int[] arr, int target) {
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;

        boolean inc = mid + 1 < arr.length && arr[mid] < arr[mid + 1];

        if (inc) {
            // Increasing part
            if (target > arr[mid]) low = mid + 1;
            else high = mid - 1;
        } else {
            // Decreasing part
            if (target > arr[mid]) high = mid - 1;
            else low = mid + 1;
        }
    }

    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Instead of first finding the peak, you infer the slope at mid:
  * If slope ↑ (increasing), you can decide which direction the target lies.
  * If slope ↓ (decreasing), same logic but reversed.
- This avoids three separate binary searches → simplifies logic.

**Complexity (Time & Space):**
- Time: O(log N)
- Because each step halves the search range.
- Space: O(1)
- No recursion or extra memory.

---

## 6. Justification / Proof of Optimality

- Binary search leverages the monotonic nature of subarrays.
- Bitonic arrays split naturally into two monotonic halves.
- Using either:
  * peak → 2 binary searches
  * or
  * slope → 1 unified binary search
  * ensures O(log N) performance.
---

## 7. Variants / Follow-Ups

- Search in rotated array
- Find peak element
- Mountain array search (LeetCode)
- Find minimum in rotated array
- Check if array is bitonic
---

## 8. Tips & Observations

- Peak in bitonic array is always found via arr[mid] < arr[mid+1]
- Use < and <= carefully to avoid infinite loops
- Avoid recursion for bitonic search — iterative is cleaner
---

<!-- #endregion -->
