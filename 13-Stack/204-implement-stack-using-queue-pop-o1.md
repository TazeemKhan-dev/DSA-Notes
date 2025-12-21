<!-- #region 204-Implement Stack using Queue-pop O(1) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q204: Implement Stack using Queue-pop O(1)</h1>

## 1. Problem Statement

- You are required to implement a Stack using two Queues.
- You need to support the following operations:
  * Type 1 x → Push the element x into the stack.
  * Type 2 → Pop the top element from the stack and print it.
    * If the stack is empty, print -1.
- Important Requirement
  * The pop operation must run in O(1) time.
---

## 2. Problem Understanding

- A stack follows LIFO (Last In First Out) order, while a queue follows FIFO (First In First Out).
- Since pop must be O(1), we:
  * Make push expensive
  * Always maintain the top element at the front of a queue
- We are allowed to use two queues only.
---

## 3. Constraints

- Constraints
- 1 <= Total number of queries <= 100
- 1 <= value of x <= 100
- Only queue operations (add, remove, peek, isEmpty) are allowed
---

## 4. Edge Cases

- Pop on empty stack → return -1
- Multiple push operations before pop
- Interleaved push and pop operations
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

3 4
```

---

## 6. Approaches

### Approach 1: Costly Push, Cheap Pop (Required)

**Idea:**
- Always keep the most recently pushed element at the front of q1
- Use q2 only during push
- This guarantees pop() in O(1)

**Steps:**
- push(x):
  * Add x to q2
  * Move all elements from q1 → q2
  * Swap q1 and q2
- pop():
  * If q1 is empty → return -1
  * Else → remove from q1

**Java Code:**
```java
class StackUsingQueue {
    Queue<Integer> q1 = new LinkedList<>();
    Queue<Integer> q2 = new LinkedList<>();

    // Push operation (O(N))
    void push(int x) {
        q2.add(x);

        while (!q1.isEmpty()) {
            q2.add(q1.remove());
        }

        // swap queues
        Queue<Integer> temp = q1;
        q1 = q2;
        q2 = temp;
    }

    // Pop operation (O(1))
    int pop() {
        if (q1.isEmpty()) {
            return -1;
        }
        return q1.remove();
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack requires last pushed element first
- Queue removes from front
- By rotating elements during push:
  * The newest element is always placed at the front
- This makes pop() a direct queue removal

**Complexity (Time & Space):**
- Time: O(N) — push rearranges all existing elements
- Time: O(1) — pop is a single queue remove
- Space: O(N) — two queues storing elements

---

## 7. Justification / Proof of Optimality

- This approach strictly satisfies the problem requirement:
  * pop operation is guaranteed O(1)
  * Stack order (LIFO) is preserved using queue rearrangement
---

## 8. Variants / Follow-Ups

- Push O(1), Pop O(N) → delay rearrangement until pop
- Single queue solution → rotate queue after every push
---

## 9. Tips & Observations

- If problem says pop O(1) → rearrange during push
- Always swap queues after push
- Keep top element at front of q1
---

## 10. Pitfalls

- Forgetting to swap q1 and q2
- Using push O(1) logic by mistake
- Missing empty stack check
---

<!-- #endregion -->
