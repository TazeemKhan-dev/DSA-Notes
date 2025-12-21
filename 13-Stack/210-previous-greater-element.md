<!-- #region 208-Previous Greater element -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q210: Previous Greater element</h1>

## 1. Problem Statement

- You are given an array arr of n distinct integers.
- For each element in the array, find the previous greater element — the nearest element to the left that is greater than the current element.
- If no such element exists, return -1 for that position.
---

## 2. Problem Understanding

- For every index i, we must look only on the left side (0 to i-1)
- Among those, we want:
  * An element greater than arr[i]
  * The closest one (nearest to i)
- If nothing qualifies → answer is -1
- Order matters → this is not sorting, this is relative position
---

## 3. Constraints

- 1 <= n <= 10^5
- 1 <= arr[i] <= 10^6
- All elements are distinct
- Large n → brute force O(n^2) will TLE
---

## 4. Edge Cases

- First element → always -1
- Strictly increasing array → all -1
- Strictly decreasing array → previous element always answer
- Single element array
---

## 5. Examples

```text
Example 1

Input:

10 20 30 40


Output:

-1 -1 -1 -1

Example 2

Input:

40 30 20 10


Output:

-1 40 30 20
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- For each element, scan leftwards until you find a greater element.

**Steps:**
- For each index i
- Loop from i-1 to 0
- First element greater than arr[i] → answer
- If none found → -1

**Java Code:**
```java
public static long[] prevGreater(long[] arr, int n) {
    long[] ans = new long[n];

    for (int i = 0; i < n; i++) {
        ans[i] = -1;
        for (int j = i - 1; j >= 0; j--) {
            if (arr[j] > arr[i]) {
                ans[i] = arr[j];
                break;
            }
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We directly simulate the definition of “previous greater” by checking all left elements until the closest valid one is found.

**Complexity (Time & Space):**
- Time: O(n^2) — nested loops, worst case full scan for each element
- Space: O(1) — no extra data structures

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Use a monotonic decreasing stack to keep only useful candidates for previous greater.
- Key Observation
  * Smaller elements to the left are useless once a bigger element appears
  * Stack helps us discard useless elements efficiently

**Steps:**
- Traverse array from left to right
- Pop elements from stack while stack.top <= arr[i]
- After popping:
  * If stack empty → answer is -1
  * Else → stack top is previous greater
- Push arr[i] into stack

**Java Code:**
```java
public static long[] prevGreater(long[] arr, int n) {
    long[] ans = new long[n];
    Stack<Long> st = new Stack<>();

    for (int i = 0; i < n; i++) {

        while (!st.isEmpty() && st.peek() <= arr[i]) {
            st.pop();
        }

        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(arr[i]);
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We only keep elements that can possibly act as a previous greater for future elements.
- Once a bigger element arrives, all smaller ones before it become irrelevant.

**Complexity (Time & Space):**
- Time: O(n) — each element pushed & popped at most once
- Space: O(n) — stack in worst case

---

## 7. Justification / Proof of Optimality

- The stack approach is optimal because:
  * It avoids repeated left scans
  * Maintains only valid candidates
  * Guarantees nearest greater by construction
---

## 8. Variants / Follow-Ups

- Previous Smaller Element
- Next Greater Element
- Next Smaller Element
- Stock Span Problem
- Largest Rectangle in Histogram
---

## 9. Tips & Observations

- Previous → traverse left to right
- Next → traverse right to left
- Greater → pop smaller
- Smaller → pop greater
- Always ask: “What elements are useless going forward?”
---

## 10. Pitfalls

- Popping the answer element instead of using it
- Using < instead of <= (important with duplicates)
- Confusing Previous vs Next logic
- Using stack index when value stack is simpler
---

<!-- #endregion -->
