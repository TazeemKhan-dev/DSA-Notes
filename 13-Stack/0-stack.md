<!-- #region 0-STACK -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: STACK</h1>

## 1. Problem Understanding

- A Stack is a linear data structure that follows the principle:
- LIFO — Last In, First Out
- The last inserted element is the first to be removed
- All operations happen from one end only, called the TOP

- **Stack Terminology (MUST KNOW)**
- Top → points to the current top element
- Push → insert element at top
- Pop → remove element from top
- Peek / Top → view top element without removing
- Underflow → pop from empty stack
- Overflow → push into full stack (array stack)

- **Basic Stack Operations**
- push(x)
- pop()
- peek()
- isEmpty()
- isFull() (array implementation)

- **Why Stack Exists (Conceptual Use)**
- Stack is used when:
  * Order reversal is needed
  * Recent element matters first
  * Backtracking is required
  * Function calls must be tracked

- **Real-Life Examples**
- Stack of plates
- Browser back/forward
- Undo / Redo
- Function call stack (recursion)

- **Internal Working (Mental Model)**
- Stack grows in one direction
- Only the top element is accessible
- No random access like arrays
- Think of it as:

```text
|   |  ← top
| 5 |
| 3 |
| 1 |
-----

```

- **Types of Stack Implementation**
- 1️⃣ Stack using Array
  * Fixed size
  * Fast access
  * Risk of overflow
- 2️⃣ Stack using Linked List
  * Dynamic size
  * No overflow (memory permitting)
  * Slight overhead due to pointers
- 3️⃣ Stack using Java Library
  * Stack<T> (legacy)
  * Deque<T> (preferred)

- **Stack Overflow & Underflow**
- Overflow
  * Trying to push when stack is full
- Underflow
  * Trying to pop when stack is empty
- Always check before push/pop.

- **Stack in DSA Problems — Where It Appears**
- 🔹 Direct Stack Problems
  * Valid Parentheses
  * Min Stack
  * Implement Stack using Queue
- 🔹 Indirect (Hidden Stack)
  * Next Greater / Smaller Element
  * Largest Rectangle in Histogram
  * Expression Evaluation
  * Stock Span Problem

- **Monotonic Stack (VERY IMPORTANT)**
- A Monotonic Stack maintains elements in:
  * Increasing order OR
  * Decreasing order
- Used for:
  * Next Greater Element
  * Next Smaller Element
  * Histogram problems
- Key idea:
  * Pop until order is restored

- **Common Mistakes Beginners Make**
- Forgetting empty stack check
- Accessing non-top element
- Confusing stack with queue
- Using stack when sliding window is required
- Not understanding why pop happens in loops

- **What You MUST Be Clear About Before Solving Stack Questions**
- ✔ What is stored in stack?
- ✔ When to pop?
- ✔ What does each pop represent logically?
- ✔ Is it a monotonic stack problem?
- ✔ Is stack storing values or indices?

- **Key Interview Insight**
- Stack is rarely used alone.
- It is mostly used as a tool to maintain order or state.
---

<!-- #endregion -->
