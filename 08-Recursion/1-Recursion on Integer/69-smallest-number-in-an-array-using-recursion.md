<!-- #region 69-Smallest Number in an Array using Recursion -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q69: Smallest Number in an Array using Recursion</h1>

## 1. Problem Understanding

- You are given an array arr of size n.
- You need to find the minimum element in the array using recursion (no loops).
- The recursion should process one element at a time and eventually return the smallest value.
---

## 2. Constraints

- 1 <= n <= 10^3
- -10^4 <= arr[i] <= 10^4
- Time limit allows simple O(n) recursive solution.
---

## 3. Edge Cases

- Array has only one element → return that element.
- All elements are same → return that element.
- Array contains negative numbers → recursion should handle comparisons properly.
---

## 4. Examples

```text
Input
5
5 4 0 -8 67
Output
-8
Explanation
→ Recursive calls compare elements one by one until the smallest (-8) is found.
```

---

## 5. Approaches

### Approach 1: Linear Recursive Comparison (Left to Right)

**Idea:**
- Compare the first element with the minimum of the rest of the array.
- Base case: when array size = 1 → return that element.

**Steps:**
- Define a recursive function minElement(arr, n).
- Base case → if n == 1, return arr[0].
- Recursive call → minElement(arr, n-1) to get the min of first (n-1) elements.
- Compare last element with recursive result.
- Return the smaller one.

**Java Code:**
```java
int minElement(int[] arr, int n) {
    if (n == 1) return arr[0];
    int small = minElement(arr, n - 1);
    return Math.min(arr[n - 1], small);
}
minElement([5, 2, 4, 1], 4)
        |
        🔁 Recursive call → minElement([5, 2, 4], 3)
                |
                🔁 Recursive call → minElement([5, 2], 2)
                        |
                        🔁 Recursive call → minElement([5], 1)
                                🟢 Base case → return 5
                        🔙 Return → min(2, 5) = 2
                🔙 Return → min(4, 2) = 2
        🔙 Return → min(1, 2) = 1
✅ Final Answer → 1

```

**Complexity (Time & Space):**
- Time Complexity
  * O(n) → one call per element.
- Space Complexity
  * O(n) → recursion stack.

### Approach 2: Index-Based Recursion

**Idea:**
- Pass the index i as a changing variable.
- Move forward from 0 to n-1, comparing elements along the way.

**Steps:**
- Base case → if i == n - 1, return arr[i].
- Recursive call to get min of rest: minElement(arr, i + 1, n).
- Compare current element arr[i] with result of recursion.

**Java Code:**
```java
int minElement(int[] arr, int i, int n) {
    if (i == n - 1) return arr[i];
    int minRest = minElement(arr, i + 1, n);
    return Math.min(arr[i], minRest);
}
```

**Complexity (Time & Space):**
- Time Complexity
  * O(n)
- Space Complexity
  * O(n) (recursion depth)

---

## 6. Justification / Proof of Optimality

- Each recursive call reduces the problem size by 1.
- Base case ensures termination at the smallest subproblem (single element).
- Works correctly for both positive and negative numbers.
---

## 7. Variants / Follow-Ups

- Find Maximum element (replace Math.min with Math.max)
- Find both Min and Max recursively (return pair or use helper class)
- Find Minimum index recursively (return index instead of value)
---

## 8. Tips & Observations

- Recursion helps visualize problems as smaller subarrays.
- Always identify a shrinking condition (n-1 or i+1).
- Use Math.min() and Math.max() to avoid manual if-else checks.
- Dry run on small arrays to ensure indices are correct.
---

<!-- #endregion -->
