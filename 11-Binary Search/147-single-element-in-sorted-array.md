<!-- #region 147-Single element in sorted array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q147: Single element in sorted array</h1>

## 1. Problem Understanding

- You're given a sorted array where:
  * Every element appears exactly twice
  * Except one element, which appears only once
  * The array is sorted in non-decreasing order
  * You must return that single element
- Example:
  * [1,1,2,2,3,3,4,5,5] → answer is 4
- The challenge: do it in O(log N) time and O(1) space.
---

## 2. Constraints

- 1 <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- Array is sorted
- Exactly one element is single
- Others appear twice
---

## 3. Edge Cases

- Single-element array → answer is nums[0]
- Single element at beginning
- Single element at end
- Single element in the middle
- Array always has odd length
- Patterns like:
  * [unique, a, a, b, b]
  * [a, a, b, b, unique]
  * [a, a, unique, b, b]
---

## 4. Examples

```text
Example 1

Input:
[1,1,2,2,3,3,4,5,5,6,6]
Output:
4

Example 2

Input:
[1,1,3,5,5]
Output:
3

Example 3

Input:
[1,1,2,2,3,3,4,4,5,5,6,6,7]
Output:
7
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Scan)

**Idea:**
- Check pairs sequentially. When a pair breaks, that's the single element.

**Steps:**
- Loop through array in steps of 2
- If nums[i] != nums[i+1] → return nums[i]
- If no mismatch found → last element is the answer

**Java Code:**
```java
public int singleNonDuplicate(int[] nums) {
    for (int i = 0; i < nums.length - 1; i += 2) {
        if (nums[i] != nums[i + 1]) return nums[i];
    }
    return nums[nums.length - 1];
}
```

**💭 Intuition Behind the Approach:**
- Pairs must always start at even indices until the single element appears.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) because you scan sequentially
  * Why → checking pairs one-by-one
- 💾 Space Complexity
  * O(1) → no extra memory

### Approach 2: Optimal Binary Search (Even-Index Trick)

**Idea:**
- Before the single element:
- Pairs are:
  * (even, odd)
  * (even, odd)
- After the single element:
  * The pairing pattern shifts.
- We use this property to locate the break.

**Steps:**
- Use binary search on the index
- Ensure mid is even
- Compare nums[mid] with nums[mid + 1]
- If they match → move right
- If not → move left (single is at mid or before)

**Java Code:**
```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int low = 0, high = nums.length - 1;

        while (low < high) {
            int mid = low + (high - low) / 2;

            if (mid % 2 == 1) mid--; // convert mid to even

            if (nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            } else {
                high = mid;
            }
        }

        return nums[low];
    }
}
```

**💭 Intuition Behind the Approach:**
- Pairs MUST be aligned as (even, odd) until they are broken by the single element.
- The first index where this alignment breaks is the answer.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log N)
  * Why → range halves every iteration
- 💾 Space Complexity
  * O(1)

### Approach 3: Optimal (Mid-Neighbor Check Pattern)

**Idea:**
- First check edge-case positions
- Then use binary search in the range [1, n-2]
- The single element must be ≠ both neighbors
- Otherwise, use parity logic to decide direction

**Java Code:**
```java
import java.util.*;

class Solution {
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length;

        // Edge cases
        if (n == 1) return nums[0];
        if (nums[0] != nums[1]) return nums[0];
        if (nums[n - 1] != nums[n - 2]) return nums[n - 1];

        int low = 1, high = n - 2;

        while (low <= high) {
            int mid = (low + high) / 2;

            // Single element found
            if (nums[mid] != nums[mid + 1] && nums[mid] != nums[mid - 1]) {
                return nums[mid];
            }

            // Left part is paired correctly
            if ((mid % 2 == 1 && nums[mid] == nums[mid - 1]) ||
               (mid % 2 == 0 && nums[mid] == nums[mid + 1])) {

                low = mid + 1;  // go right
            } 
            else {
                high = mid - 1; // go left
            }
        }

        return -1; // should never reach
    }
}
```

**💭 Intuition Behind the Approach:**
- A valid pair always consumes two elements
- If the pairing around mid matches the parity pattern, the single element is on the right
- If the pairing is broken early, the single element is on the left

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log N)
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Both binary search solutions are correct because:
- The array structure is predictable
- The single element disrupts the pairing pattern
- Binary search efficiently locates this disruption
- No extra space required
- Sorted array → monotonic behavior exists in pair alignment
---

## 7. Variants / Follow-Ups

- Find the single element when all others appear three times
- Find the single element in unsorted array
- Bit manipulation version (O(N) but constant-space)
---

## 8. Tips & Observations

- Pairs ALWAYS start at even index until the unique element
- After the unique element, pairing shifts
- If you’re confused, print index parities during dry run
- The even-index trick is the cleanest and most common solution
- Your neighbor-check solution is also valid but more verbose
---

<!-- #endregion -->
