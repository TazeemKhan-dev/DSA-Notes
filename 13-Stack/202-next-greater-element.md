<!-- #region 202-Next Greater Element -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q202: Next Greater Element</h1>

## 1. Problem Statement

- Given an array arr of size n consisting of non-zero positive integers, find the Next Greater Element (NGE) for each element.
- The Next Greater Element of an element is the nearest element to its right that is strictly greater than the current element.
- If no such element exists, return -1 for that position.
---

## 2. Problem Understanding

- For every index i, look only to the right side
- Find the first element greater than arr[i]
- If none exists → answer is -1
- Order of output must match input order
- Brute force is too slow → need O(N) solution
---

## 3. Constraints

- 1 <= n <= 10^6
- 1 <= arr[i] <= 10^18
- Expected Time: O(N)
- Expected Space: O(N)
---

## 4. Edge Cases

- Single element array → answer is -1
- Strictly decreasing array → all -1
- Strictly increasing array → next element for all except last
- Very large values (use long)
- Duplicate values (strictly greater required)
---

## 5. Examples

```text
Example 1

Input:

4
1 3 2 4


Output:

3 4 4 -1

Example 2

Input:

5
6 8 0 1 3


Output:

8 -1 1 3 -1
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- For each element, scan all elements to its right and find the first greater one.

**Steps:**
- For each index i
- Traverse from i+1 to n-1
- Stop when a greater element is found

**Complexity (Time & Space):**
- Time: O(N^2) — nested loops
- Space: O(1) — no extra space
- Not acceptable due to constraints

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Use a monotonic decreasing stack
- Stack stores indices (not values)
- When a greater element appears, it resolves NGE for smaller elements

**Steps:**
- Initialize an empty stack
- Create result array ans of size n
- Traverse array from left to right
- While stack not empty AND
  * arr[current] > arr[stack.top()]:
    * Pop index from stack
    * Set its NGE as arr[current]
- Push current index onto stack
- After traversal, remaining indices in stack → -1

**Java Code:**
```java
import java.util.*;

class Solution {
    public static long[] nextGreaterElement(long[] arr, int n) {
        long[] ans = new long[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && arr[i] > arr[st.peek()]) {
                ans[st.pop()] = arr[i];
            }
            st.push(i);
        }

        while (!st.isEmpty()) {
            ans[st.pop()] = -1;
        }

        return ans;
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack keeps track of elements waiting for a greater element
- When a larger value arrives, it satisfies all smaller values before it
- Each element is pushed and popped only once
- This ensures linear time complexity

**Complexity (Time & Space):**
- Time: O(N) — each element pushed & popped once
- Space: O(N) — stack + result array

---
### Approach 3: Reverse Traversal (Right → Left)

**Idea:**
- Instead of waiting for a future element to resolve answers, we pre-load future elements by traversing from right to left.
- At each index:
  * The stack already contains elements to the right
  * The nearest greater element will be on top after cleanup
- This approach is the mirror image of Previous Greater Element.

**Steps:**
- Traverse the array from n-1 to 0
- Pop elements from stack while stack.top <= arr[i]
- After popping:
  * If stack is empty → ans[i] = -1
  * Else → ans[i] = stack.top
- Push arr[i] into stack

**Java Code:**
```java
public static long[] nextGreater(long[] arr, int n) {
    long[] ans = new long[n];
    Stack<Long> st = new Stack<>();

    for (int i = n - 1; i >= 0; i--) {

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
- When scanning from right to left:
  * The stack always represents future elements
  * Smaller future elements are useless and removed
  * The top of the stack becomes the closest greater element on the right
- No element is left “pending”, so no cleanup loop is required.

**Complexity (Time & Space):**
- Time: O(n) — each element is pushed and popped at most once
- Space: O(n) — stack in the worst case (strictly decreasing array)

---


## 7. Justification / Proof of Optimality

- The monotonic stack approach efficiently finds the next greater element in a single traversal, meeting the required O(N) time complexity even for large inputs.
---

## 8. Variants / Follow-Ups

- Next Smaller Element
- Previous Greater Element
- Previous Smaller Element
- Circular Next Greater Element
- Stock Span Problem
- Daily Temperatures
---

## 9. Tips & Observations

- Use stack of indices, not values
- Stack remains monotonically decreasing
- For circular NGE → traverse array twice
- Always clarify strictly greater vs greater or equal
---

## 10. Pitfalls

- Using brute force for large n
- Storing values instead of indices
- Forgetting to assign -1 to remaining stack elements
- Confusing NGE with max to the right
---

<!-- #endregion -->
