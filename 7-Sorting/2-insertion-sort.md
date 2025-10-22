<!-- #region 2-Insertion Sort -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q2: Insertion Sort</h1>

## 1. Problem Understanding

- Goal: Sort the array by building the sorted portion one element at a time — like how we sort cards in our hand.
- It takes one element from the unsorted part and inserts it into its correct position in the sorted part.
- It’s efficient for small or partially sorted arrays.

- **Working Principle**
    - Assume the first element is already sorted.
    - Pick the next element and compare it with elements in the sorted part (to its left).
    - Shift all elements greater than it to the right.
    - Insert the picked element at its correct position.
    - Repeat until the entire array is sorted.

- **Algorithm Steps**
    - Start from index 1 to n - 1 (let the first element be sorted).
    - Store the current element (key = arr[i]).
    - Compare key with elements before it (j = i - 1).
    - While arr[j] > key, shift arr[j] to arr[j + 1].
    - Place the key in the correct position.
---

## 2. Examples

```text
Input: [5, 3, 4, 1, 2]

Step-by-step:

Pass 1 (i=1): key = 3
Compare with 5 → shift 5 → [5, 5, 4, 1, 2]
Insert 3 → [3, 5, 4, 1, 2]

Pass 2 (i=2): key = 4
Compare with 5 → shift 5 → [3, 5, 5, 1, 2]
Compare with 3 → stop → insert 4 → [3, 4, 5, 1, 2]

Pass 3 (i=3): key = 1
Shift 5 → [3, 4, 5, 5, 2]
Shift 4 → [3, 4, 4, 5, 2]
Shift 3 → [3, 3, 4, 5, 2]
Insert 1 → [1, 3, 4, 5, 2]

Pass 4 (i=4): key = 2
Shift 5, 4, 3 → [1, 3, 4, 5, 5], [1, 3, 4, 4, 5], [1, 3, 3, 4, 5]
Insert 2 → [1, 2, 3, 4, 5]

✅ Sorted Array: [1, 2, 3, 4, 5]
```

---

## 3. Approaches

### Approach 1: 

**Java Code:**
```java
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;

        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;

            // Move elements greater than key one step ahead
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }

            arr[j + 1] = key;
        }
    }
}
```

**Complexity (Time & Space):**
- Time Complexity:
  * Best Case → O(n) (already sorted array)
  * Average Case → O(n²)
  * Worst Case → O(n²) (reverse sorted array)
- Space Complexity: O(1) (in-place)
- Stable: ✅ Yes (does not swap non-adjacent equal elements)
- Adaptive: ✅ Yes (optimized for nearly sorted arrays)

---

## 4. Tips & Observations

- Excellent for online sorting (can sort data as it arrives).
- Best-case O(n) → very efficient if array is almost sorted.
- Stable: equal elements retain their original order.
- Frequently used in interview warm-ups before moving to Merge/Quick sort.
- Remember: shifting, not swapping — key difference from Bubble/Selection Sort.
---

<!-- #endregion -->
