<!-- #region 220-Sum of Subarray Minimums -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q220: Sum of Subarray Minimums</h1>

## 1. Problem Statement

- Sum of Subarray Minimums
- Given an integer array arr, find the sum of the minimum element of every contiguous subarray.
- Since the result can be large, return the answer modulo 10^9 + 7.
---

## 2. Problem Understanding

- You must consider all possible contiguous subarrays
- For each subarray, take its minimum element
- Add all those minimums together
- Brute force is infeasible due to large constraints
- Key realization:
  * Instead of iterating over all subarrays, count how many subarrays consider a given element as the minimum.
---

## 3. Constraints

- 1 <= n <= 3 * 10^4
- 1 <= arr[i] <= 3 * 10^4
- Answer must be returned modulo 10^9 + 7
---

## 4. Edge Cases

- Single element array
- Strictly increasing array
- Strictly decreasing array
- Duplicate values (important for monotonic stack logic)
---

## 5. Examples

```text
Example 1
Input

4
3 1 2 4
Output
17

Explanation

Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4].
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.

Example 2
Input

3
1 2 3
Output
10

Explanation

Subarrays are [1], [2], [3], [1,2], [2,3], [1,2,3].
Minimums are 1, 2, 3, 1, 2, 1.
Sum is 10.
```

---

## 6. Approaches

### Approach 1: Brute Force (TLE)

**Idea:**
- Generate all subarrays
- Find minimum of each
- Add them up

**Steps:**
- Fix starting index i
- Extend subarray till j
- Track minimum while extending

**Java Code:**
```java
public long minSubarraySum(int[] arr, int n) {
    long sum = 0;
    for (int i = 0; i < n; i++) {
        int min = Integer.MAX_VALUE;
        for (int j = i; j < n; j++) {
            min = Math.min(min, arr[j]);
            sum += min;
        }
    }
    return sum;
}
```

**💭 Intuition Behind the Approach:**
- Every subarray is explicitly evaluated, but this repeats work heavily.

**Complexity (Time & Space):**
- Time: O(N^2) — nested loops over all subarrays
- Space: O(1) — no extra space

### Approach 2: Contribution Technique + Previous/Next Smaller (Optimal)

**Idea:**
- Each element arr[i] contributes to the answer as:
  * arr[i] * (number of subarrays where arr[i] is the minimum)
- To compute this:
  * Find Previous Smaller Element (PSE)
  * Find Next Smaller Element (NSE)
- Then:
  * left  = i - PSE
  * right = NSE - i
  * contribution = arr[i] * left * right

**Steps:**
- Use monotonic increasing stack to find:
  * PSE (strictly smaller → >)
  * NSE (smaller or equal → >=)
- For each index:
  * Calculate how many subarrays take arr[i] as minimum
- Add contribution modulo 10^9 + 7

**Java Code:**
```java
public int minSubarraySum(int[] arr, int n) {
    int MOD = 1000000007;

    int[] pse = new int[n];
    int[] nse = new int[n];

    Stack<Integer> st = new Stack<>();

    // Previous Smaller Element
    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && arr[st.peek()] > arr[i]) {
            st.pop();
        }
        pse[i] = st.isEmpty() ? -1 : st.peek();
        st.push(i);
    }

    st.clear();

    // Next Smaller Element
    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
            st.pop();
        }
        nse[i] = st.isEmpty() ? n : st.peek();
        st.push(i);
    }

    long sum = 0;
    for (int i = 0; i < n; i++) {
        long left = i - pse[i];
        long right = nse[i] - i;
        sum = (sum + arr[i] * left % MOD * right % MOD) % MOD;
    }

    return (int) sum;
}
```

**💭 Intuition Behind the Approach:**
- Each element becomes the minimum for a range of subarrays
- PSE decides how far left it can dominate
- NSE decides how far right it can dominate
- Multiplying both gives the number of valid subarrays

**Complexity (Time & Space):**
- Time: O(N) — each element pushed and popped once
- Space: O(N) — stacks and auxiliary arrays

---

## 7. Justification / Proof of Optimality

- Brute force fails for large n
- Contribution technique converts the problem into counting dominance
- Monotonic stacks ensure linear time
- This is the standard optimal solution asked in interviews
---

## 8. Variants / Follow-Ups

- Sum of Subarray Maximums
- Largest Rectangle in Histogram
- Maximal Rectangle
- Minimum Cost Tree From Leaf Values
---

## 9. Tips & Observations

- Use > for PSE and >= for NSE to avoid double counting
- This is NOT a sliding window problem
- Always think in terms of element contribution, not subarrays
---

## 10. Pitfalls

- Using same comparison (> or >=) on both sides
- Forgetting modulo during multiplication
- Using absolute counts instead of distances
- Overwriting first occurrence in stack logic
- Treating it as brute-force sliding window
---

<!-- #endregion -->
