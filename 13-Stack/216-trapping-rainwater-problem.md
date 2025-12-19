<!-- #region 216-Trapping Rainwater Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q216: Trapping Rainwater Problem</h1>

## 1. Problem Statement

- You are given n non-negative integers representing an elevation map, where the width of each bar is 1.
- Compute how much water can be trapped after raining.
---

## 2. Problem Understanding

- Water can be trapped between two taller bars
- At any index i, trapped water depends on:
  * the maximum height to its left
  * the maximum height to its right
- Water at index i is:
  * water[i] = min(leftMax[i], rightMax[i]) - height[i]
- (if positive)
---

## 3. Constraints

- 1 <= n <= 2 * 10^4
- 0 <= height[i] <= 10^5
---

## 4. Edge Cases

- n <= 2 → no water
- All bars same height → no water
- Strictly increasing or decreasing heights → no water
- Large height values
---

## 5. Examples

```text
Trapping Rain Water — Text Diagram (Example 1)

Input

height = [0,1,0,2,1,0,1,3,2,1,2,1]

Histogram View (bars only)
        █
        █ █
    █   █ █ █
    █ █ █ █ █ █
  █ █ █ █ █ █ █ █
--------------------------------
 0 1 0 2 1 0 1 3 2 1 2 1

Histogram + Trapped Water (~ = water)
        █
        █ █
    █~~~█ █~█
    █~█~█ █~█ █
  █ █~█~█ █~█ █ █
--------------------------------
 0 1 0 2 1 0 1 3 2 1 2 1

Index-wise Explanation
Index :  0 1 2 3 4 5 6 7 8 9 10 11
Height:  0 1 0 2 1 0 1 3 2 1  2  1
Water :  0 0 1 0 1 2 1 0 0 1  0  0

Total Water Trapped
= 1 + 1 + 2 + 1 + 1
= 6 units
```

---

## 6. Approaches

### Approach 1: Brute Force

**Idea:**
- For every index:
  * find the maximum height on the left
  * find the maximum height on the right
  * calculate trapped water

**Steps:**
- For each index i
- Scan left to find leftMax
- Scan right to find rightMax
- Add min(leftMax, rightMax) - height[i] if positive

**Java Code:**
```java
public int trapBrute(int[] height) {
    int n = height.length;
    int water = 0;

    for (int i = 0; i < n; i++) {
        int leftMax = 0, rightMax = 0;

        for (int l = 0; l <= i; l++)
            leftMax = Math.max(leftMax, height[l]);

        for (int r = i; r < n; r++)
            rightMax = Math.max(rightMax, height[r]);

        water += Math.max(0, Math.min(leftMax, rightMax) - height[i]);
    }
    return water;
}
```

**💭 Intuition Behind the Approach:**
- Water at a bar depends on the shorter boundary
- Directly simulate the definition

**Complexity (Time & Space):**
- Time: O(N^2) — nested scans
- Space: O(1) — no extra space

### Approach 2: Dynamic Programming (Prefix & Suffix Max)

**Idea:**
- Precompute:
  * maximum height to the left of every index
  * maximum height to the right of every index
- Use formula directly

**Steps:**
- Build leftMax[]
- Build rightMax[]
- For each index:
  * add trapped water

**Java Code:**
```java
public int trapDP(int[] height) {
    int n = height.length;
    int[] leftMax = new int[n];
    int[] rightMax = new int[n];

    leftMax[0] = height[0];
    for (int i = 1; i < n; i++)
        leftMax[i] = Math.max(leftMax[i - 1], height[i]);

    rightMax[n - 1] = height[n - 1];
    for (int i = n - 2; i >= 0; i--)
        rightMax[i] = Math.max(rightMax[i + 1], height[i]);

    int water = 0;
    for (int i = 0; i < n; i++)
        water += Math.max(0, Math.min(leftMax[i], rightMax[i]) - height[i]);

    return water;
}
```

**💭 Intuition Behind the Approach:**
- Avoid repeated scans by storing boundary heights
- Trade space for time

**Complexity (Time & Space):**
- Time: O(N) — three linear passes
- Space: O(N) — prefix & suffix arrays

### Approach 3: Two Pointers (Optimal)

**Idea:**
- Use two pointers moving inward
- Maintain leftMax and rightMax
- Decide side based on smaller boundary

**Steps:**
- Initialize left = 0, right = n - 1
- Maintain leftMax, rightMax
- Move pointer with smaller height
- Accumulate trapped water

**Java Code:**
```java
public int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int leftMax = 0, rightMax = 0;
    int water = 0;

    while (left <= right) {
        if (height[left] <= height[right]) {
            if (height[left] >= leftMax)
                leftMax = height[left];
            else
                water += leftMax - height[left];
            left++;
        } else {
            if (height[right] >= rightMax)
                rightMax = height[right];
            else
                water += rightMax - height[right];
            right--;
        }
    }
    return water;
}
```

**💭 Intuition Behind the Approach:**
- Water is limited by the shorter side
- If left side is smaller, right side boundary is guaranteed
- Eliminates extra arrays

**Complexity (Time & Space):**
- Time: O(N) — single traversal
- Space: O(1) — constant extra space
- ✅ Interview-Preferred Solution

### Approach 4: Monotonic Stack (Valley Trapping)

**Idea:**
- Use a monotonic decreasing stack that stores indices of bars.
- The stack helps detect “valleys” between two taller bars where water can be trapped.
- When we find a bar taller than the bar at the top of the stack:
  * the popped bar becomes the bottom of the valley
  * the current bar is the right boundary
  * the new stack top is the left boundary
- Water trapped is calculated using:
  * width = rightIndex − leftIndex − 1
  * height = min(leftHeight, rightHeight) − valleyHeight

**Steps:**
- Traverse the array from left to right
- Maintain a stack of indices with decreasing heights
- While current height is greater than the height at stack top:
  * pop the valley index
  * if stack becomes empty, break (no left boundary)
  * calculate trapped water using left and right boundaries
- Push current index into the stack

**Java Code:**
```java
public int trapStack(int[] height) {
    Stack<Integer> st = new Stack<>();
    int water = 0;

    for (int i = 0; i < height.length; i++) {
        while (!st.isEmpty() && height[i] > height[st.peek()]) {
            int mid = st.pop();   // valley

            if (st.isEmpty()) break;

            int left = st.peek();
            int width = i - left - 1;
            int h = Math.min(height[left], height[i]) - height[mid];

            water += width * h;
        }
        st.push(i);
    }
    return water;
}
```

**💭 Intuition Behind the Approach:**
- public int trapStack(int[] height) {
    * Stack<Integer> st = new Stack<>();
    * int water = 0;
    * for (int i = 0; i < height.length; i++) {
        * while (!st.isEmpty() && height[i] > height[st.peek()]) {
            * int mid = st.pop();   // valley
            * if (st.isEmpty()) break;
            * int left = st.peek();
            * int width = i - left - 1;
            * int h = Math.min(height[left], height[i]) - height[mid];
            * water += width * h;
        * }
        * st.push(i);
    * }
    * return water;
- }

**Complexity (Time & Space):**
- Time: O(N) — each index is pushed and popped at most once
- Space: O(N) — stack stores indices
- Note
  * This approach is mainly used to teach the monotonic stack pattern.
  * The two-pointer approach is still the most optimal and interview-preferred solution.

---

## 7. Justification / Proof of Optimality

- Brute force matches definition but inefficient
- DP optimizes time with extra space
- Two-pointer solution achieves optimal balance
---

## 8. Variants / Follow-Ups

- Container With Most Water
- Maximum area histogram
- Trapping rainwater II (2D grid)
---

## 9. Tips & Observations

- Always think in terms of boundaries
- Minimum of left & right max controls water
- Two pointers eliminate redundant memory
---

## 10. Pitfalls

- Forgetting to clamp negative water
- Using wrong boundary in two-pointer logic
- Confusing with container problem
- Off-by-one errors in loops
---

<!-- #endregion -->
