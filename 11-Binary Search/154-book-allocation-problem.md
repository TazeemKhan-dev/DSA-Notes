<!-- #region 154-Book Allocation Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q154: Book Allocation Problem</h1>

## 1. Problem Understanding

- You are given:
- nums[i] = pages in i-th book
- m = number of students
- You must allocate contiguous books
- Every student must receive at least 1 book
- A book cannot be split
- Goal → Minimize the maximum pages assigned to any student
- This is a classic Binary Search on Answer (BSOA) minimization problem.
- We want to find the minimum possible maximum pages a student reads.
---

## 2. Constraints

- 1 <= n, m <= 10^4
- 1 <= nums[i] <= 10^5
- Large values → greedy O(n) + binary search O(log(sum)) required
- Contiguous allocation required
- m > n → impossible (not enough books to give 1 per student)
---

## 3. Edge Cases

- If m > n → return -1
- Single book → answer is that book
- Single student → answer = sum of array
- All books same pages
- Very large pages → use long for sum
---

## 4. Examples

```text
Example 1
nums = [12, 34, 67, 90], m = 2
Output: 113

Example 2
nums = [25, 46, 28, 49, 24], m = 4
Output: 71

Example 3
nums = [15, 17, 20], m = 2
Output: -1 (when m > n)
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try every possible maximum pages value from:
  * max(nums) → sum(nums)
- and check feasibility.

**Steps:**
- For each X in the range
- Try to allocate books greedily
- If at any point allocations ≤ m, return X

**Java Code:**
```java
import java.util.*;

class Solution {
    /*Function to count the number of 
    students required given the maximum 
    pages each student can read*/
    private int countStudents(int[] nums, int pages) {
        // Size of array
        int n = nums.length;
        
        int students = 1;
        int pagesStudent = 0;
        
        for (int i = 0; i < n; i++) {
            if (pagesStudent + nums[i] <= pages) {
                // Add pages to current student
                pagesStudent += nums[i];
            } else {
                // Add pages to next student
                students++;
                pagesStudent = nums[i];
            }
        }
        return students;
    }
    
    /* Function to allocate the book to ‘m’ 
    students such that the maximum number 
    of pages assigned to a student is minimum*/
    public int findPages(int[] nums, int m) {
        int n = nums.length;
        
        // Book allocation impossible
        if (m > n) return -1;

        // Calculate the range for search
        int low = Integer.MIN_VALUE;
        int high = 0;
        for(int i = 0; i < n; i++){
            low = Math.max(low, nums[i]);
            high = high + nums[i];
        }

        // Linear search for minimum maximum pages
        for (int pages = low; pages <= high; pages++) {
            if (countStudents(nums, pages) <= m) {
                return pages;
            }
        }
        return low;
    }

    public static void main(String[] args) {
        int[] arr = {25, 46, 28, 49, 24};
        int m = 4;

        // Create an instance of the Solution class
        Solution sol = new Solution();

        int ans = sol.findPages(arr, m);

        // Output the result
        System.out.println("The answer is: " + ans);
    }
}
```

**💭 Intuition Behind the Approach:**
- Check all potential answers manually until a feasible one is found.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O((sum(nums) - max(nums)) * n) → impossible for large constraints
- 💾 Space Complexity
  * O(1)

### Approach 2: Binary Search on Answer (Correct Minimization using <=)

**Idea:**
- The answer lies in:
  * low = max(nums) (minimum max pages must be ≥ largest book)
  * high = sum(nums) (one student reads everything)
- Binary search for the smallest mid such that allocation is possible.

**Steps:**
- Set search space:
  * low = max(nums)
  * high = sum(nums)
- For each mid, check feasibility:
  * Allocate books to student until sum > mid
  * Then start new student
- If number of students needed ≤ m → feasible
- For minimization:
  * if feasible → ans = mid, high = mid - 1
  * else        → low = mid + 1

**Java Code:**
```java
class Solution {
    public int findPages(int[] nums, int m) {

        int n = nums.length;
        if (m > n) return -1;

        int low = 0, high = 0;

        for (int x : nums) {
            low = Math.max(low, x);
            high += x;
        }

        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isPossible(nums, m, mid)) {
                ans = mid;
                high = mid - 1;   // minimize
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }

    private boolean isPossible(int[] nums, int m, int limit) {
        int studentCount = 1;
        int pages = 0;

        for (int x : nums) {
            if (pages + x <= limit) {
                pages += x;
            } else {
                studentCount++;
                pages = x;

                if (studentCount > m) return false;
            }
        }
        return true;
    }
}
```

**💭 Intuition Behind the Approach:**
- We simulate “if each student can read at most mid pages”.
- If it's possible with ≤ m students → we try to reduce mid.
- If not → we increase mid.
- The monotonic property of feasibility makes binary search valid.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n * log(sum(nums)))
  * Why:
  * Each binary search step checks feasibility in O(n)
- 💾 Space Complexity
  * O(1)
  * Just counters and variables.

---

## 6. Justification / Proof of Optimality

- The solution is justified because:
- ✔ 1. The problem has a monotonic feasibility behavior
  * If we can allocate books such that no student reads more than X pages,
  * then we can also allocate for any value > X.
  * This monotonic pattern:
  * F F F F T T T T
  * is required for binary search.
- ✔ 2. The minimum possible maximum pages is always between:
- max(nums)  and  sum(nums)
  * You cannot assign less than the largest single book → lower bound
  * You cannot assign more than total pages → upper bound
  * This forms a valid search space.
- ✔ 3. Greedy feasibility check is optimal
  * Assigning books left → right ensures:
  * Each student gets contiguous books
  * We minimize splitting
  * We count how many students are needed for limit = mid
  * If students needed ≤ m → allocation is possible.
  * The greedy stays valid because:
  * Adding a new student only when needed is always optimal
  * Reordering books is not allowed
- ✔ 4. Binary Search narrows down the exact threshold
  * We search for the smallest mid for which allocation is possible.
  * This is a classic minimization BSOA, so:
  * if possible(mid):
      * ans = mid
      * high = mid - 1
  * else:
      * low = mid + 1
- This guarantees finding the minimum maximum pages.
---

## 7. Variants / Follow-Ups

- Allocate Books (GFG version)
- Painter Partition Problem
- Split Array Largest Sum (LeetCode Hard)
- Ship Packages Within D Days
- Minimize Max Distance to Gas Stations (similar feasibility)
---

## 8. Tips & Observations

- Always use low = max(nums)
- Always use high = sum(nums)
- Always check m > n early
- Feasibility always follows monotonic increasing property
- Use long for sum in bigger constraints
- The key idea: minimizing the maximum load across partitions

- **🕳️ Pitfalls**
    - ❌ Starting low = 0 → WRONG
    - ❌ Ignoring m > n → WRONG
    - ❌ Not resetting pages correctly when switching students
    - ❌ Using < instead of <= in binary search (breaks edge cases)
    - ❌ Handling feasibility incorrectly when pages == limit
    - ❌ Using sorted array (NOT required — order must be preserved)
---

<!-- #endregion -->
