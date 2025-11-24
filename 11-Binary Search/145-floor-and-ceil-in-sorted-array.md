<!-- #region 145-Floor and Ceil in Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q145: Floor and Ceil in Sorted Array</h1>

## 1. Problem Understanding

- Given a sorted array nums and an integer x, find:
  * Floor(x) = largest element <= x
  * Ceil(x) = smallest element >= x
- If no valid floor/ceil exists → return -1.
- This is a classic binary search variation.
---

## 2. Constraints

- 1 <= nums.length <= 100000
- 0 < nums[i], x < 100000
- Array is sorted in ascending order.
---

## 3. Edge Cases

- x is smaller than all elements → floor = -1
- x is greater than all elements → ceil = -1
- x exactly matches an element → floor = ceil = x
- Duplicate values present
- Array of size 1
---

## 4. Examples

```text
nums = [3,4,4,7,8,10], x=5
👉 floor = 4, ceil = 7

nums = [3,4,4,7,8,10], x=8
👉 floor = 8, ceil = 8

nums = [2,4,6,8,10,12,14], x=1
👉 floor = -1, ceil = 2
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Search)

**Idea:**
- Scan the whole array and track:
- largest value <= x
- smallest value >= x

**Steps:**
- Traverse the array once.
- Compare each element with x.
- Update floor or ceil accordingly.

**Java Code:**
```java
public static int[] floorCeilBrute(int[] nums, int x) {
    int floor = -1, ceil = -1;

    for (int num : nums) {
        if (num <= x) floor = num;
        if (num >= x && ceil == -1) ceil = num;
    }

    return new int[]{floor, ceil};
}
```

**💭 Intuition Behind the Approach:**
- We simply check every element.
- If it’s ≤ x, it can be a floor.
- If it’s ≥ x and no ceil yet, it's the smallest so far.

**Complexity (Time & Space):**
- Time: O(N) → we traverse all elements
  * Reason: Every element is checked once.
- Space: O(1) → no extra storage

### Approach 2: Binary Search (Optimal)

**Idea:**
- Use binary search to:
  * Find floor = rightmost position where nums[i] <= x
  * Find ceil = leftmost position where nums[i] >= x

**Steps:**
- Use two modified binary searches:
  * One for floor
  * One for ceil

**Java Code:**
```java
class Solution {

    public int floor(int[] nums, int x) {
        int s = 0, e = nums.length - 1;
        int ans = -1;

        while (s <= e) {
            int mid = s + (e - s) / 2;

            if (nums[mid] <= x) {
                ans = nums[mid];
                s = mid + 1;
            } else {
                e = mid - 1;
            }
        }
        return ans;
    }

    public int ceil(int[] nums, int x) {
        int s = 0, e = nums.length - 1;
        int ans = -1;

        while (s <= e) {
            int mid = s + (e - s) / 2;

            if (nums[mid] >= x) {
                ans = nums[mid];
                e = mid - 1;
            } else {
                s = mid + 1;
            }
        }
        return ans;
    }

    public int[] getFloorAndCeil(int[] nums, int x) {
        return new int[]{floor(nums, x), ceil(nums, x)};
    }
}
```

**💭 Intuition Behind the Approach:**
- Binary search helps reduce search space by half each step.
  * For floor, we move right whenever nums[mid] <= x because a better (bigger) floor may exist.
  * For ceil, we move left whenever nums[mid] >= x because a smaller (closer) ceil may exist.
- We keep storing the candidate answer while narrowing down.

**Complexity (Time & Space):**
- Time: O(log N)
  * Reason: Each search halves the array.
- Space: O(1)
  * Reason: Only pointers and variables used.

---

## 6. Justification / Proof of Optimality

- The optimal binary-search approach is correct because:
  * The array is sorted → allows monotonic decisions.
  * Floor/ceil conditions (<= x, >= x) allow pruning half of the search space.
  * By storing candidate answers, we guarantee closest values.
---

## 7. Variants / Follow-Ups

- Find just floor
- Find just ceil
- Lower Bound (>= x)
- Upper Bound (> x)
- Insert Position (LeetCode 35)
- Count occurrences of a number using LB + UB
---

## 8. Tips & Observations

- Floor ⇢ look for ≤ x
- Ceil ⇢ look for ≥ x
- Floor always moves right on match
- Ceil always moves left on match
- If no candidate found → answer = -1
- <= and >= are the key comparisons for these problems.
---

<!-- #endregion -->
