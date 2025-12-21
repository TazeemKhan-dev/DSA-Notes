<!-- #region 206-Stock Span Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q212: Stock Span Problem</h1>

## 1. Problem Statement

- You are given stock prices for n consecutive days.
- For each day i, you must calculate how many consecutive days (ending at i, including i) the stock price was less than or equal to the price on day i.
- The span stops as soon as you encounter a strictly greater price to the left.
- In simple words:
  * Span = distance to the nearest greater element on the left, or the entire prefix if none exists.
---

## 2. Problem Understanding

- 1 <= n <= 100000
- 1 <= arr[i] <= 100000
- Large input → brute force will TLE
---

## 3. Edge Cases

- n = 1 → span is always 1
- Strictly increasing array → spans increase as 1,2,3,...
- Strictly decreasing array → all spans are 1
- Equal elements → span continues (<= allowed)
---

## 4. Examples

```text
Input
[100, 80, 60, 70, 60, 75, 85]

Output
[1, 1, 1, 2, 1, 4, 6]
```

---

## 5. Approaches

### Approach 1: Brute Force (Check Left for Every Day)

**Idea:**
- For each day i, move left while prices are <= arr[i] and count days.

**Steps:**
- For each index i
- Start from i-1 and move left
- Stop when you find a price > arr[i]
- Count days including current

**Java Code:**
```java
public int[] stockSpan(int[] arr) {
    int n = arr.length;
    int[] span = new int[n];

    for (int i = 0; i < n; i++) {
        int count = 1;
        int j = i - 1;
        while (j >= 0 && arr[j] <= arr[i]) {
            count++;
            j--;
        }
        span[i] = count;
    }
    return span;
}
```

**💭 Intuition Behind the Approach:**
- We directly simulate the definition of span by checking previous days one by one.

**Complexity (Time & Space):**
- Time: O(N^2) — for each day, worst case scans all previous days
- Space: O(1) — only output array

### Approach 2: Stack (Nearest Greater to Left) — Optimal

**Idea:**
- Span depends on nearest greater element on the left
- Use a monotonic decreasing stack storing indices

**Steps:**
- Create an empty stack (store indices)
- Traverse from left to right
- While stack top price <= current price, pop
- If stack empty → span = i + 1
- Else → span = i - stack.peek()
- Push current index

**Java Code:**
```java
public int[] stockSpan(int[] arr) {
    int n = arr.length;
    int[] span = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && arr[st.peek()] <= arr[i]) {
            st.pop();
        }

        if (st.isEmpty()) {
            span[i] = i + 1;
        } else {
            span[i] = i - st.peek();
        }

        st.push(i);
    }
    return span;
}
```

**💭 Intuition Behind the Approach:**
- Stack keeps only useful candidates that can block the span
- Once a smaller element is popped, it is never needed again
- Each element is pushed and popped at most once

**Complexity (Time & Space):**
- Time: O(N) — amortized, each element pushed and popped once
- Space: O(N) — stack in worst case (strictly decreasing array)

---

## 6. Justification / Proof of Optimality

- Brute force is intuitive but inefficient
- Stack approach efficiently tracks the nearest greater element
- This is a classic monotonic stack problem
---

## 7. Variants / Follow-Ups

- Nearest Greater Element to Left
- Daily Temperatures
- Largest Rectangle in Histogram
- Online Stock Span (streaming version)
---

## 8. Tips & Observations

- Span is NOT sliding window
- Condition is <=, not <
- Stack stores indices, not values
- Empty stack means full span till start
---

## 9. Pitfalls

- Using < instead of <=
- Forgetting to include the current day
- Storing values instead of indices
- Trying nested loops for large n
---

<!-- #endregion -->
