<!-- #region 52-Reverse Pairs -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q52: Reverse Pairs</h1>

## 1. Problem Understanding

- Given an integer array nums, count the number of reverse pairs.
- A reverse pair (i, j) satisfies:
  * 0 <= i < j < nums.length
  * nums[i] > 2 * nums[j]
- Conceptually, it’s similar to counting inversions but with a multiplicative condition.
---

## 2. Constraints

- 1 <= nums.length <= 5 * 10^4
- -2^31 <= nums[i] <= 2^31 - 1
---

## 3. Edge Cases

- Already sorted ascending array → 0 reverse pairs
- Fully descending array → potentially maximum reverse pairs
- Array with duplicates → count all valid (i, j) pairs
- Single element → 0 reverse pairs
---

## 4. Examples

```text
Example 1:
Input: [6, 4, 1, 2, 7]
Output: 3
Explanation (reverse pairs):
(0, 2) → 6 > 2*1
(0, 3) → 6 > 2*2
(1, 2) → 4 > 2*1

Example 2:
Input: [5, 4, 4, 3, 3]
Output: 0
Explanation: no valid pairs

Example 3:
Input: [6, 4, 4, 2, 2]
Output: 2
Explanation: pairs (0, 3) and (0, 4)
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check every pair (i, j) with i < j and count if nums[i] > 2 * nums[j].

**Steps:**
- Initialize count = 0
- Iterate i from 0 to n-2
- Iterate j from i+1 to n-1
- If nums[i] > 2 * nums[j] → increment count

**Java Code:**
```java
class ReversePairsBrute {
    public int reversePairs(int[] nums){
        int n = nums.length;
        int count = 0;
        for(int i=0;i<n-1;i++){
            for(int j=i+1;j<n;j++){
                if((long)nums[i] > 2L * nums[j]) count++;
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
- Similar to inversion count using merge sort
- During merge, for each element in left half, count elements in right half satisfying nums[i] > 2 * nums[j]

**Steps:**
- Implement modified merge sort
- During merge step, before merging, count valid reverse pairs using two pointers
- Merge arrays normally after counting
- Return total count

**Java Code:**
```java
class ReversePairsMergeSort {
    public int reversePairs(int[] nums){
        return mergeSort(nums, 0, nums.length - 1);
    }
    
    private int mergeSort(int[] nums, int left, int right){
        if(left >= right) return 0;
        int mid = left + (right-left)/2;
        int count = mergeSort(nums, left, mid) + mergeSort(nums, mid+1, right);
        
        int j = mid+1;
        for(int i=left;i<=mid;i++){
            while(j<=right && (long)nums[i] > 2L * nums[j]) j++;
            count += (j - mid - 1);
        }
        
        merge(nums, left, mid, right);
        return count;
    }
    
    private void merge(int[] nums, int left, int mid, int right){
        int n1 = mid - left + 1;
        int n2 = right - mid;
        int[] L = new int[n1];
        int[] R = new int[n2];
        for(int i=0;i<n1;i++) L[i] = nums[left+i];
        for(int j=0;j<n2;j++) R[j] = nums[mid+1+j];
        int i=0,j=0,k=left;
        while(i<n1 && j<n2){
            if(L[i] <= R[j]) nums[k++] = L[i++];
            else nums[k++] = R[j++];
        }
        while(i<n1) nums[k++] = L[i++];
        while(j<n2) nums[k++] = R[j++];
    }
}
```

**Complexity (Time & Space):**
- Time: O(n log n)
- Space: O(n)

---

## 6. Justification / Proof of Optimality

- Brute Force → easy but slow for large arrays
- Merge Sort → efficient, counts reverse pairs while sorting
- Merge Sort is necessary for n <= 5 * 10^4 constraints
---

## 7. Variants / Follow-Ups

- Count reverse pairs modulo some number
- Count reverse pairs in a streaming array
- Generalized condition: nums[i] > k * nums[j]
---

## 8. Tips & Observations

- Use long when multiplying by 2 to avoid overflow
- Merge sort counting is similar to counting inversions
- Each merge step counts cross-pairs efficiently
---

<!-- #endregion -->
