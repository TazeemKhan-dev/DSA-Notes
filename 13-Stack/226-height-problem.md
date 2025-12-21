<!-- #region 226-Height Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q226: Height Problem</h1>

## 1. Problem Statement

- n people are standing in a line from left to right.
- Each person wants to know the height of the closest person on the left who is strictly shorter than them.
- If there are multiple such people, choose the closest one.
- If no such person exists, return -1.
---

## 2. Problem Understanding

- For each index i:
  * Look only to the left (0 … i-1)
  * Among people with height < arr[i]
  * Pick the nearest one
  * Output their height, not index
- This is a Previous Smaller Element (PSE) problem.
---

## 3. Constraints

- 2 ≤ n ≤ 10^5
- 0 ≤ arr[i] ≤ 10^9
- Large n → brute force will TLE
---

## 4. Edge Cases

- First person → always -1
- Strictly increasing array
- Strictly decreasing array
- Equal heights (not allowed, must be less than)
---

## 5. Examples

```text
Input

5
1 2 3 4 5


Output

-1 1 2 3 4
```

---

## 6. Approaches

### Approach 1: Brute Force (Naive) 

**Idea:**
- For each person, scan leftwards until you find a shorter height.

**Steps:**
- For each i:
  * Start from j = i-1
  * Move left while arr[j] >= arr[i]
  * First arr[j] < arr[i] → answer
  * If none found → -1

**Java Code:**
```java
public int[] heightProblemBrute(int[] arr) {
    int n = arr.length;
    int[] ans = new int[n];

    for (int i = 0; i < n; i++) {
        ans[i] = -1;
        for (int j = i - 1; j >= 0; j--) {
            if (arr[j] < arr[i]) {
                ans[i] = arr[j];
                break;
            }
        }
    }
    return ans;
}
```

**Complexity (Time & Space):**
- Time: O(N^2) — nested loop, worst case decreasing array
- Space: O(1) — output array excluded

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Maintain a monotonic increasing stack of heights.
- Stack invariant:
  * Stack stores candidates for previous smaller
  * Pop elements that are ≥ current height

**Steps:**
- Create empty stack
- Traverse array from left to right
- While stack is not empty and stack.peek() >= arr[i], pop
- If stack is empty → answer -1
- Else → answer stack.peek()
- Push arr[i] to stack

**Java Code:**
```java
public int[] heightProblem(int[] arr) {
    int n = arr.length;
    int[] ans = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && st.peek() >= arr[i]) {
            st.pop();
        }

        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(arr[i]);
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We only care about nearest smaller on the left
- Taller or equal people are useless for future elements
- Stack keeps only valid smaller candidates
- Each element is pushed and popped once

**Complexity (Time & Space):**
- Time: O(N) — each element pushed & popped once
- Space: O(N) — stack in worst case (increasing array)

---

## 7. Justification / Proof of Optimality

- The monotonic stack approach is optimal because:
  * Brute force fails for large n
  * Stack reduces redundant comparisons
  * Guarantees nearest smaller element efficiently
---

## 8. Variants / Follow-Ups

- Previous Greater Element
- Next Smaller Element
- Next Greater Element
- Stock Span Problem
- Largest Rectangle in Histogram
---

## 9. Tips & Observations

- Condition is strictly smaller (<)
- Use >= while popping to handle equals
- Output requires value, not index
---

## 10. Pitfalls

- Using > instead of >= in pop condition
- Scanning right side instead of left
- Confusing PSE with NSE
---

<!-- #endregion -->
