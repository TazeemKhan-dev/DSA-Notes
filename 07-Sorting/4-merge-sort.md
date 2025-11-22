<!-- #region 4-Merge Sort -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q4: Merge Sort</h1>

## 1. Problem Understanding

- Merge Sort is a divide-and-conquer sorting algorithm.
- Steps:
  * Divide the array into two halves
  * Recursively sort each half
  * Merge the two sorted halves to get the final sorted array
- Works on any array, including duplicates and negative numbers.
---

## 2. Constraints

- Array can contain duplicates and negative numbers
- Size: 1 ≤ n ≤ 10^6
- Works on arrays, lists, and linked lists
---

## 3. Edge Cases

- Empty array → already sorted
- Single element → already sorted
- All elements same → still needs merging
---

## 4. Examples

```text
Input:

arr = [5, 2, 4, 1, 3]


Output:

[1, 2, 3, 4, 5]
```

---

## 5. Approaches

### Approach 1: Quick Sort using Lomuto Partition

**Idea:**
- Split the array until each subarray has one element
- Merge the sorted subarrays while maintaining order

**Java Code:**
```java
static void mergeSort(int[] arr, int left, int right) {
    if (left >= right) return;

    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

static void merge(int[] arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    int[] L = new int[n1];
    int[] R = new int[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int i = 0; i < n2; i++) R[i] = arr[mid + 1 + i];

    int i = 0, j = 0, k = left;

    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
    }

    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}
Recursion Tree Example (arr = [5,2,4,1]):

mergeSort([5,2,4,1])
          /           \
 mergeSort([5,2])   mergeSort([4,1])
     /     \           /     \
 [5]       [2]      [4]      [1]
    \       /        \       /
   merge([5,2])      merge([4,1])
        \             /
       [2,5]       [1,4]
            \       /
           merge([2,5],[1,4])
                [1,2,4,5]
```

**Complexity (Time & Space):**
- Time: O(n log n) for all cases (best, worst, average)
- Space: O(n) extra space for merging

---

## 6. Justification / Proof of Optimality

- Always divides array in half → guaranteed log n depth
- Merging is linear → efficient
- Stable sort → preserves relative order of equal elements
---

## 7. Variants / Follow-Ups

- Iterative Merge Sort (bottom-up)
- Merge Sort on linked lists → can be done in-place with O(1) extra space
- External Merge Sort → for very large datasets on disk
---

## 8. Tips & Observations

- Very suitable for large datasets
- Good choice when stability is important
- Recursive implementation uses O(log n) stack space
---

<!-- #endregion -->
