<!-- #region 3-Quick Sort -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q3: Quick Sort</h1>

## 1. Problem Understanding

- Quick Sort is a divide-and-conquer sorting algorithm.
- Pick a pivot element and partition the array such that:
  * Elements smaller than pivot are on the left
  * Elements larger than pivot are on the right
- Recursively sort left and right subarrays.
- Goal: sort an array in ascending order.
---

## 2. Constraints

- Array can contain negative and positive integers
- 1 ≤ n ≤ 10^6 (practical limits)
- In-place sorting preferred
---

## 3. Edge Cases

- Empty array → already sorted
- Single element → already sorted
- All elements equal → pivot selection matters for performance
- Already sorted array → worst-case for naive pivot selection
---

## 4. Examples

```text
Input:

arr = [3, 6, 2, 8, 5]

Output:

[2, 3, 5, 6, 8]
```

---

## 5. Approaches

### Approach 1: Quick Sort using Lomuto Partition

**Idea:**
- Select last element as pivot.
- Place elements smaller than pivot on left, larger on right.
- Recursively sort left and right subarrays.

**Steps:**
- If low >= high → return (base case)
- Partition array with pivot
- Recursively quicksort left and right

**Java Code:**
```java
static void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int p = partition(arr, low, high);
        quickSort(arr, low, p - 1);
        quickSort(arr, p + 1, high);
    }
}

static int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    return i + 1;
}
Recursion Tree Example (arr = [3, 6, 2]):

[3,6,2]
pivot=2
Left: []      Right: [3,6]
              pivot=6
              Left: [3] Right: []


Sorted result: [2, 3, 6]
```

**Complexity (Time & Space):**
- Time:
- Average case: O(n log n)
- Worst case (sorted array, bad pivot): O(n^2)
- Space: O(log n) recursion stack

---

## 6. Justification / Proof of Optimality

- Divides problem recursively → divide-and-conquer
- Works in-place → no extra array needed
- Pivot selection impacts performance → can use random pivot to improve
---

## 7. Variants / Follow-Ups

- Hoare partition scheme → slightly more efficient in some cases
- Randomized quicksort → choose pivot randomly to avoid worst-case
- 3-way quicksort → handles repeated elements efficiently
---

## 8. Tips & Observations

- Quick Sort is not stable
- Always check pivot selection for already sorted / reverse sorted arrays
- Small subarrays can be switched to Insertion Sort for efficiency
---

<!-- #endregion -->
