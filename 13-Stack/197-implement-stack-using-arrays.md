<!-- #region 197-Implement Stack using Arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q197: Implement Stack using Arrays</h1>

## 1. Problem Statement

- Implement a Last-In-First-Out (LIFO) stack using an array.
- The stack should support the following operations:
  * push(int x) → insert element
  * pop() → remove & return top element
  * top() → return top element without removing
  * isEmpty() → check if stack is empty
---

## 2. Problem Understanding

- Stack allows insertion and deletion from one end only (TOP)
- Follows LIFO (Last In First Out) principle
- Array is used as the underlying storage
- A variable top is used to track the current top index
---

## 3. Constraints

- 1 <= number of operations <= 100
- 1 <= x <= 100
---

## 4. Edge Cases

- pop() on empty stack
- top() on empty stack
- isEmpty() immediately after initialization
---

## 5. Examples

```text
Example 1

Input: ["ArrayStack", "push", "push", "top", "pop", "isEmpty"]

[[], [5], [10], [], [], []]

Output: [null, null, null, 10, 10, false]

Explanation:

ArrayStack stack = new ArrayStack();

stack.push(5);

stack.push(10);

stack.top(); // returns 10

stack.pop(); // returns 10

stack.isEmpty(); // returns false

Example 2

Input: ["ArrayStack","isEmpty", "push", "pop", "isEmpty"]

[[], [], [1], [], []]

Output: [null, true, null, 1, true]

Explanation: 

ArrayStack stack = new ArrayStack();

stack.push(1);

stack.pop(); // returns 1

stack.isEmpty(); // returns true
```

---

## 6. Approaches

### Approach 1: Stack Using Array (Optimal)

**Idea:**
- Use an integer array
- Maintain an index top
- Stack is empty when top == -1

**Steps:**
- Initialize top = -1
- For push(x):
  * Increment top
  * Store x at arr[top]
- For pop():
  * If empty, return -1
  * Return arr[top] and decrement top
- For top():
  * If empty, return -1
  * Return arr[top]
- For isEmpty():
  * Return true if top == -1

**Java Code:**
```java
class ArrayStack {
    int[] arr;
    int top;

    ArrayStack() {
        arr = new int[100];
        top = -1;
    }

    void push(int x) {
        arr[++top] = x;
    }

    int pop() {
        if (top == -1) return -1;
        return arr[top--];
    }

    int top() {
        if (top == -1) return -1;
        return arr[top];
    }

    boolean isEmpty() {
        return top == -1;
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack operations occur only at one end
- Array allows direct access using index
- top always points to the most recently inserted element

**Complexity (Time & Space):**
- Time: O(1) — direct index access
- Space: O(N) — array storage

---

## 7. Justification / Proof of Optimality

- Array-based stack is the simplest and most efficient way to implement stack operations with constant-time complexity.
---

## 8. Variants / Follow-Ups

- Dynamic stack (resizable array)
- Stack using Linked List
- Stack using Queue
- Min Stack
- Multiple stacks in one array
---

## 9. Tips & Observations

- Always initialize top to -1
- top++ happens before insertion
- Stack does not support random access
- Overflow handling may be skipped unless specified
---

## 10. Pitfalls

- Forgetting empty check before pop/top
- Confusing top() with pop()
- Incorrect top initialization
---

<!-- #endregion -->
