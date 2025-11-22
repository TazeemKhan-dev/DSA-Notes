<!-- #region 1-Selection Sort -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q1: Selection Sort</h1>

## 1. Problem Understanding

- Goal: Sort an array by repeatedly selecting the smallest (or largest) element from the unsorted part and putting it at the beginning.
- At the end of each pass, one element is placed in its correct position.
- Think of it as “selecting” the correct element for each position.

- **Working Principle**
    - Divide the array into two parts: sorted (left) and unsorted (right).
    - Initially, the sorted part is empty, and the unsorted part is the entire array.
    - For each index i:
      * Find the index of the smallest element in the unsorted part.
      * Swap it with the element at index i.
    - Continue until the array is fully sorted.

- **Algorithm Steps**
    - Loop i from 0 to n - 2:
    - Set minIndex = i
    - Loop j from i + 1 to n - 1:
    - If arr[j] < arr[minIndex], update minIndex = j
    - After inner loop, swap arr[i] and arr[minIndex]
    - Repeat until all elements are placed correctly.
---

## 2. Examples

```text
Input: [64, 25, 12, 22, 11]

Pass 1:
Find smallest in [64, 25, 12, 22, 11] → 11
Swap 11 and 64
Array → [11, 25, 12, 22, 64]

Pass 2:
Smallest in [25, 12, 22, 64] → 12
Swap 12 and 25
Array → [11, 12, 25, 22, 64]

Pass 3:
Smallest in [25, 22, 64] → 22
Swap 22 and 25
Array → [11, 12, 22, 25, 64]

Pass 4:
Smallest in [25, 64] → 25
No change
Final: [11, 12, 22, 25, 64]
```

---

## 3. Approaches

### Approach 1: 

**Java Code:**
```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;

            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }

            // Swap smallest with the first element of unsorted part
            int temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
    }
}
```

**Complexity (Time & Space):**
- Time Complexity:
- Best Case → O(n²)
  * Average Case → O(n²)
  * Worst Case → O(n²)
  * Space Complexity: O(1) (in-place sorting)
- Stable: ❌ No (swapping can break the order of equal elements)
- Adaptive: ❌ No (always performs full passes even if already sorted)

---

## 4. Tips & Observations

- Ideal for small arrays or when swapping cost > comparison cost.
- Easy to reason about — interviewers often use it to check your understanding of comparison-based sorting.
- Understand why it’s not stable — swapping non-adjacent elements breaks order.
- Even if array is sorted, it will still do all passes (non-adaptive).
---

<!-- #endregion -->
