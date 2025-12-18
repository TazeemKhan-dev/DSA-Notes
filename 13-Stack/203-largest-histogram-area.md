<!-- #region 203-Largest Histogram Area -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q203: Largest Histogram Area</h1>

## 1. Problem Statement

- You are given a histogram consisting of N bars, where the height of each bar is given in array A.
- Each bar has a width of 1.
- Your task is to find the largest rectangular area that can be formed using contiguous bars in the histogram.
---

## 2. Problem Understanding

- Each bar can extend left and right until a smaller bar blocks it
- For every bar i, we want:
  * the first smaller bar on the left
  * the first smaller bar on the right
- The bar A[i] becomes the minimum height of the rectangle
- Width = (rightSmallerIndex - leftSmallerIndex - 1)
- Area = height * width
- We need the maximum area among all bars
---

## 3. Constraints

- 1 <= N <= 10000
- 1 <= A[i] <= 10000
- Expected Time Complexity: O(N)
- Expected Space Complexity: O(N)
---

## 4. Edge Cases

- Single bar → area = height
- All bars same height
- Strictly increasing heights
- Strictly decreasing heights
- Minimum height bars spanning full width
---

## 5. Examples

```text
Example 1

Input:

4
1 2 3 1


Output:

4

Example 2

Input:

3
2 2 2


Output:

6
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- For each bar, expand left and right until a smaller bar is found.

**Complexity (Time & Space):**
- Time: O(N^2) — expansion for each bar
- Space: O(1) — no extra space
- ❌ Not acceptable for given constraints

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Use a monotonic increasing stack
- Stack stores indices
- When a smaller bar appears, it finalizes rectangles for taller bars
- This allows computing area in one traversal

**Steps:**
- Initialize an empty stack
- Traverse bars from left to right
- While stack not empty AND
  * A[current] < A[stack.top()]:
      * Pop index h
      * Height = A[h]
      * Right boundary = current index
      * Left boundary = stack.top() (after pop) or -1
      * Width = right - left - 1
      * Update max area
- Push current index
- After traversal, pop remaining bars using N as right boundary

**Java Code:**
```java
class Solution {
    public static int largestRectangleArea(int[] A, int n) {
        Stack<Integer> st = new Stack<>();
        int maxArea = 0;

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && A[i] < A[st.peek()]) {
                int h = A[st.pop()];
                int right = i;
                int left = st.isEmpty() ? -1 : st.peek();
                int width = right - left - 1;
                maxArea = Math.max(maxArea, h * width);
            }
            st.push(i);
        }

        while (!st.isEmpty()) {
            int h = A[st.pop()];
            int right = n;
            int left = st.isEmpty() ? -1 : st.peek();
            int width = right - left - 1;
            maxArea = Math.max(maxArea, h * width);
        }

        return maxArea;
    }
}
```

**💭 Intuition Behind the Approach:**
- Each bar is treated as the smallest bar in a rectangle
- Stack ensures bars are processed only when their right boundary is known
- Popping means:
  * “I have found the maximum width where this bar is the smallest”
- Each index is pushed and popped once

**Complexity (Time & Space):**
- Time: O(N) — each index pushed and popped once
- Space: O(N) — stack usage

---

## 7. Justification / Proof of Optimality

- The monotonic stack efficiently computes the largest rectangle by determining the maximum width for each bar where it acts as the minimum height, achieving optimal linear time complexity.
---

## 8. Variants / Follow-Ups

- Maximum rectangle in binary matrix
- Largest rectangle in skyline
- Trapping Rain Water
- Next Smaller Element
- Maximum area under histogram (circular)
---

## 9. Tips & Observations

- Always store indices, not heights
- Use < not <= to avoid incorrect width
- Right boundary is known only when a smaller bar appears
- Remaining stack elements extend till end of array
---

## 10. Pitfalls

- Forgetting to process remaining stack
- Incorrect width calculation
- Using values instead of indices
- Confusing NGE logic with histogram logic
---

<!-- #endregion -->
