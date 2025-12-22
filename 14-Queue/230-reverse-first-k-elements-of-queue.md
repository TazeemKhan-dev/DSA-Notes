<!-- #region 230-Reverse First K Elements of Queue -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q230: Reverse First K Elements of Queue</h1>

## 1. Problem Statement

- Given an integer K and a queue of N integers, reverse the order of the first K elements of the queue, while keeping the remaining elements in the same relative order.
---

## 2. Problem Understanding

- You are given:
  * A queue (FIFO structure)
  * An integer K
- Your task:
  * Reverse only the first K elements
  * Do not disturb the order of the remaining N - K elements
- Important points:
  * Only the prefix of length K is affected
  * Queue nature must be preserved after modification
---

## 3. Constraints

- 1 <= K <= N <= 10000
- 1 <= elements <= 10000
- Efficient solution required
---

## 4. Edge Cases

- K = 1 → queue remains unchanged
- K = N → entire queue is reversed
- Queue with only one element
- Large N → avoid unnecessary operations
---

## 5. Examples

```text
Input:
N = 5, K = 3
Queue = [1, 2, 3, 4, 5]

Output:
[3, 2, 1, 4, 5]
Explanation:

First 3 elements → [1, 2, 3]

Reverse → [3, 2, 1]

Remaining elements stay same → [4, 5]
```

---

## 6. Approaches

### Approach 1: Brute Force using Extra Array (Conceptual)

**Idea:**
- Copy queue to array
- Reverse first K elements in array
- Rebuild queue
- ⚠️ Not preferred in interviews, but helps understanding.

**Steps:**
- Dequeue all elements into array
- Reverse array indices [0..K-1]
- Enqueue elements back

**Java Code:**
```java
static void reverseFirstK_Array(Queue<Integer> q, int k) {
    int n = q.size();
    int[] arr = new int[n];

    for (int i = 0; i < n; i++) {
        arr[i] = q.poll();
    }

    int l = 0, r = k - 1;
    while (l < r) {
        int temp = arr[l];
        arr[l] = arr[r];
        arr[r] = temp;
        l++; r--;
    }

    for (int x : arr) {
        q.add(x);
    }
}
```

**💭 Intuition Behind the Approach:**
- Treat queue like a normal array
- Straightforward but breaks queue abstraction

**Complexity (Time & Space):**
- Time: O(N) — full traversal and rebuild
- Space: O(N) — extra array used

### Approach 2: Using Recursion (Better, but Risky)

**Idea:**
- Use recursion to reverse first K elements
- Stack frame acts as implicit stack

**Steps:**
- Pop first element
- Recursively reverse next K-1
- Insert popped element at rear

**Java Code:**
```java
static void reverseFirstK_Recursion(Queue<Integer> q, int k) {
    if (k == 0) return;

    int x = q.poll();
    reverseFirstK_Recursion(q, k - 1);
    q.add(x);
}

After recursion, rotate remaining N-K elements.

static void reverseK_WithRotation(Queue<Integer> q, int k) {
    int n = q.size();
    reverseFirstK_Recursion(q, k);

    for (int i = 0; i < n - k; i++) {
        q.add(q.poll());
    }
}
```

**💭 Intuition Behind the Approach:**
- Recursion reverses order naturally
- Queue rotation maintains remaining elements

**Complexity (Time & Space):**
- Time: O(N) — recursion + rotation
- Space: O(K) — recursion stack

### Approach 3: Stack + Queue (Optimal & Interview-Preferred)

**Idea:**
- Stack reverses first K elements
- Queue rotation preserves remaining order

**Steps:**
- Push first K elements into stack
- Pop stack back into queue
- Rotate remaining N-K elements

**Java Code:**
```java
static void reverseFirstK(Queue<Integer> q, int k) {
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < k; i++) {
        st.push(q.poll());
    }

    while (!st.isEmpty()) {
        q.add(st.pop());
    }

    int rem = q.size() - k;
    for (int i = 0; i < rem; i++) {
        q.add(q.poll());
    }
}


// In accio boiler plate solution
import java.util.*;
import java.io.*;

public class Main {
    public static void main(String args[]) {
        Scanner input = new Scanner(System.in);
        int n = input.nextInt(), k = input.nextInt();
        Queue<Integer> q = new LinkedList<>();

        for (int i = 0; i < n; i++) {
            q.add(input.nextInt());
        }

        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < k; i++) {
            st.push(q.poll());
        }

        while (!st.isEmpty()) {
            q.add(st.pop());
        }

        int rem = q.size() - k;
        for (int i = 0; i < rem; i++) {
            q.add(q.poll());
        }

        while (q.size() > 0) {
            System.out.print(q.poll() + " ");
        }
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack = reversal
- Queue = order preservation
- Clean separation of responsibilities

**Complexity (Time & Space):**
- Time: O(N) — each element moved once
- Space: O(K) — stack usage

---

## 7. Justification / Proof of Optimality

- Stack-based solution is clean and interview-expected
- Respects queue abstraction
- Efficient and safe for large inputs
---

## 8. Variants / Follow-Ups

- Reverse every K elements
- Reverse first K using Deque
- Reverse queue using only recursion
---

## 9. Tips & Observations

- Stack is the natural choice for reversal
- Queue problems often mix stack usage
- Always rotate remaining elements
---

## 10. Pitfalls

- Forgetting to rotate N-K elements
- Reversing entire queue
- Mishandling K = N
---

<!-- #endregion -->
