<!-- #region 228-Circular Queue -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q228: Circular Queue</h1>

## 1. Problem Statement

- Implement a Circular Queue using an array that supports the following operations:
  * push(x) → Insert element x into the queue
  * pop() → Remove and return the front element of the queue
  * front() → Return the front element without removing it
  * size() → Return the number of elements currently in the queue
- Key Requirements
  * The queue must use a fixed-size array
  * Efficiently utilize space by reusing empty positions
  * All operations should work in O(1) time
---

## 2. Problem Understanding

- A Circular Queue is an improved version of a normal array-based queue.
- Why not a normal queue?
- In a linear queue:
  * After several pop() operations, front moves forward
  * Empty spaces at the beginning cannot be reused
  * This causes space wastage
- 👉 Circular Queue solves this by:
  * Treating the array as circular
  * Using modulo (%) to wrap indices
---

## 3. Constraints

- Fixed-size array
- Maximum size is known beforehand
- 0 <= number of elements <= size
- All operations must be constant time
---

## 4. Edge Cases

- Queue is empty
- Queue is full
- First insertion
- Last deletion (queue becomes empty)
- Wrap-around when rear reaches end
---

## 5. Examples

```text
push(10) → [10, _, _, _, _]
push(20) → [10, 20, _, _, _]
push(30) → [10, 20, 30, _, _]
pop()    → [_, 20, 30, _, _]
push(40) → [40, 20, 30, _, _]   ← wrap-around
```

---

## 6. Approaches

### Approach 1: Circular Queue using Array (Optimal)

**Idea:**
- Use % size to reuse array positions
- Queue is full when the next position of rear hits front
- Queue is empty when front == -1
- Reset pointers when queue becomes empty
- 🔹 Conditions (Very Important)
    * Empty  → front == -1
    * Full   → (rear + 1) % size == front

**Java Code:**
```java
class CircularQueue {
    int[] arr;
    int front, rear, size;

    CircularQueue(int size) {
        this.size = size;
        arr = new int[size];
        front = -1;
        rear = -1;
    }

    // enqueue
    void push(int x) {
        if ((rear + 1) % size == front) {
            return; // queue full
        }

        if (front == -1) {
            front = 0;
            rear = 0;
        } else {
            rear = (rear + 1) % size;
        }

        arr[rear] = x;
    }

    // dequeue
    int pop() {
        if (front == -1) return -1;

        int val = arr[front];

        if (front == rear) {
            front = -1;
            rear = -1;
        } else {
            front = (front + 1) % size;
        }

        return val;
    }

    // peek
    int front() {
        if (front == -1) return -1;
        return arr[front];
    }

    // size
    int size() {
        if (front == -1) return 0;
        if (rear >= front)
            return rear - front + 1;
        return size - (front - rear - 1);
    }
}
```

**Complexity (Time & Space):**
- Time: O(1) — index updates use constant-time modulo
- Space: O(N) — fixed-size array storage

---

## 7. Justification / Proof of Optimality

- Prevents space wastage present in linear queue
- Supports all queue operations in constant time
- Standard interview-expected implementation
---

## 8. Variants / Follow-Ups

- Circular Queue using Linked List
- Deque (Double Ended Queue)
- Dynamic Circular Queue (resizable)
---

## 9. Tips & Observations

- Circular Queue is conceptually important even if not directly coded
- % size is the heart of the solution
- Always reset pointers after last removal
- Deque problems are extensions of circular queue logic
---

## 10. Pitfalls

- Forgetting modulo while incrementing
- Wrong full condition
- Not resetting front and rear
- Mixing linear and circular queue logic
---

<!-- #endregion -->
