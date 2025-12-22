<!-- #region 232-Sliding Window Maximum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q232: Sliding Window Maximum</h1>

## 1. Problem Statement

- You are given an array of integers nums of size N and an integer K representing the window size.
- A sliding window of size K moves from left to right by one position at a time.
- At each position, return the maximum element present in the current window.
- You need to return an array containing the maximum of each window.
---

## 2. Problem Understanding

- Window size is fixed (K)
- Window moves one step at a time
- For each window, we need the maximum
- Brute force would recompute max for every window
- We need to reuse information from previous windows
---

## 3. Constraints

- 1 <= N <= 20000
- 1 <= K <= N
- -10^4 <= arr[i] <= 10^4
---

## 4. Edge Cases

- K = 1 → answer is the array itself
- K = N → answer has one element (max of whole array)
- All elements equal
- Negative numbers
---

## 5. Examples

```text
Input:
nums = [1,3,-1,-3,5,3,6,7], K = 3

Output:
[3, 3, 5, 5, 6, 7]
```

---

## 6. Approaches

### Approach 1: Brute Force (Naive)

**Idea:**
- For every window of size K, scan all K elements and find the maximum.

**Steps:**
- For i from 0 to N-K
- For each window [i ... i+K-1], find max
- Store result

**Java Code:**
```java
static int[] slidingWindowMaxBrute(int[] arr, int n, int k) {
    int[] ans = new int[n - k + 1];

    for (int i = 0; i <= n - k; i++) {
        int max = arr[i];
        for (int j = i; j < i + k; j++) {
            max = Math.max(max, arr[j]);
        }
        ans[i] = max;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Direct simulation of the problem
- Simple but repetitive work
- Does not reuse previous window information

**Complexity (Time & Space):**
- Time: O(N*K) — each window scans K elements
- Space: O(1) — excluding output

### Approach 2: Using Max Heap (Better)

**Idea:**
- Use a max heap to always get the maximum element of the current window.

**Steps:**
- Push (value, index) into heap
- Remove elements that are outside the window
- Heap top gives current max

**Java Code:**
```java
static int[] slidingWindowMaxHeap(int[] arr, int n, int k) {
    PriorityQueue<int[]> pq =
        new PriorityQueue<>((a, b) -> b[0] - a[0]);

    int[] ans = new int[n - k + 1];
    int idx = 0;

    for (int i = 0; i < n; i++) {
        pq.offer(new int[]{arr[i], i});

        while (pq.peek()[1] <= i - k) {
            pq.poll();
        }

        if (i >= k - 1) {
            ans[idx++] = pq.peek()[0];
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Heap keeps max accessible
- Old elements are lazily removed
- Better than brute force but still not optimal

**Complexity (Time & Space):**
- Time: O(N log N) — heap operations
- Space: O(N) — heap storage

### Approach 3: Monotonic Deque (Optimal & Interview-Expected)

**Idea:**
- Maintain a deque of indices such that:
  * Elements are in decreasing order
  * Front of deque is always the maximum

**Steps:**
- Remove indices from front that are outside window
- Remove smaller elements from back
- Add current index
- Front of deque is max for current window

**Java Code:**
```java
static int[] slidingWindowMaximum(int[] arr, int n, int k) {
    Deque<Integer> dq = new ArrayDeque<>();
    int[] ans = new int[n - k + 1];
    int idx = 0;

    for (int i = 0; i < n; i++) {

        // remove out-of-window indices
        while (!dq.isEmpty() && dq.peekFirst() <= i - k) {
            dq.pollFirst();
        }

        // maintain decreasing order
        while (!dq.isEmpty() && arr[dq.peekLast()] <= arr[i]) {
            dq.pollLast();
        }

        dq.addLast(i);

        // record answer
        if (i >= k - 1) {
            ans[idx++] = arr[dq.peekFirst()];
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Deque stores useful candidates only
- Smaller elements are discarded early
- Each element enters and leaves deque once
- Front always holds max

**Complexity (Time & Space):**
- Time: O(N) — each index processed once
- Space: O(K) — deque stores window indices

---

## 7. Justification / Proof of Optimality

- Brute force is too slow
- Heap improves but still logarithmic
- Monotonic deque achieves linear time
- This is the industry-standard solution
---

## 8. Variants / Follow-Ups

- Sliding Window Minimum
- First negative number in every window
- Count distinct elements in window
- Maximum of minimums of every window size
---

## 9. Tips & Observations

- Always store indices, not values
- Deque problems are about order + validity
- Remove out-of-window elements first
- Monotonic structures appear frequently
---

## 10. Pitfalls

- Storing values instead of indices
- Forgetting to remove out-of-window indices
- Incorrect comparison (<= vs <)
- Using heap when O(N) solution exists
---

<!-- #endregion -->
