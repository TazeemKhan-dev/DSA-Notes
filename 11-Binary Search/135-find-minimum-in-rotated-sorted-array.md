<!-- #region 135-Find Minimum in Rotated Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q135: Find Minimum in Rotated Sorted Array</h1>

## 1. Problem Understanding

- You are given a sorted array that has been rotated between 1 and n times.
- Examples:
- Original: [1,2,3,4,5]
- Rotated: [3,4,5,1,2]
- Your task:
- Find the minimum element in the rotated array.
- Must run in O(log n) → Binary Search.
---

## 2. Constraints

- 1 <= n <= 5000
- Numbers can be negative.
- All elements are unique
- Must use binary search → no linear scan.
---

## 3. Edge Cases

- Array not rotated → minimum at index 0.
- Example: [1,2,3,4]
- Rotation by n times also gives original array.
- Minimum might be at the boundaries.
- Example: [2,3,4,5,1]
- Very small arrays: size 1, size 2.
- Already sorted means return nums[0].
---

## 4. Examples

```text
Example 1

Input:

[3,4,5,1,2]


Output:

1

Example 2

Input:

[4,5,6,7,0,1,2]


Output:

0
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Scan)

**Idea:**
- The minimum of any array is simply the smallest element.
- Directly scan the array.

**💭 Intuition Behind the Approach:**
- A sorted rotated array still contains a minimum somewhere.
- You inspect all elements to find it.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Because scanning all elements.
- 💾 Space Complexity
  * O(1)
  * Only one variable used.

### Approach 2: Slightly Better (Check Adjacent Break Point)

**Idea:**
- In a rotated array, the minimum occurs at the break point where:
- nums[i] < nums[i-1]
- Scan and check only such break.

**Java Code:**
```java
public static int findMin(int[] nums) {
    int n = nums.length;
    for (int i = 1; i < n; i++) {
        if (nums[i] < nums[i - 1]) return nums[i];
    }
    return nums[0];
}
```

**💭 Intuition Behind the Approach:**
- The array is sorted except one pivot point.
- Minimum sits right after the point where order breaks.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) worst case
  * If array not rotated.
- 💾 Space Complexity
  * O(1)

### Approach 3: Binary Search — O(log n)

**Idea:**
- Use binary search to find the pivot where rotation happened.
- Observations:
  * If nums[mid] > nums[right], the minimum is to the right.
  * Else → minimum is to the left or at mid.

**Steps:**
- Set l = 0, r = n-1.
- While l < r:
  * Compute mid.
  * If nums[mid] > nums[r] → search right half.
  * Else → search left half (including mid).
- At the end l = index of minimum.

**Java Code:**
```java
public static int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;

    while (l < r) {
        int mid = l + (r - l) / 2;

        if (nums[mid] > nums[r]) {
            l = mid + 1;     // min is to the right
        } else {
            r = mid;         // min is at mid or left side
        }
    }

    return nums[l]; // l is the index of minimum
}
```

**💭 Intuition Behind the Approach:**
- When nums[mid] > nums[r], right half is unsorted → min lies there.
- When nums[mid] <= nums[r], right half is sorted → min cannot be in sorted region except possibly mid.
- Binary search narrows search space towards the pivot point.
- This is a classic Binary Search on answer space, not on presence.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Because each iteration halves the search space.
- 💾 Space Complexity
  * O(1)
  * Only two pointers.

---

## 6. Justification / Proof of Optimality

- The array is partially sorted.
- Binary search is the perfect fit when:
  * There is a monotonic pattern.
  * There is a pivot.
  * We can eliminate half of the array on each decision.
- The condition nums[mid] > nums[r] is the key that tells us which side the pivot lies on.
---

## 7. Variants / Follow-Ups

- Find index of minimum instead of value.
- Find number of times rotated → index of min is the rotation count.
- Rotated array with duplicates → trickier binary search.
- Search element in rotated sorted array.
---

## 8. Tips & Observations

- Always compare with the rightmost element for easier logic.
- If whole array is already sorted → return nums[0].
- If duplicates exist:
  * Need to shrink boundaries carefully.
- Rotated sorted array problems ALWAYS revolve around:
  * Detecting pivot
  * Searching on monotonic segments
---

<!-- #endregion -->
