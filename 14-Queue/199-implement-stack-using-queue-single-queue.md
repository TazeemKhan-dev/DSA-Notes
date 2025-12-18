<!-- #region 199-Implement Stack Using Queue (Single Queue) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q199: Implement Stack Using Queue (Single Queue)</h1>

## 1. Problem Statement

- Implement a Last-In-First-Out (LIFO) stack using a single queue.
- The stack must support the following operations:
  * push(int x) → push element onto stack
  * pop() → remove and return top element
  * top() → return top element without removing
  * isEmpty() → check if stack is empty
---

## 2. Problem Understanding

- Stack follows LIFO
- Queue follows FIFO
- Only one queue is allowed
- Stack behavior must be simulated using queue operations
- The most recently pushed element must always be removed first
---

## 3. Constraints

- 1 <= number of calls <= 100
- 1 <= x <= 100
- Only one queue can be used
---

## 4. Edge Cases

- pop() on empty stack
- top() on empty stack
- isEmpty() immediately after initialization
- Single element push → pop
---

## 5. Examples

```text
Example 1

Input:

["QueueStack", "push", "push", "pop", "top", "isEmpty"]
[[], [4], [8], [], [], []]


Output:

[null, null, null, 8, 4, false]

Example 2

Input:

["QueueStack", "isEmpty"]
[[]]


Output:

[null, true]

Example 3

Input:

["QueueStack", "push", "pop", "isEmpty"]
[[], [6], [], []]


Output:

[null, null, 6, true]
```

---

## 6. Approaches

### Approach 1: Stack Using Single Queue (Push Costly)

**Idea:**
- Use one queue
- After every push(x), rotate the queue so that:
  * x comes to the front
- This ensures:
  * Front of queue = top of stack

**Steps:**
- Maintain one queue q
- push(x):
  * Add x to queue
  * Rotate the queue size - 1 times
- pop():
  * If empty, return -1
  * Remove and return front element
- top():
  * If empty, return -1
  * Return front element
- isEmpty():
  * Return true if queue is empty

**Java Code:**
```java
class QueueStack {
    Queue<Integer> q = new ArrayDeque<>();

    void push(int x) {
        q.offer(x);
        int size = q.size();
        while (size-- > 1) {
            q.offer(q.poll());
        }
    }

    int pop() {
        if (q.isEmpty()) return -1;
        return q.poll();
    }

    int top() {
        if (q.isEmpty()) return -1;
        return q.peek();
    }

    boolean isEmpty() {
        return q.isEmpty();
    }
}
```

**💭 Intuition Behind the Approach:**
- Queue gives FIFO access
- Rotating after push moves the newest element to the front
- Front of queue always behaves like stack top
- Sacrifices push efficiency to simplify pop and top

**Complexity (Time & Space):**
- Time: O(N) — push rotates all elements
- Time: O(1) — pop, top, isEmpty
- Space: O(N) — queue storage

---

## 7. Justification / Proof of Optimality

- Rotating the queue after each push ensures correct LIFO behavior while using only one queue, satisfying the problem constraints.
---

## 8. Variants / Follow-Ups

- Stack using two queues (push O(1))
- Stack using two queues (pop O(1))
- Stack using array
- Stack using linked list
---

## 9. Tips & Observations

- Single-queue solution always makes push costly
- If push must be O(1), two queues are required
- Rotation count is always queue_size - 1
- Front of queue represents stack top
---

## 10. Pitfalls

- Forgetting to rotate after push
- Rotating incorrect number of times
- Confusing FIFO behavior during pop
- Not checking empty before pop/top
---

<!-- #endregion -->
