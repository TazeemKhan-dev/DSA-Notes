<!-- #region 155-Median of 2 sorted arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q155: Median of 2 sorted arrays</h1>

## 1. Problem Understanding

- You are given two sorted arrays arr1 and arr2.
- You must return the median of the combined sorted array without merging (optimal approach).
- Median rules:
  * If total length is odd → return the middle element.
  * If even → return average of the two middle elements.
---

## 2. Constraints

- 0 ≤ m, n ≤ 1000
- 1 ≤ m + n ≤ 2000
- Values can be negative.
---

## 3. Edge Cases

- One array empty.
- Arrays of very uneven sizes.
- Duplicate elements.
- Both arrays length 1.
- All elements in arr1 ≤ all in arr2 (or vice-versa).
---

## 4. Examples

```text
arr1 = [2, 4, 6], arr2 = [1, 3, 5] → median = 3.5
arr1 = [2, 4, 6], arr2 = [1, 3] → median = 3
arr1 = [2, 4, 5], arr2 = [1, 6] → median = 4
```

---

## 5. Approaches

### Approach 1: Brute Force (Merge Like MergeSort)

**Idea:**
- Merge both arrays into one sorted list.
- Find the median normally.

**Steps:**
- Use two pointers.
- Build a merged array.
- Pick median based on total length.

**Java Code:**
```java
public static double medianBrute(int[] a, int[] b) {
    int m = a.length, n = b.length;
    int[] merged = new int[m+n];
    
    int i=0, j=0, k=0;
    while(i<m && j<n){
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
- Classic merge — straightforward but not optimal.

**Complexity (Time & Space):**
- Time:
  * O(m+n) because we merge all elements.
- Space:
  * O(m+n) because we store merged array.

### Approach 2: Two-Pointer Without Full Merge (Better)

**Idea:**
- Simulate merge until middle index.
- Don’t store full merged array.
- Track only middle elements

**Steps:**
- Use two pointers.
- Iterate only till (m+n)/2.

**Java Code:**
```java
public static double medianBetter(int[] a, int[] b) {
    int m = a.length, n = b.length;
    int total = m+n;
    
    int i=0, j=0;
    int prev = -1, curr = -1;

    for(int k=0; k<=total/2; k++){
        prev = curr;

        if(i<m && (j>=n || a[i] <= b[j])){
            curr = a[i++];
        } else {
            curr = b[j++];
        }
    }

    if(total % 2 == 1) return curr;
    return (prev + curr) / 2.0;
}
```

**💭 Intuition Behind the Approach:**
- We don’t care about the entire merge—only until we hit the median position.

**Complexity (Time & Space):**
- Time:
  * O((m+n)/2) ≈ O(m+n)
- Space:
  * O(1) — no extra array.

### Approach 3: Binary Search on Smaller Array (Partition Method)

**Idea:**
- Use binary search to partition both arrays so that:
  * left half has exactly (m+n+1)/2 elements
  * All left side ≤ all right side
- We binary search on arr1 partition index.

**Steps:**
- Ensure arr1 is smaller array.
- Binary search partition cut1 in arr1.
- Compute cut2 in arr2 = required left half − cut1.
- Check if:
  * maxLeft1 ≤ minRight2
  * maxLeft2 ≤ minRight1
- If not balanced → shift binary search.

**Java Code:**
```java
public static double findMedianSortedArrays(int[] a, int[] b) {
    if(a.length > b.length) return findMedianSortedArrays(b, a);

    int m = a.length, n = b.length;
    int low = 0, high = m;

    while(low <= high){
        int cut1 = low + (high - low)/2;
        int cut2 = (m+n+1)/2 - cut1;

        int left1 = (cut1 == 0 ? Integer.MIN_VALUE : a[cut1-1]);
        int left2 = (cut2 == 0 ? Integer.MIN_VALUE : b[cut2-1]);
        int right1 = (cut1 == m ? Integer.MAX_VALUE : a[cut1]);
        int right2 = (cut2 == n ? Integer.MAX_VALUE : b[cut2]);

        if(left1 <= right2 && left2 <= right1){
            if((m+n) % 2 == 0){
                return (Math.max(left1,left2) + Math.min(right1,right2)) / 2.0;
            } else {
                return Math.max(left1,left2);
            }
        }
        else if(left1 > right2){
            high = cut1 - 1;
        } else {
            low = cut1 + 1;
        }
    }
    
    return 0.0;
}
```

**💭 Intuition Behind the Approach:**
- Instead of merging, we search for the correct partition such that:
  * All left side elements ≤ all right side elements.
  * Once partition is valid, median comes from edges.
- This uses the fact both arrays are sorted → binary search works.

**Complexity (Time & Space):**
- Time:
  * O(log(min(m,n))) because we binary search only the smaller array.
  * It’s optimal because we avoid touching all elements.
- Space:
  * O(1) — no extra space.

### Approach 4: K-th Element via Binary Search Recursion

**Idea:**
- We find the k-th element using recursive shrinking.
- Median =
  * odd → kth element
  * even → avg of k and k+1.

**Java Code:**
```java
public static double medianKth(int[] a, int[] b){
    int m = a.length, n = b.length;
    int total = m+n;

    if(total % 2 == 1){
        return kth(a,0,b,0,total/2 + 1);
    } else {
        double x = kth(a,0,b,0,total/2);
        double y = kth(a,0,b,0,total/2 + 1);
        return (x + y) / 2.0;
    }
}

private static int kth(int[] a, int i, int[] b, int j, int k){
    if(i >= a.length) return b[j + k - 1];
    if(j >= b.length) return a[i + k - 1];
    if(k == 1) return Math.min(a[i], b[j]);

    int midA = (i + k/2 - 1 < a.length) ? a[i + k/2 - 1] : Integer.MAX_VALUE;
    int midB = (j + k/2 - 1 < b.length) ? b[j + k/2 - 1] : Integer.MAX_VALUE;

    if(midA < midB){
        return kth(a, i + k/2, b, j, k - k/2);
    } else {
        return kth(a, i, b, j + k/2, k - k/2);
    }
}



Recursion Tree

Example: total = 10 → find k=5

k=5
 |
 |-- compare a[k/2], b[k/2]
 |
 k becomes 3
 |
 |-- compare next halves
 |
 k becomes 1
 |
 return min(...)
```

**💭 Intuition Behind the Approach:**
- At each step, eliminate k/2 elements from one of the arrays—like searching for the k-th number.

**Complexity (Time & Space):**
- Time:
  * O(log(m+n)) because every recursive step halves k.
- Space:
  * O(log(m+n)) due to recursion stack.

---

## 6. Justification / Proof of Optimality

- The arrays are sorted → median position depends only on ordering.
- Instead of merging all, find a partition where left half contains exactly half of total elements.
- Binary search is possible because:
  * when left1 > right2 → cut1 is too big
  * when left2 > right1 → cut1 is too small
- This monotonic behaviour enables O(log(min(m,n))).
---

## 7. Variants / Follow-Ups

- Find median of k sorted arrays.
- Find k-th smallest of two arrays.
- Find median of infinite sorted streams.
- Weighted median.
- Merging intervals using similar partition logic.
---

## 8. Tips & Observations

- Always binary search on the smaller array.
- Take care of boundaries using ±∞.
- For odd length, median is just max(left side).
- Partition logic is the heart of the problem.

- **⚠️ Pitfalls**
    - Forgetting to pick smaller array for binary search.
    - Boundary handling when partition is at index 0 or m.
    - Floating-point return for even length.
    - Off-by-one errors in partition formula.
---

<!-- #endregion -->
