<!-- #region 240-Next Greater Element – II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q240: Next Greater Element – II</h1>

## 1. Problem Statement

- Given a circular integer array, find the Next Greater Element (NGE) for each element.
  * NGE of x = first element greater than x when moving clockwise
  * If no such element exists → -1
---

## 2. Problem Understanding

- Array is circular, not linear
- After last index, traversal continues from index 0
- Each element may need to look ahead beyond array end
- Brute force will time out
---

## 3. Constraints

- Large input possible
- Efficient solution required → O(N)
---

## 4. Edge Cases

- All elements same
- Strictly increasing
- Strictly decreasing
- Single element array
---

## 5. Examples

```text
arr = [5, 7, 1, 7, 6, 0]
Output = [7, -1, 7, -1, 7, 5]
```

---

## 6. Approaches

### Approach 1: Brute Force (Circular Scan)

**Idea:**
- For every element:
- Move clockwise
- Stop when:
  * Greater element found
  * Full circle completed

**Java Code:**
```java
int[] nextGreater(int[] arr) {
    int n = arr.length;
    int[] res = new int[n];

    for (int i = 0; i < n; i++) {
        res[i] = -1;
        for (int j = 1; j < n; j++) {
            int idx = (i + j) % n;
            if (arr[idx] > arr[i]) {
                res[i] = arr[idx];
                break;
            }
        }
    }
    return res;
}
```

**💭 Intuition Behind the Approach:**
- Direct definition simulation
- For each element, check next n-1 elements circularly

**Complexity (Time & Space):**
- Time: O(N^2) — nested loops
- Space: O(1) — excluding output

### Approach 2: Optimal (Monotonic Stack + Double Traversal)

**Idea:**
- Core Idea (MEMORIZE)
  * Traverse array from 0 → 2n-1
  * use i % n to access elements
- This simulates circular traversal without nested loops.

**Steps:**
- Initialize result array with -1
- Use stack to store indices
- Traverse from 0 to 2n-1
- Let idx = i % n
- While stack top element < arr[idx]:
  * Pop
  * Set its NGE
- Push index only in first pass (i < n)

**Java Code:**
```java
int[] nextGreaterElements(int[] arr) {
    int n = arr.length;
    int[] res = new int[n];
    Arrays.fill(res, -1);

    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < 2 * n; i++) {
        int idx = i % n;

        while (!st.isEmpty() && arr[st.peek()] < arr[idx]) {
            res[st.pop()] = arr[idx];
        }

        if (i < n) {
            st.push(idx);
        }
    }
    return res;
}
```

**💭 Intuition Behind the Approach:**
- Stack keeps decreasing elements
- Second traversal only helps resolve pending elements
- No re-pushing in second pass → prevents infinite loop
- Each index pushed & popped once

**Complexity (Time & Space):**
- Time: O(N) — each element pushed & popped once
- Space: O(N) — stack

---

## 7. Justification / Proof of Optimality

- Your idea works logically but degrades to O(N²)
- Stack + circular traversal solves it cleanly
- This is the standard expected solution
---

## 8. Variants / Follow-Ups

- Next Smaller Element (Circular)
- Previous Greater Element (Circular)
- Circular Stock Span
- Maximum Circular Subarray (Kadane variant)
---

## 9. Tips & Observations

- Circular array → think “2× traversal”
- Never do “handle remaining stack separately”
- % n is the key trick
- Push indices, not values
---

## 10. Pitfalls

- Pushing indices again in second loop
- Forgetting % n
- Using < instead of <= (depends on strictness)
- Trying to brute-fix leftover stack
---

<!-- #endregion -->
