<!-- #region 250-Design Ring Buffer -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q250: Design Ring Buffer</h1>

## 1. Problem Statement

- Design a Circular Queue (Ring Buffer) that supports the following operations without using any built-in queue data structure.
- You are required to implement the AccioQueue class with the following methods:
  * AccioQueue(k) → Initialize the queue with size k
  * int Front() → Return the front element, or -1 if empty
  * int Rear() → Return the rear element, or -1 if empty
  * boolean enQueue(int value) → Insert an element, return true if successful
  * boolean deQueue() → Remove an element, return true if successful
  * boolean isEmpty() → Check if queue is empty
  * boolean isFull() → Check if queue is full
- Input Queries
  * 0 X → Create queue of size X
  * 1 → Front
  * 2 → Rear
  * 3 X → enQueue(X)
  * 4 → deQueue
  * 5 → isEmpty
  * 6 → isFull
---

## 2. Problem Understanding

- A circular queue is a FIFO structure where the last position is connected back to the first.
- Unlike a normal queue, empty space at the front can be reused.
- Indices wrap around using modulo arithmetic.
- We track:
  * front → first valid element
  * rear → last valid element
---

## 3. Constraints

- 1 <= k <= 1000
- 0 <= value <= 1000
- 0 <= number of queries <= 300
---

## 4. Edge Cases

- Dequeue on empty queue
- Enqueue on full queue
- Single element queue (front == rear)
- Wrap-around condition ((rear + 1) % size)
- Calling operations before initialization
---

## 5. Examples

```text
Input:
10
0 3
3 1
3 2
3 3
3 4
2
6
4
3 4
2

Output:
null true true true false 3 true true true 4
```

---

## 6. Approaches

### Approach 1: Brute Force Using Array + Shifting (NOT OPTIMAL)

**Idea:**
- Use an array
- On dequeue, shift elements left
- 🚫 Why Not Used
  * Shifting costs O(n) per operation
  * Fails time efficiency requirement

**💭 Intuition Behind the Approach:**
- This mimics a normal queue but wastes time by shifting elements.

**Complexity (Time & Space):**
- Time: O(n) — shifting on every deQueue
- Space: O(n)

### Approach 2: Circular Queue Using Modulo (OPTIMAL)

**Idea:**
- Use a fixed size array
- Maintain front and rear
- Use modulo % size to wrap around

**Steps:**
- Initialize:
  * front = -1, rear = -1
- enQueue
  * If full → return false
  * First insert → set front = rear = 0
  * Else → rear = (rear + 1) % size
- deQueue
  * If empty → return false
  * If only one element → reset both to -1
  * Else → front = (front + 1) % size
- isEmpty
  * front == -1
- isFull
  * (rear + 1) % size == front

**Java Code:**
```java
import java.util.*;

public class Main {

    static AccioQueue curr = null; // MUST be static

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        for (int i = 0; i < n; i++) {
            int call = sc.nextInt();

            if (call == 0 || call == 3) {
                int value = sc.nextInt();
                FunctionToBeCalled(call, value);
            } else {
                FunctionToBeCalled(call);
            }
        }
    }

    static void FunctionToBeCalled(int call) {
        FunctionToBeCalled(call, 0);
    }

    static void FunctionToBeCalled(int call, int value) {
        switch (call) {
            case 0:
                curr = new AccioQueue(value);
                System.out.print("null ");
                break;
            case 1:
                System.out.print(curr.Front() + " ");
                break;
            case 2:
                System.out.print(curr.Rear() + " ");
                break;
            case 3:
                System.out.print(curr.enQueue(value) + " ");
                break;
            case 4:
                System.out.print(curr.deQueue() + " ");
                break;
            case 5:
                System.out.print(curr.isEmpty() + " ");
                break;
            case 6:
                System.out.print(curr.isFull() + " ");
                break;
        }
    }

    static class AccioQueue {
        int[] arr;
        int front, rear, size;

        AccioQueue(int size) {
            this.size = size;
            arr = new int[size];
            front = -1;
            rear = -1;
        }

        int Front() {
            if (isEmpty()) return -1;
            return arr[front];
        }

        int Rear() {
            if (isEmpty()) return -1;
            return arr[rear];
        }

        boolean enQueue(int value) {
            if (isFull()) return false;
            if (front == -1) {
                front = 0;
                rear = 0;
            } else {
                rear = (rear + 1) % size;
            }
            arr[rear] = value;
            return true;
        }

        boolean deQueue() {
            if (isEmpty()) return false;
            if (front == rear) {
                front = -1;
                rear = -1;
                return true;
            }
            front = (front + 1) % size;
            return true;
        }

        boolean isEmpty() {
            return front == -1;
        }

        boolean isFull() {
            return (rear + 1) % size == front;
        }
    }
}
```

**💭 Intuition Behind the Approach:**
- We reuse space instead of shifting elements.
- Modulo ensures indices stay within bounds.
- Resetting when front == rear handles the last element case cleanly.

**Complexity (Time & Space):**
- Time: O(1) — each operation is constant time
- Space: O(n) — fixed array size

---

## 7. Justification / Proof of Optimality

- Circular queue eliminates wasted space.
- Modulo arithmetic avoids shifting.
- All operations meet optimal time complexity.
---

## 8. Variants / Follow-Ups

- Deque (Double-Ended Queue)
- Round-Robin CPU Scheduling
- Producer–Consumer buffer
- Ring buffer in OS / networking
---

## 9. Tips & Observations

- Circular queue always keeps one empty slot logically
- Full condition is next rear hits front
- Reset pointers when queue becomes empty
- Very common interview + system design topic
---

## 10. Pitfalls

- Forgetting modulo % size
- Not resetting front and rear
- Using = instead of == in isFull
- Mishandling single element case
---

<!-- #endregion -->
