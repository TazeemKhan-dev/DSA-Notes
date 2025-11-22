<!-- #region 54-Merge Two Sorted Arrays Without Extra Space -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q54: Merge Two Sorted Arrays Without Extra Space</h1>

## 1. Problem Understanding

- Given two sorted arrays nums1 and nums2.
- Merge them in-place into a single sorted array.
- nums1 has enough space to hold all elements (m + n), first m are valid elements, rest are 0s.
- nums2 has n elements.
- Goal: nums1 should contain the merged sorted array without using extra space.
---

## 2. Constraints

- n == nums2.length
- m + n == nums1.length
- 0 <= n, m <= 1000
- -10^4 <= nums1[i], nums2[i] <= 10^4
- Both arrays are sorted in non-decreasing order
---

## 3. Edge Cases

- Either array is empty → return the non-empty array
- Arrays contain duplicates → keep all elements
- Negative numbers → ensure correct ordering
- All elements of nums2 are smaller than nums1 → insert at the beginning
- All elements of nums2 are larger than nums1 → insert at the end
---

## 4. Examples

```java
Example 1:
Input: nums1 = [-5, -2, 4, 5], nums2 = [-3, 1, 8]
Output: [-5, -3, -2, 1, 4, 5, 8]

Example 2:
Input: nums1 = [0, 2, 7, 8], nums2 = [-7, -3, -1]
Output: [-7, -3, -1, 0, 2, 7, 8]

Example 3:
Input: nums1 = [1, 3, 5], nums2 = [2, 4, 6, 7]
Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 5. Approaches

### Approach 1: Brute Force (Merge and Sort)

**Idea:**
- Copy elements from nums2 into nums1’s extra space
- Sort the entire nums1

**Steps:**
- Copy nums2 elements into the last n positions of nums1
- Sort nums1 using Arrays.sort()

**Java Code:**
```java
import java.util.Arrays;

class MergeSortedBrute {
    public void merge(int[] nums1, int m, int[] nums2, int n){
        for(int i=0;i<n;i++) nums1[m+i] = nums2[i];
        Arrays.sort(nums1);
    }
}
```

**Complexity (Time & Space):**
- Time: O((m+n) log(m+n))                             
- Space: O(1) (in-place sorting)

### Approach 2: Two Pointers from End

**Idea:**
- Use three pointers from the end to avoid overwriting elements
- Compare largest elements from nums1 and nums2 and fill from the end

**Steps:**
- Initialize i = m-1 (last valid in nums1), j = n-1 (last in nums2), k = m+n-1 (last in nums1)
- While i >= 0 && j >= 0:
    * If nums1[i] > nums2[j] → nums1[k--] = nums1[i--]
   * Else → nums1[k--] = nums2[j--]
- Copy remaining elements of nums2 if any

**Java Code:**
```java
class MergeSortedTwoPointers {
    public void merge(int[] nums1, int m, int[] nums2, int n){
        int i = m-1, j = n-1, k = m+n-1;
        while(i>=0 && j>=0){
            if(nums1[i] > nums2[j]) nums1[k--] = nums1[i--];
            else nums1[k--] = nums2[j--];
        }
        while(j>=0) nums1[k--] = nums2[j--];
    }
}
```

**Complexity (Time & Space):**
- Time: O(m + n)                    
- Space: O(1)

### Approach 3: : Gap Method (Shell Sort Inspired, for strict no extra space)

**Idea:**
- Treat nums1 and nums2 as a single array
- Use gap method to compare and swap elements at distance gap
- Reduce gap until it becomes 1

**Steps:**
- Initialize gap = ceil((m+n)/2)
- Compare elements at distance gap in combined arrays
- Swap if out of order
- Reduce gap: gap = ceil(gap/2)
- Repeat until gap = 0

**Java Code:**
```java
class MergeSortedGapMethod {
    public void merge(int[] nums1, int m, int[] nums2, int n){
        int total = m+n;
        int gap = (total+1)/2;
        while(gap>0){
            int i=0, j=i+gap;
            while(j<total){
                int val1 = (i<m)? nums1[i] : nums2[i-m];
                int val2 = (j<m)? nums1[j] : nums2[j-m];
                if(val1 > val2){
                    if(i<m && j<m){
                        int temp = nums1[i]; nums1[i]=nums1[j]; nums1[j]=temp;
                    } else if(i<m && j>=m){
                        int temp = nums1[i]; nums1[i]=nums2[j-m]; nums2[j-m]=temp;
                    } else {
                        int temp = nums2[i-m]; nums2[i-m]=nums2[j-m]; nums2[j-m]=temp;
                    }
                }
                i++; j++;
            }
            if(gap==1) gap=0;
            else gap = (gap+1)/2;
        }
        for(int i=0;i<n;i++) nums1[m+i] = nums2[i];
    }
}
```

**Complexity (Time & Space):**
- Time: O((m+n) log(m+n))                   
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Brute Force → simple but slower due to sorting
- Two Pointers → optimal and widely used in interviews
- Gap Method → useful for strict in-place merge without extra space
---

## 7. Variants / Follow-Ups

- Merge k sorted arrays in-place
- Merge sorted linked lists
- Merge arrays with different data types or custom comparator
---

## 8. Tips & Observations

- Always merge from the end to avoid overwriting elements in nums1
- Gap method is tricky but reduces extra space constraints
- Two pointer method is most practical for interviews
---

<!-- #endregion -->
