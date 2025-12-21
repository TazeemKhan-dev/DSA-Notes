<!-- #region 203-Implement Stack using Queue-push O(1) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q203: Implement Stack using Queue-push O(1)</h1>

## 1. Problem Statement

- You are required to implement a Stack using two Queues.
- You need to support the following operations:
  * Type 1 x → Push the element x into the stack.
  * Type 2 → Pop the top element from the stack and print it.
    * If the stack is empty, print -1.
- Important Requirement
  * The push operation must run in O(1) time.
---

## 2. Problem Understanding

- A stack follows LIFO (Last In First Out) order, while a queue follows FIFO (First In First Out).
- Since push must be O(1), we:
  * Insert elements directly into a queue
  * Shift the cost to the pop operation
- We are allowed to use two queues only.
---

## 3. Constraints

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

### Approach 1: Push O(1), Costly Pop (Required)

**Idea:**
- Always push into Queue q1
- During pop:
  * Move size - 1 elements from q1 → q2
  * Pop the last remaining element (top of stack)
  * Swap q1 and q2
- This keeps push cheap and shifts cost to pop.

**Steps:**
- push(x):
  * Add x directly to q1
- pop():
  * If q1 is empty → return -1
  * Move elements from q1 to q2 until one element is left
  * Remove and store the last element (this is the stack top)
  * Swap q1 and q2
  * Return stored value

**Java Code:**
```java
class StackUsingQueue {
    Queue<Integer> q1 = new LinkedList<>();
    Queue<Integer> q2 = new LinkedList<>();

    // Push operation (O(1))
    void push(int x) {
        q1.add(x);
    }

    // Pop operation (O(N))
    int pop() {
        if (q1.isEmpty()) {
            return -1;
        }

        while (q1.size() > 1) {
            q2.add(q1.remove());
        }

        int top = q1.remove();

        // swap queues
        Queue<Integer> temp = q1;
        q1 = q2;
        q2 = temp;

        return top;
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack requires last pushed element first
- Queue removes from front
- By delaying the rearrangement until pop:
  * Push stays constant time
  * Pop extracts the “last inserted” element correctly

**Complexity (Time & Space):**
- Time: O(1) — push is a single queue add
- Time: O(N) — pop moves all elements except one
- Space: O(N) — two queues storing elements

---

## 7. Justification / Proof of Optimality

- This approach strictly satisfies the problem requirement:
  * Push is guaranteed O(1)
  * Stack order (LIFO) is preserved using queue rotations
---

## 8. Variants / Follow-Ups

- Pop O(1), Push O(N) → rotate during push
- Single queue solution → rotate queue after each push
---

## 9. Tips & Observations

- If problem says push O(1) → never rotate during push
- Always swap queues after pop
- Size check (q1.size() > 1) is critical
---

## 10. Pitfalls

- Forgetting to swap queues after pop
- Using pop O(1) logic by mistake
- Missing empty stack check
---

<!-- #endregion -->
