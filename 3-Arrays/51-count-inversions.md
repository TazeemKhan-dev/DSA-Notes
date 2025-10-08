<!-- #region 51-Count Inversions -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q51: Count Inversions</h1>

## 1. Problem Understanding

- Given an integer array nums, count the number of inversions.
- An inversion is a pair (i, j) such that:
  * i < j
  * nums[i] > nums[j]
- Interpretation: measures how far the array is from being sorted.
   * Sorted array → 0 inversions
   * Descending array → maximum inversions
---

## 2. Constraints

- 1 ≤ nums.length ≤ 10^5
- -10^4 ≤ nums[i] ≤ 10^4
---

## 3. Edge Cases

- Already sorted array → 0 inversions
- Fully descending array → maximum inversions
- Array with duplicate numbers → count all valid (i, j) pairs
- Single element array → 0 inversions
---

## 4. Examples

```text
Example 1:
Input: [2, 3, 7, 1, 3, 5]
Output: 5
Explanation (inversions):
(0,3) → 2 > 1
(1,3) → 3 > 1
(2,3) → 7 > 1
(2,4) → 7 > 3
(2,5) → 7 > 5

Example 2:
Input: [-10, -5, 6, 11, 15, 17]
Output: 0
Explanation: already sorted → no inversions

Example 3:
Input: [9, 5, 4, 2]
Output: 6
Explanation: all possible pairs are inversions
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check every pair (i, j) with i < j and count if nums[i] > nums[j].

**Steps:**
- Initialize count = 0
- Iterate i from 0 to n-2
- Iterate j from i+1 to n-1
- If nums[i] > nums[j] → increment count

**Java Code:**
```java
class CountInversionsBrute {
    public long countInversions(int[] nums){
        int n = nums.length;
        long count = 0;
        for(int i=0;i<n-1;i++){
            for(int j=i+1;j<n;j++){
                if(nums[i] > nums[j]) count++;
            }
        }
        return count;
    }
}
```

**Complexity (Time & Space):**
- Time: O(n²)
- Space: O(1)

### Approach 2: Merge Sort Based (Optimal)

**Idea:**
- Use modified merge sort to count inversions while merging.
- If nums[i] > nums[j] in merge step → all remaining elements in left half also form inversions with nums[j].

**Steps:**
- Implement standard merge sort with a merge function
- During merge:
- If left[i] ≤ right[j] → no inversion
- Else → inversion count += remaining elements in left array
- Return total inversions

**Java Code:**
```java
class CountInversionsMergeSort {
    public long countInversions(int[] nums){
        return mergeSort(nums, 0, nums.length - 1);
    }
    
    private long mergeSort(int[] nums, int left, int right){
        long inv = 0;
        if(left < right){
            int mid = left + (right-left)/2;
            inv += mergeSort(nums, left, mid);
            inv += mergeSort(nums, mid+1, right);
            inv += merge(nums, left, mid, right);
        }
        return inv;
    }
    
    private long merge(int[] nums, int left, int mid, int right){
        int n1 = mid - left + 1;
        int n2 = right - mid;
        int[] L = new int[n1];
        int[] R = new int[n2];
        for(int i=0;i<n1;i++) L[i] = nums[left+i];
        for(int j=0;j<n2;j++) R[j] = nums[mid+1+j];
        
        int i=0,j=0,k=left;
        long inv = 0;
        while(i<n1 && j<n2){
            if(L[i] <= R[j]){
                nums[k++] = L[i++];
            } else {
                nums[k++] = R[j++];
                inv += (n1 - i); // Remaining elements in left are inversions
            }
        }
        while(i<n1) nums[k++] = L[i++];
        while(j<n2) nums[k++] = R[j++];
        return inv;
    }
}
```

**Complexity (Time & Space):**
- Time: O(n log n)
- Space: O(n)

---

## 6. Justification / Proof of Optimality

- Brute Force → simple but slow for large arrays
- Merge Sort → efficient for large arrays, counts inversions during sorting
- Merge Sort is preferred for constraints n ≤ 10^5
---

## 7. Variants / Follow-Ups

- Count inversions modulo some number
- Count inversions in a streaming array
- Find k-th inversion pair
---

## 8. Tips & Observations

- Inversions = minimum number of swaps required to sort array
- Merge sort counting uses divide-and-conquer effectively
- Always check for long type if n is large to avoid overflow
---

<!-- #endregion -->
