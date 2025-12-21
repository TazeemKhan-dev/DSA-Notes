<!-- #region 202-Implement Queue using stack - Dequeue O(1) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q202: Implement Queue using stack - Dequeue O(1)</h1>

## 1. Problem Statement

- You are required to implement a Queue using two Stacks S1 and S2.
- You need to support the following operations:
  * Type 1 x → Insert (enqueue) the element x into the queue.
  * Type 2 → Remove (dequeue) the front element from the queue and print it.
    * If the queue is empty, print -1.
- Important Requirement
  * The dequeue operation must run in O(1) time.
---

## 2. Problem Understanding

- A queue follows FIFO (First In First Out) order, while a stack follows LIFO (Last In First Out).
- Since dequeue must be O(1), we:
  * Make enqueue costly
  * Ensure the front element is always on top of a stack
- We are allowed to use two stacks only.
---

## 3. Constraints

- 1 <= Total number of queries <= 100
- 1 <= value of x <= 100
- Only stack operations (push, pop, peek, isEmpty) are allowed
---

## 4. Edge Cases

- Dequeue on empty queue → return -1
- Multiple enqueue operations before dequeue
- Interleaved enqueue and dequeue operations
---

## 5. Examples

```text
Input

5
1 2
1 3
2
1 4
2


Output

2 3
```

---

## 6. Approaches

### Approach 1: Costly Enqueue, Cheap Dequeue (Optimal as per requirement)

**Idea:**
- Always keep the front of the queue on top of S1
- Use S2 only during enqueue
- This guarantees deQueue() in O(1)

**Steps:**
- enQueue(x):
  * Move all elements from S1 to S2
  * Push x into S1
  * Move all elements back from S2 to S1
- deQueue():
  * If S1 is empty → return -1
  * Else → pop from S1

**Java Code:**
```java
class QueueUsingStack {
    Stack<Integer> s1 = new Stack<>();
    Stack<Integer> s2 = new Stack<>();

    // Enqueue operation (O(N))
    void push(int x) {
        while (!s1.isEmpty()) {
            s2.push(s1.pop());
        }

        s1.push(x);

        while (!s2.isEmpty()) {
            s1.push(s2.pop());
        }
    }

    // Dequeue operation (O(1))
    int pop() {
        if (s1.isEmpty()) {
            return -1;
        }
        return s1.pop();
    }
}
```

**💭 Intuition Behind the Approach:**
- Queue needs FIFO
- Stack gives LIFO
- By reversing the stack during enqueue, we insert the new element at the bottom
- After restoring, the front element stays on top, making dequeue constant time

**Complexity (Time & Space):**
- Time: O(N) — enqueue shifts all elements twice
- Space: O(N) — two stacks storing elements

---

## 7. Justification / Proof of Optimality

- This approach satisfies the problem requirement exactly by ensuring:
- deQueue runs in O(1)
- Cost is intentionally shifted to enQueue
---

## 8. Variants / Follow-Ups

- enQueue O(1), deQueue O(N) → lazy two-stack approach
- Amortized O(1) dequeue → interview-friendly but not accepted here
---

## 9. Tips & Observations

- If problem explicitly says “deQueue O(1)”, never use the lazy two-stack solution
- Always clarify worst-case vs amortized in interviews
---

## 10. Pitfalls

- Using the enQueue O(1) logic by mistake
- Forgetting to move elements back to S1
- Missing empty queue check
---

<!-- #endregion -->
