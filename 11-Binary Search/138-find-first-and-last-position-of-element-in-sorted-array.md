<!-- #region 138-Find First and Last Position of Element in Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q138: Find First and Last Position of Element in Sorted Array</h1>

## 1. Problem Understanding

- Given a sorted (non-decreasing) array nums, find the first and last index of a given target.
- If target doesn’t appear, return [-1, -1].
- We must do it in O(log n), so binary search is required.
---

## 2. Constraints

- Array length: 0 to 1e5
- Values range: -1e9 to 1e9
- Array is sorted
- Must achieve O(log n)
---

## 3. Edge Cases

- n = 0 → return [-1, -1]
- Target is smaller than first element
- Target is greater than last element
- All elements are the same as target → output [0, n-1]
- Target appears once
- Target appears multiple times
---

## 4. Examples

```text
Example 1

Input

6 8
5 7 7 8 8 10


Output

3 4

Example 2

Input

6 6
5 7 7 8 8 10


Output

-1 -1
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Scan)

**Idea:**
- Iterate whole array once to find first occurrence and last occurrence.

**Steps:**
- Loop from left to right → find first match
- Loop from right to left → find last match

**Java Code:**
```java
public int[] searchRange(int[] nums, int target) {
    int first = -1, last = -1;

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == target) {
            first = i;
            break;
        }
    }

    for (int i = nums.length - 1; i >= 0; i--) {
        if (nums[i] == target) {
            last = i;
            break;
        }
    }

    return new int[]{first, last};
}
```

**💭 Intuition Behind the Approach:**
- We check the entire array because it’s unspecific where target may occur.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) because we scan the array twice.
- 💾 Space Complexity
  * O(1) because no extra space.

### Approach 2: Two Binary Searches (Optimal)

**Idea:**
- Use binary search twice:
  * once to find the first occurrence
  * once to find the last occurrence
- Because array is sorted, binary search ensures O(log n).

**Steps:**
- Steps
- Find first position
  * Normal binary search
  * When you find nums[mid] == target, move left to see if earlier occurrence exists
  * So set high = mid - 1
- Find last position
  * When nums[mid] == target, move right
  * So set low = mid + 1
- Return [first, last]

**Java Code:**
```java
public int[] searchRange(int[] nums, int target) {
    int first = findFirst(nums, target);
    int last = findLast(nums, target);
    return new int[]{first, last};
}

private int findFirst(int[] nums, int target) {
    int low = 0, high = nums.length - 1, ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (nums[mid] == target) {
            ans = mid;
            high = mid - 1; // go left
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}

private int findLast(int[] nums, int target) {
    int low = 0, high = nums.length - 1, ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (nums[mid] == target) {
            ans = mid;
            low = mid + 1; // go right
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Binary search normally stops when the target is found, but we modify it:
  * For first occurrence, we force the search to continue left until no earlier index contains the target.
  * For last occurrence, we force search right.
- This preserves the O(log n) speed but still finds boundaries.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Two binary searches → O(log n)
  * Why?
    * Each binary search halves the array repeatedly, so two searches still log n + log n = log n.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only variables used; no extra data structures.

---

## 6. Justification / Proof of Optimality

- Binary search ensures fastest possible runtime (O(log n)), and by slightly modifying conditions, we can locate the exact boundaries of the target.
---

## 7. Variants / Follow-Ups

- Find first occurrence only
- Find last occurrence only
- Count occurrences → last - first + 1
- Works for rotated sorted arrays with modifications
---

## 8. Tips & Observations

- Always prefer binary search if array is sorted and you need positions.
- Use the mid calculation trick:
- mid = low + (high - low) / 2 to avoid overflow.
- Searching for first → move left
- Searching for last → move right
---

<!-- #endregion -->
