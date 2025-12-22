<!-- #region 198-Implement Queue Using Arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q198: Implement Queue Using Arrays</h1>

## 1. Problem Statement

- Implement a First-In-First-Out (FIFO) queue using an array.
- The queue must support the following operations:
  * push(int x) → add element at the end
  * pop() → remove and return the front element
  * peek() → return the front element without removing it
  * isEmpty() → check if queue is empty
---

## 2. Problem Understanding

- Queue follows FIFO principle
- Insertion happens at the rear
- Deletion happens from the front
- Array is used as the underlying data structure
- We must track:
- front → index of first element
- rear → index of last inserted element
---

## 3. Constraints

- 1 <= number of operations <= 100
- 1 <= x <= 100
- Fixed-size array is sufficient
---

## 4. Edge Cases

- pop() when queue is empty
- peek() when queue is empty
- isEmpty() immediately after initialization
- Single element enqueue → dequeue
---

## 5. Examples

```text
Example 1

Input:

["ArrayQueue", "push", "push", "peek", "pop", "isEmpty"]
[[], [5], [10], [], [], []]


Output:

[null, null, null, 5, 5, false]

Example 2

Input:

["ArrayQueue", "isEmpty"]
[[]]


Output:

[null, true]

Example 3

Input:

["ArrayQueue", "push", "pop", "isEmpty"]
[[], [1], [], []]


Output:

[null, null, 1, true]
```

---

## 6. Approaches

### Approach 1: Queue Using Array (Simple Linear Queue)

**Idea:**
- Use an integer array
- Maintain two pointers:
  * front → points to current front
  * rear → points to last inserted element
- Queue is empty when front > rear

**Steps:**
- Initialize:
  * front = 0
  * rear = -1
  * push(x):
  * Increment rear
  * Store x at arr[rear]
- pop():
  * If empty, return -1
  * Return arr[front]
  * Increment front
- peek():
  * If empty, return -1
  * Return arr[front]
- isEmpty():
  * Return front > rear

**Java Code:**
```java
class ArrayQueue {
    int[] arr;
    int front, rear;

    ArrayQueue() {
        arr = new int[100];
        front = 0;
        rear = -1;
    }

    void push(int x) {
        arr[++rear] = x;
    }

    int pop() {
        if (isEmpty()) return -1;

        int val = arr[front++];

        // 🔥 reset when queue becomes empty
        if (front > rear) {
            front = 0;
            rear = -1;
        }
        return val;
    }

    int peek() {
        if (isEmpty()) return -1;
        return arr[front];
    }

    int size() {
        return rear - front + 1;
    }

    boolean isEmpty() {
        return size() == 0;
    }
}

```

**💭 Intuition Behind the Approach:**
- Queue processes elements in arrival order
- front always represents the oldest element
- rear tracks the most recent insertion
- Array provides constant-time access using indices

**Complexity (Time & Space):**
- Time: O(1) — direct index access for all operations
- Space: O(N) — array storage

---

## 7. Justification / Proof of Optimality

- This approach correctly simulates FIFO behavior using an array with constant-time operations and minimal overhead, making it suitable for small constraints.
---

## 8. Variants / Follow-Ups

- Queue using Linked List
- Circular Queue (to reuse freed space)
- Queue using Two Stacks
- Deque (Double Ended Queue)
---

## 9. Tips & Observations

- Linear array queue wastes space after many dequeues
- Circular queue solves space wastage
- FIFO problems often appear in:
  * BFS
  * Sliding Window
  * Scheduling problems
---

## 10. Pitfalls

- Forgetting empty condition before pop() / peek()
- Confusing queue with stack (FIFO vs LIFO)
- Not resetting pointers correctly
- Ignoring space wastage in linear queue
---

<!-- #endregion -->
