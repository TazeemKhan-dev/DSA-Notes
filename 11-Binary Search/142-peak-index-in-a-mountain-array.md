<!-- #region 142-Peak Index in a Mountain Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q142: Peak Index in a Mountain Array</h1>

## 1. Problem Understanding

- You are given a mountain array, meaning:
- It strictly increases up to a peak
- Then strictly decreases
- Example:
  * 0 2 5 7 6 3 1
          * ↑
         * peak
- You must find the index of the peak element.
- Mountain array guarantees:
- Exactly one peak
- Peak is not at index 0 or index n-1
- Must solve in O(log n)
---

## 2. Constraints

- 3 ≤ n ≤ 1e5
- Values: 0 ≤ arr[i] ≤ 1e6
- Array is guaranteed to be a perfect mountain
- Must solve in O(log n)
---

## 3. Edge Cases

- (not many because mountain is guaranteed)
  * Peak at index 1 (smallest valid peak)
  * Peak at index n-2 (largest valid peak)
  * Large values
  * Minimum length (3 elements)
---

## 4. Examples

```text
Example 1

Input

3
0 1 0


Output

1

Example 2

Input

4
0 2 1 0


Output

1

Example 3

Input

4
0 10 5 2


Output

1
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Scan until the numbers stop increasing.
- The first time you see a drop, the previous index is peak.

**Steps:**
- Loop from i = 1 to n-1
- If arr[i] < arr[i-1]
  * → return i-1
- Because guaranteed mountain → no other checks needed.

**Java Code:**
```java
public int peakIndex(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] < arr[i - 1]) return i - 1;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Peak is exactly where sequence changes from increasing → decreasing.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
- 💾 Space Complexity
  * O(1)

### Approach 2: Binary Search (Optimal)

**Idea:**
- Binary search for the point where slope changes:
  * If arr[mid] < arr[mid + 1] → we are on the increasing slope
  * → peak is on the right
  * → move low = mid + 1
  * If arr[mid] > arr[mid + 1] → we are on the decreasing slope
  * → mid could be the peak
  * → move high = mid
- Eventually low == high → peak index.

**Steps:**
- Initialize low = 0, high = n-1
- While low < high:
  * Compute mid
  * If increasing part → move right
  * If decreasing part → move left
- Return low (same as high)

**Java Code:**
```java
public int peakIndex(int[] arr) {
    int low = 0, high = arr.length - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] < arr[mid + 1]) {
            // Increasing part: peak is on the right
            low = mid + 1;
        } else {
            // Decreasing part: peak is mid or left side
            high = mid;
        }
    }

    return low; // or high, both same
}

In class solution
public int findPeak(int arr[], int n) {

    // Handle corner peaks
    if (n == 1) return 0;   // Only one element
    if (arr[0] >= arr[1]) return 0;
    if (arr[n-1] >= arr[n-2]) return n-1;

    int low = 1, high = n - 2;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        // Correct peak condition
        if (arr[mid] >= arr[mid-1] && arr[mid] >= arr[mid+1]) {
            return mid;
        }

        // If increasing → peak lies to the right
        if (arr[mid] < arr[mid + 1]) {
            low = mid + 1;
        }
        else {
            // If decreasing → peak lies to the left
            high = mid - 1;
        }
    }

    return -1;   // theoretically unreachable
}
  
```

**💭 Intuition Behind the Approach:**
- The mountain array has a monotonic behavior:
  * Increasing → Peak → Decreasing
- At any mid:
  * If arr[mid] < arr[mid+1], slope is increasing → move right
  * If arr[mid] > arr[mid+1], slope is decreasing → peak is left or mid
- We narrow down the peak using binary search.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * We cut the search space in half each step.
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Binary search works because the array is monotonic on each side of the peak:
  * left half strictly increasing
  * right half strictly decreasing
- This allows you to determine the direction of the peak at every step, ensuring O(log n) time.
---

## 7. Variants / Follow-Ups

- Find peak in mountain array (LeetCode 852)
- Find peak element in unsorted array (LeetCode 162)
- Find max in bitonic array
- Find first element greater than previous
- Find turning point in unimodal array
---

## 8. Tips & Observations

- Use mid < mid+1 to detect slope
- Always check arr[mid] < arr[mid+1] FIRST
- low will always converge to the peak
- Never check neighbors outside bounds because mountain is guaranteed
---

<!-- #endregion -->
