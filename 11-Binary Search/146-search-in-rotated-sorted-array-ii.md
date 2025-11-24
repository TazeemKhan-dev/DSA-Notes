<!-- #region 146-Search in rotated sorted array-II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q146: Search in rotated sorted array-II</h1>

## 1. Problem Understanding

- You are given a rotated sorted array (in ascending order) that may contain duplicates.
- Example of rotated sorted array:
  * [7,8,1,2,3,3,3,4,5,6]
- You need to check if target k exists in the array.
- Duplicates make this problem trickier because the usual binary-search logic may fail when nums[mid] == nums[low] == nums[high].
- Return:
  * True → if target exists
  * False → if target does not exist
---

## 2. Constraints

- 1 <= nums.length <= 10000
- Elements may have duplicates
- Array is rotated at some pivot
- -10^4 <= nums[i], k <= 10^4
- Time target is better than O(N) but worst-case O(N) may occur due to duplicates
---

## 3. Edge Cases

- Entire array consists of duplicates → [2,2,2,2,2]
- Target equals pivot element
- Target is in left sorted part
- Target is in right sorted part
- Single element array
- Rotation at index 0 (no actual rotation)
---

## 4. Examples

```text
nums = [7,8,1,2,3,3,3,4,5,6], k=3
👉 Output: True

nums = [7,8,1,2,3,3,3,4,5,6], k=10
👉 Output: False

nums = [7,8,1,2,3,3,3,4,5,6], k=7
👉 Output: True
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Steps:**
- Iterate from start to end.
- If num == k → return true.

**Java Code:**
```java
public boolean searchBrute(int[] nums, int k) {
    for (int n : nums) {
        if (n == k) return true;
    }
    return false;
}
```

**Complexity (Time & Space):**
- Time: O(N) → scans all elements
- Space: O(1) → no extra data structures

### Approach 2: Modified Binary Search for Rotated Array with Duplicates

**Idea:**
- Use binary search but handle duplicates by shrinking boundaries.

**Steps:**
- While l <= r:
  * Compute mid.
  * If nums[mid] == k → return true.
  * Problem: duplicates like 2,2,2,2,2
  * solution → move l++ and r--
- Otherwise determine which part is sorted:
  * If left part sorted:
    * If nums[l] <= k < nums[mid] → search left
    * else → search right
  * Else right part sorted:
    * If nums[mid] < k <= nums[r] → search right
    * else → search left

**Java Code:**
```java
class Solution {
    public boolean search(int[] nums, int k) {
        int l = 0, r = nums.length - 1;

        while (l <= r) {
            int mid = l + (r - l) / 2;

            if (nums[mid] == k) return true;

            // Handle duplicates
            if (nums[l] == nums[mid] && nums[mid] == nums[r]) {
                l++;
                r--;
                continue;
            }

            // Left side sorted
            if (nums[l] <= nums[mid]) {
                if (nums[l] <= k && k < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            }
            // Right side sorted
            else {
                if (nums[mid] < k && k <= nums[r]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }

        return false;
    }
}
```

**💭 Intuition Behind the Approach:**
- Rotated array has one sorted half.
- We identify the sorted half.
- Check if the target falls inside that sorted range.
- Duplicates break the sorted structure, so we shrink from both ends (l++, r--).
- This keeps the search moving and eventually resolves ambiguity.

**Complexity (Time & Space):**
- Time:
  * Best / average: O(log N)
  * Reason: Binary search halves the array.
  * Worst-case: O(N)
  * Reason: When duplicates equalize boundaries (l == mid == r), we shrink one step at a time.
- Space: O(1)
  * Reason: Only pointers used.

---

## 6. Justification / Proof of Optimality

- Binary search is valid because:
  * Rotated sorted arrays always have one sorted half.
  * Even with duplicates, eliminating ambiguous boundary duplicates leads to eventually clear sorted zones.
  * Each step reduces the search space.
---

## 7. Variants / Follow-Ups

- Search in Rotated Array without duplicates (LC 33)
- Find minimum in rotated sorted array
- Find rotation pivot index
- Count rotations in an array
---

## 8. Tips & Observations

- If nums[mid] == nums[l] == nums[r] → shrink both ends
- Always check which half is sorted
- Target range check decides which direction to move
- Worst case O(N) only when many duplicates exist
---

<!-- #endregion -->
