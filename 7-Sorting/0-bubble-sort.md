<!-- #region 0-Bubble Sort -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Bubble Sort</h1>

## 1. Problem Understanding

- Goal: Sort an array by repeatedly swapping adjacent elements if they are in the wrong order.
- The largest element “bubbles up” to the end after each full pass.
- After each iteration, one more element at the end becomes sorted.

- **Working Principle**
    - Compare adjacent pairs of elements.
    - If arr[j] > arr[j + 1], swap them.
    - Continue this process for the entire array.
    - After the 1st pass → the largest element reaches the last position.
    - After the 2nd pass → the second-largest is at the second-last position, and so on.
    - Continue until no swaps are needed (array is sorted).

- **Algorithm Steps**
    - Loop i from 0 to n-1
    - Initialize a flag swapped = false
    - Inner loop j from 0 to n - i - 2
      * If arr[j] > arr[j + 1], swap them
      * Set swapped = true
    - If no swaps occurred in the inner loop → break early (array is already sorted)
    - Print or return the sorted array.
---

## 2. Examples

```text
Input: [5, 1, 4, 2, 8]
  Pass 1: [1, 4, 2, 5, 8] → 8 bubbled to end
  Pass 2: [1, 2, 4, 5, 8] → 5 in correct position
  Pass 3: [1, 2, 4, 5, 8] → No swap → stop early
Output: [1, 2, 4, 5, 8]
```

---

## 3. Approaches

### Approach 1: 

**Java Code:**
```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        boolean swapped;

        for (int i = 0; i < n - 1; i++) {
            swapped = false;

            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap elements
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }

            // If no swaps, array is sorted
            if (!swapped) break;
        }
    }
}
```

**Complexity (Time & Space):**
- Time Complexity:
- Worst Case → O(n²) (reverse sorted array)
- Best Case → O(n) (already sorted, with swap check)
- Average Case → O(n²)
- Space Complexity: O(1) (in-place sorting)
- Stable: ✅ Yes (equal elements maintain relative order)
- Adaptive: ✅ Yes (can stop early if already sorted)

---

## 4. Tips & Observations

- Always add a swapped flag for early termination.
- Bubble Sort is mainly for teaching concepts (not used in production).
- Interviewers may ask to optimize or detect early stop condition.
- Understand it well — it helps you grasp in-place sorting and pairwise comparison logic used in many algorithms.
---

<!-- #endregion -->
