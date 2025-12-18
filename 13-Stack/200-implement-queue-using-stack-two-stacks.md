<!-- #region 200-Implement Queue Using Stack (Two Stacks) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q200: Implement Queue Using Stack (Two Stacks)</h1>

## 1. Problem Statement

- Implement a First-In-First-Out (FIFO) queue using two stacks.
- The queue must support the following operations:
  * push(int x) → add element to the end of the queue
  * pop() → remove and return the front element
  * peek() → return the front element without removing it
  * isEmpty() → check if the queue is empty
---

## 2. Problem Understanding

- Queue follows FIFO
- Stack follows LIFO
- Using two stacks, we must reverse order to simulate FIFO
- One stack is used for insertion, the other for removal
- The oldest element must be returned first
---

## 3. Constraints

- 1 <= number of operations <= 100
- 1 <= x <= 100
- Only stack operations are allowed
---

## 4. Edge Cases

- pop() on empty queue
- peek() on empty queue
- isEmpty() immediately after initialization
- Single element enqueue → dequeue
---

## 5. Examples

```text
Example 1

Input:

["StackQueue", "push", "push", "pop", "peek", "isEmpty"]
[[], [4], [8], [], [], []]


Output:

[null, null, null, 4, 8, false]

Example 2

Input:

["StackQueue", "isEmpty"]
[[]]


Output:

[null, true]

Example 3

Input:

["StackQueue", "push", "pop", "isEmpty"]
[[], [6], [], []]


Output:

[null, null, 6, true]
```

---

## 6. Approaches

### Approach 1: Queue Using Two Stacks (Amortized Optimal)

**Idea:**
- Use two stacks:
  * inStack → for push operations
  * outStack → for pop and peek operations
- Reverse order only when required

**Steps:**
- push(x):
  * Push x into inStack
- pop():
  * If both stacks empty → return -1
  * If outStack empty:
  * Move all elements from inStack to outStack
  * Pop from outStack
- peek():
  * Same logic as pop, but return top without removing
- isEmpty():
  * Return true if both stacks are empty

**Java Code:**
```java
class StackQueue {
    Stack<Integer> in = new Stack<>();
    Stack<Integer> out = new Stack<>();

    void push(int x) {
        in.push(x);
    }

    int pop() {
        if (isEmpty()) return -1;

        if (out.isEmpty()) {
            while (!in.isEmpty()) {
                out.push(in.pop());
            }
        }
        return out.pop();
    }

    int peek() {
        if (isEmpty()) return -1;

        if (out.isEmpty()) {
            while (!in.isEmpty()) {
                out.push(in.pop());
            }
        }
        return out.peek();
    }

    boolean isEmpty() {
        return in.isEmpty() && out.isEmpty();
    }
}
```

**💭 Intuition Behind the Approach:**
- inStack stores elements in arrival order
- outStack reverses order to expose the oldest element on top
- Each element moves from inStack to outStack only once
- This ensures FIFO behavior using LIFO structures

**Complexity (Time & Space):**
- Time: O(1) amortized — each element is pushed and popped once
- Space: O(N) — two stacks store all elements

---

## 7. Justification / Proof of Optimality

- Using two stacks efficiently simulates queue behavior by reversing element order only when necessary, ensuring optimal performance and correct FIFO semantics.
---

## 8. Variants / Follow-Ups

- Queue using single stack (recursion-based)
- Queue using array
- Queue using linked list
- Circular queue
- Deque (double-ended queue)
---

## 9. Tips & Observations

- Always check outStack before popping
- Lazy transfer minimizes unnecessary operations
- This pattern appears frequently in interview problems
- Similar logic is used in amortized analysis questions
---

## 10. Pitfalls

- Transferring elements on every pop
- Forgetting empty check before pop/peek
- Confusing stack top with queue front
- Assuming pop is always O(1) (it’s amortized)
---

<!-- #endregion -->
