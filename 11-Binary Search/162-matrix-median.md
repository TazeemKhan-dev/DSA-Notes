<!-- #region 162-Matrix Median -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q162: Matrix Median</h1>

## 1. Problem Understanding

- You are given a matrix where:
- Each row is sorted individually.
- The matrix is not globally sorted.
- Total number of elements (N * M) is odd.
- You must find the median of all elements if collected in a sorted list.
---

## 2. Constraints

- 1 ≤ N, M ≤ 10^5
- 1 ≤ N*M ≤ 10^6 (so total elements up to 1 million)
- Each row sorted
- 1 ≤ matrix[i][j] ≤ 10^9
- N*M is always odd → median is a single element
- You CANNOT flatten → too slow/memory heavy
---

## 3. Edge Cases

- N = 1 → median is median of single array
- Rows of different sizes (not here; all have M columns)
- All small or all large values
- Duplicate numbers
- Matrix values not globally sorted
---

## 4. Examples

```text
Example 1

Input:
[1 4 9]
[2 5 6]
[3 7 8]

Median = 5


Example 2

[1 3 8]
[2 3 4]
[1 2 5]

Flatten: 1 1 2 2 3 3 4 5 8 → median = 3
```

---

## 5. Approaches

### Approach 1: Flatten & Sort (Brute Force)

**Idea:**
- Create an array of size N*M
- Copy all elements
- Sort it
- Return middle element

**💭 Intuition Behind the Approach:**
- Works but wasteful.

**Complexity (Time & Space):**
- Time: O(N*M log(N*M))
- Space: O(N*M)

### Approach 2: Min-Heap / K-way merge

**Idea:**
- Use a min-heap to merge rows (each sorted)
- Pop (N*M)/2 elements
- Return next element

**Complexity (Time & Space):**
- Time: O((N*M) log N)
- Space: O(N)
- Still too slow for 10^6 elements.

### Approach 3: Optimal): Binary Search on Answer

**Idea:**
- Since each row is sorted, we can count:
  * How many elements in matrix are ≤ mid?
- If we can count that efficiently, we can binary search the value of the median, not position.
- Median position (0-indexed):
  * k = (N*M)/2
- We need the smallest value x such that:
  * countLessOrEqual(x) > k
- Key Insight
- Every row sorted → count elements ≤ mid in a row using binary search.

**Steps:**
- Compute:
  * low = min element (first elements of each row)
  * high = max element (last elements of each row)
- While low <= high:
  * mid = low + (high - low) / 2
  * count = total elements ≤ mid across all rows
  * If count > k → median might be mid → move left
  * Else → move right
- Return low (the smallest element satisfying count > k)

**Java Code:**
```java
public class Solution {

    public int findMedian(int[][] mat) {
        int n = mat.length;
        int m = mat[0].length;
        int low = Integer.MAX_VALUE, high = Integer.MIN_VALUE;

        // find global min and max
        for(int i = 0; i < n; i++){
            low = Math.min(low, mat[i][0]);
            high = Math.max(high, mat[i][m - 1]);
        }

        int k = (n * m) / 2; // median index

        while(low <= high){
            int mid = low + (high - low) / 2;

            int count = 0;

            // count elements <= mid in each row
            for(int i = 0; i < n; i++){
                count += upperBound(mat[i], mid);
            }

            if(count > k){
                high = mid - 1;
            }
            else{
                low = mid + 1;
            }
        }

        return low;
    }

    // number of elements <= x in sorted array
    private int upperBound(int[] row, int x){
        int low = 0, high = row.length - 1, ans = row.length;

        while(low <= high){
            int mid = low + (high - low) / 2;
            if(row[mid] > x){
                ans = mid;
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }
}
```

**💭 Intuition Behind the Approach:**
- Median value is the K-th smallest element.
- Instead of merging arrays, we binary search over the value range.
- Each mid represents a candidate value for median.
- Using binary search in each row (sorted), counting is efficient.
- Because the matrix isn’t globally sorted, but each row is, you can treat each row independently during count.
- This is a classic BSOA pattern.

**Complexity (Time & Space):**
- Time:
  * Counting each mid: O(N * log M)
  * Binary search over value range → about 32 iterations (log of 1e9)
  * Total: O(N * log M * log(1e9))
  * For constraints (N*M ≤ 10^6), this is optimal.
- Space:
  * O(1)

---

## 6. Justification / Proof of Optimality

- Each row sorted → count elements ≤ x using binary search.
- Counting is monotonic → if mid is large, count increases.
- Because global sorted structure is not present, you cannot do one binary search on a flattened index.
- BSOA is the only scalable method.
---

## 7. Variants / Follow-Ups

- Median of row-wise sorted matrix (same problem)
- Kth smallest element in a sorted matrix
- Kth smallest in sorted lists
- Find first element greater than X in matrix
- Median in a stream (heaps)
---

## 8. Tips & Observations

- Always binary search on value, not index, when multi-row sorted only row-wise.
- Use upperBound to count ≤ mid.
- Lowest possible matrix element → low
- Highest possible → high
- Final answer = low
- Total length is odd → no need to average two middle values.

- **⚠️ Pitfalls**
    - Using flatten or heap → TLE
    - Using full matrix min & max incorrectly
    - Mistaking for 2D sorted full matrix (it's not)
    - Confusing median index condition
    - Using lowerBound instead of upperBound incorrectly
---

<!-- #endregion -->
