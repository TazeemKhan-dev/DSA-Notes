<!-- #region 155-Median of 2 sorted arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q155: Median of 2 sorted arrays</h1>

## 1. Problem Understanding

- You are given two sorted arrays, arr1 (size m) and arr2 (size n).
- Your task:
  * Find the median of the combined array, without actually merging them.
- Median definition:
  * If total length is odd → middle element
  * If total length is even → average of the two middle elements
- This is a classic hard problem based on binary search on partitions, NOT BSOA.
---

## 2. Constraints

- 0 ≤ m, n ≤ 1000
- 1 ≤ m + n ≤ 2000
- Arrays individually sorted
- Possible negative numbers
- brute O(m+n) OK for constraints, but binary search O(log(min(m,n))) is optimal
---

## 3. Edge Cases

- One array empty: median = median of the other
- Very small arrays (size 1,2)
- All elements of arr1 < all elements of arr2
- All elements of arr1 > all elements of arr2
- Overlapping ranges
- Even vs odd total length
---

## 4. Examples

```text
Example 1
arr1 = [2,4,6]
arr2 = [1,3,5]
Merged: [1,2,3,4,5,6]
Median: (3+4)/2 = 3.5

Example 2
arr1 = [2,4,6]
arr2 = [1,3]
Merged: [1,2,3,4,6]
Median: 3
```

---

## 5. Approaches

### Approach 1: Brute Force Merge

**Idea:**
- Merge like merge-sort into a new array and pick median.

**Steps:**
- Use two pointers
- Build merged array in O(m+n)
- If total odd → return mid
- If even → return average(mid1, mid2)

**Java Code:**
```java
public double findMedianSortedArrays(int[] a, int[] b) {
    int m = a.length, n = b.length;
    int[] merged = new int[m+n];
    int i=0,j=0,k=0;

    while(i<m && j<n) {
        if(a[i] < b[j]) merged[k++] = a[i++];
        else merged[k++] = b[j++];
    }
    while(i<m) merged[k++] = a[i++];
    while(j<n) merged[k++] = b[j++];

    int total = m+n;
    if(total % 2 == 1) return merged[total/2];
    return (merged[total/2] + merged[total/2 - 1]) / 2.0;
}
```

**💭 Intuition Behind the Approach:**
- Simulate sorted merge, simple and intuitive.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(m+n)
  * Because merging touches every element.
- 💾 Space Complexity
  * O(m+n)
  * Due to merged array.

### Approach 2: Optimal Binary Search on Smaller Array (Partition Method)

**Idea:**
- We do NOT merge arrays.
- We binary search a partition in the smaller array so that:
  * Left side contains the first (m+n+1)/2 elements of the merged array.
- We enforce:
  * max(left1, left2) <= min(right1, right2)
- If valid → we found correct partition.
- Then median is computed from edges of the partitions.

**Steps:**
- Ensure arr1 is always the smaller array
- Binary search on cut position of arr1
- Let:
  * cut1 = mid
  * cut2 = (total+1)/2 - cut1
- Compute:
  * left1  = (cut1 == 0 ? -∞ : arr1[cut1-1])
  * right1 = (cut1 == m ? +∞ : arr1[cut1])
  * left2  = (cut2 == 0 ? -∞ : arr2[cut2-1])
  * right2 = (cut2 == n ? +∞ : arr2[cut2])
- If:
  * left1 <= right2 AND left2 <= right1
- → correct partition
- → median = average of max(left1,left2) and min(right1,right2)
- Else adjust binary search range.

**Java Code:**
```java
class Solution {
    public double findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length, n = B.length;

        // Ensure A is the smaller array
        if (m > n) return findMedianSortedArrays(B, A);

        int low = 0, high = m;

        while (low <= high) {
            int cutA = low + (high - low) / 2;
            int cutB = (m + n + 1) / 2 - cutA;

            int leftA = (cutA == 0) ? Integer.MIN_VALUE : A[cutA - 1];
            int rightA = (cutA == m) ? Integer.MAX_VALUE : A[cutA];

            int leftB = (cutB == 0) ? Integer.MIN_VALUE : B[cutB - 1];
            int rightB = (cutB == n) ? Integer.MAX_VALUE : B[cutB];

            if (leftA <= rightB && leftB <= rightA) {
                // Correct partition
                if ((m + n) % 2 == 0)
                    return (Math.max(leftA, leftB) + Math.min(rightA, rightB)) / 2.0;
                else
                    return Math.max(leftA, leftB);
            }
            else if (leftA > rightB) {
                // Need to move cutA left
                high = cutA - 1;
            } else {
                // Need to move cutA right
                low = cutA + 1;
            }
        }

        return 0.0; // unreachable
    }
}
```

**💭 Intuition Behind the Approach:**
- A valid partition ensures:
  * all elements in left half are ≤ all in right half
  * At that point, the median is determined only from the edges of these halves.
- Binary search guarantees O(log(min(m,n))).

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log(min(m,n)))
  * Because we binary search on smaller array size.
- 💾 Space Complexity
  * O(1)
  * Only variables, no extra storage.

---

## 6. Justification / Proof of Optimality

- A valid median split must satisfy sorted ordering between left and right partitions.
- The partition approach guarantees that the left side always contains exactly half of the combined sorted array.
- Because both arrays are sorted, binary searching the cut position ensures monotonic behavior.
- The median depends only on edges of partitions, not on full merged array.
- This method is optimal and mathematically proven; used in all major interview solutions (Google, Meta, Amazon).
---

## 7. Variants / Follow-Ups

- Find kth element of two sorted arrays
- Median of k sorted arrays
- Weighted median
- Median in data stream (Heaps)
---

## 8. Tips & Observations

- Always binary search the smaller array to avoid overflow / OOB.
- Use sentinels (±∞) at boundaries to simplify logic.
- Understand partition sizes:
- (m+n+1)/2 ensures correct handling of odd/even lengths.
- No merging required → optimal speed.

- **🕳️ Pitfalls**
    - Forgetting to handle empty arrays properly
    - Using wrong boundary sentinel values
    - Confusing cut positions with indexes
    - Returning index instead of value (here we return value)
    - Overflow if not using +∞ and −∞ sentinels
---

<!-- #endregion -->
