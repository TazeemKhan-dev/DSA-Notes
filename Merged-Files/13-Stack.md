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
<!-- #region 1-Infix, Prefix & Postfix — Evaluation & Conversions -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q1: Infix, Prefix & Postfix — Evaluation & Conversions</h1>

## 1. Problem Statement

- Expressions can be written in 3 forms:
  * Infix: a + b
  * Postfix: ab+
  * Prefix: +ab
- Stacks help:
- Operands → value / string stack
- Operators → operator stack

- **Helper Rules (IMPORTANT)**
- Operator Precedence
  * +  -  → 1
  * *  /  → 2
- Digit vs Alphabet
  * Character.isDigit(ch)      // '1'...'9'
  * Character.isLetter(ch)     // 'a'...'z'
- 🧪 Operator Utility Methods (Used Everywhere)

```text
static int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

static int apply(int a, int b, char op) {
    if (op == '+') return a + b;
    if (op == '-') return a - b;
    if (op == '*') return a * b;
    return a / b;
}

```
---

## 2. Approaches

### Approach 1: Infix → Postfix

**Idea:**
- Operators wait until higher precedence operators are processed
- Parentheses control scope

**Java Code:**
```java
static String infixToPostfix(String exp) {
    Stack<Character> ops = new Stack<>();
    StringBuilder res = new StringBuilder();

    for (char ch : exp.toCharArray()) {
        if (Character.isLetterOrDigit(ch)) {
            res.append(ch);
        } 
        else if (ch == '(') {
            ops.push(ch);
        } 
        else if (ch == ')') {
            while (ops.peek() != '(')
                res.append(ops.pop());
            ops.pop();
        } 
        else {
            while (!ops.isEmpty() && precedence(ops.peek()) >= precedence(ch))
                res.append(ops.pop());
            ops.push(ch);
        }
    }

    while (!ops.isEmpty())
        res.append(ops.pop());

    return res.toString();
}
```

### Approach 2: Infix → Prefix

**Idea:**
- Reverse expression
- Swap ( and )
- Convert to postfix
- Reverse result

**Java Code:**
```java
static String infixToPrefix(String exp) {
    StringBuilder rev = new StringBuilder(exp).reverse();
    for (int i = 0; i < rev.length(); i++) {
        if (rev.charAt(i) == '(') rev.setCharAt(i, ')');
        else if (rev.charAt(i) == ')') rev.setCharAt(i, '(');
    }

    String postfix = infixToPostfix(rev.toString());
    return new StringBuilder(postfix).reverse().toString();
}
```

### Approach 3: Infix Evaluation (Digits Only)

**Java Code:**
```java
static int evaluateInfix(String exp) {
    Stack<Integer> val = new Stack<>();
    Stack<Character> ops = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isDigit(ch)) {
            val.push(ch - '0');
        } 
        else if (ch == '(') {
            ops.push(ch);
        } 
        else if (ch == ')') {
            while (ops.peek() != '(') {
                int b = val.pop(), a = val.pop();
                val.push(apply(a, b, ops.pop()));
            }
            ops.pop();
        } 
        else {
            while (!ops.isEmpty() && precedence(ops.peek()) >= precedence(ch)) {
                int b = val.pop(), a = val.pop();
                val.push(apply(a, b, ops.pop()));
            }
            ops.push(ch);
        }
    }

    while (!ops.isEmpty()) {
        int b = val.pop(), a = val.pop();
        val.push(apply(a, b, ops.pop()));
    }

    return val.peek();
}
```

### Approach 4: Postfix → Infix

**Java Code:**
```java
static String postfixToInfix(String exp) {
    Stack<String> st = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String b = st.pop();
            String a = st.pop();
            st.push("(" + a + ch + b + ")");
        }
    }
    return st.peek();
}
```

### Approach 5: Postfix → Prefix

**Java Code:**
```java
static String postfixToPrefix(String exp) {
    Stack<String> st = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String b = st.pop();
            String a = st.pop();
            st.push(ch + a + b);
        }
    }
    return st.peek();
}
```

### Approach 6: Postfix Evaluation (Digits)

**Java Code:**
```java
static int evaluatePostfix(String exp) {
    Stack<Integer> st = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isDigit(ch)) {
            st.push(ch - '0');
        } else {
            int b = st.pop();
            int a = st.pop();
            st.push(apply(a, b, ch));
        }
    }
    return st.peek();
}
```

### Approach 7: Prefix → Infix

**Java Code:**
```java
static String prefixToInfix(String exp) {
    Stack<String> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String a = st.pop();
            String b = st.pop();
            st.push("(" + a + ch + b + ")");
        }
    }
    return st.peek();
}
```

### Approach 8: Prefix → Postfix

**Java Code:**
```java
static String prefixToPostfix(String exp) {
    Stack<String> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String a = st.pop();
            String b = st.pop();
            st.push(a + b + ch);
        }
    }
    return st.peek();
}
```

### Approach 9: Prefix Evaluation (Digits)

**Java Code:**
```java
static int evaluatePrefix(String exp) {
    Stack<Integer> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);
        if (Character.isDigit(ch)) {
            st.push(ch - '0');
        } else {
            int a = st.pop();
            int b = st.pop();
            st.push(apply(a, b, ch));
        }
    }
    return st.peek();
}
```

---

## 3. Tips & Observations

- Conversion = stack of strings
- Evaluation = stack of integers
- Postfix → left to right
- Prefix → right to left
- Infix → operator precedence matters
- Alphabet → conversion only
- Digits → evaluation only
---

<!-- #endregion -->
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
<!-- #region 201-Implement Stack Using Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q201: Implement Stack Using Linked List</h1>

## 1. Problem Statement

- Implement a Last-In-First-Out (LIFO) stack using a singly linked list.
- The stack must support the following operations:
  * push(int x) → push element onto the stack
  * pop() → remove and return the top element
  * top() → return the top element without removing it
  * isEmpty() → check if the stack is empty
---

## 2. Problem Understanding

- Stack follows LIFO order
- Only the top element is accessible
- Linked list allows dynamic memory allocation
- Stack top is represented by the head of the linked list
- Insertions and deletions are done at the head for O(1) time
---

## 3. Constraints

- 1 <= number of calls <= 100
- 1 <= x <= 100
- Singly linked list only
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

["LinkedListStack", "push", "push", "pop", "top", "isEmpty"]
[[], [3], [7], [], [], []]


Output:

[null, null, null, 7, 3, false]

Example 2

Input:

["LinkedListStack", "isEmpty"]
[[]]


Output:

[null, true]

Example 3

Input:

["LinkedListStack", "push", "pop", "isEmpty"]
[[], [2], [], []]


Output:

[null, null, 2, true]
```

---

## 6. Approaches

### Approach 1: Stack Using Singly Linked List (Optimal)

**Idea:**
- Use a linked list where:
  * Head node = top of stack
- Push and pop operations are done at the head
- This guarantees constant time operations

**Steps:**
- Maintain a pointer head pointing to top
- push(x):
  * Create new node
  * Point new node’s next to current head
  * Update head
- pop():
  * If empty, return -1
  * Store head.val
  * Move head to head.next
- top():
  * If empty, return -1
  * Return head.val
- isEmpty():
  * Return true if head == null

**Java Code:**
```java
class LinkedListStack {

    private class Node {
        int val;
        Node next;
        Node(int v) {
            val = v;
            next = null;
        }
    }

    Node head = null;

    void push(int x) {
        Node n = new Node(x);
        n.next = head;
        head = n;
    }

    int pop() {
        if (head == null) return -1;
        int res = head.val;
        head = head.next;
        return res;
    }

    int top() {
        if (head == null) return -1;
        return head.val;
    }

    boolean isEmpty() {
        return head == null;
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack operations require access to only one end
- Linked list head provides instant access
- No shifting of elements is needed
- Dynamic memory avoids overflow issues of arrays

**Complexity (Time & Space):**
- Time: O(1) — push, pop, top operate at head
- Space: O(N) — one node per element

---

## 7. Justification / Proof of Optimality

- Using a linked list allows implementing a stack with constant-time operations and dynamic size, making it more flexible than array-based stacks.
---

## 8. Variants / Follow-Ups

- Stack using array
- Stack using two queues
- Stack using single queue
- Min Stack using linked list + auxiliary stack
- Stack using doubly linked list
---

## 9. Tips & Observations

- Head insertion is key for O(1) push
- No need to track size unless asked
- Linked list avoids overflow but uses extra memory
- Prefer linked list when size is unknown
---

## 10. Pitfalls

- Pushing at tail instead of head (becomes O(N))
- Forgetting empty check before pop/top
- Losing reference to next node during pop
- Confusing stack top with tail of list
---

<!-- #endregion -->
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
<!-- #region 205-Implement two Stacks in an Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q205: Implement two Stacks in an Array</h1>

## 1. Problem Statement

- You are required to implement two stacks using a single array in a space-efficient manner.
- You need to support the following operations:
  * pop1() → Pop an element from Stack 1 and print it
  * push1(x) → Push element x into Stack 1
  * pop2() → Pop an element from Stack 2 and print it
  * push2(x) → Push element x into Stack 2
- Rules
  * Both stacks must use the same array
  * Print -1 in case of overflow or underflow
---

## 2. Problem Understanding

- We need two independent stacks
- We are allowed only one array
- Space must be used efficiently
- Naive approach (splitting array in half) wastes space.
- Instead, we let:
  * Stack 1 grow from the left
  * Stack 2 grow from the right
---

## 3. Constraints

- 1 <= n <= 500
- Stack operations must be O(1)
- No extra arrays allowed
---

## 4. Edge Cases

- Pop from empty stack → print -1
- Push when array is full → print -1
- Interleaved operations on both stacks
---

## 5. Examples

```text
Input

5
1
4 5156
2 989
3
1


Output

-1
5156
989
```

---

## 6. Approaches

### Approach 1: Two Pointers (Optimal)

**Idea:**
- Use a single array arr[]
- Maintain two pointers:
  * top1 = -1 → Stack 1 (left to right)
  * top2 = size → Stack 2 (right to left)
- Stack overflow occurs when top1 + 1 == top2

**Steps:**
- push1(x):
  * If top1 + 1 == top2 → overflow → print -1
  * Else → arr[++top1] = x
- push2(x):
  * If top1 + 1 == top2 → overflow → print -1
  * Else → arr[--top2] = x
- pop1():
  * If top1 == -1 → underflow → print -1
  * Else → print arr[top1--]
- pop2():
  * If top2 == size → underflow → print -1
  * Else → print arr[top2++]

**Java Code:**
```java
class TwoStacks {
    int[] arr;
    int size;
    int top1, top2;

    TwoStacks(int n) {
        size = n;
        arr = new int[n];
        top1 = -1;
        top2 = n;
    }

    void push1(int x) {
        if (top1 + 1 == top2) {
            System.out.println(-1);
            return;
        }
        arr[++top1] = x;
    }

    void push2(int x) {
        if (top1 + 1 == top2) {
            System.out.println(-1);
            return;
        }
        arr[--top2] = x;
    }

    void pop1() {
        if (top1 == -1) {
            System.out.println(-1);
            return;
        }
        System.out.println(arr[top1--]);
    }

    void pop2() {
        if (top2 == size) {
            System.out.println(-1);
            return;
        }
        System.out.println(arr[top2++]);
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack 1 grows left → right
- Stack 2 grows right → left
- Both stacks expand towards the center
- Space is fully utilized with zero wastage

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * push1 / push2: O(1) — pointer movement only
  * pop1 / pop2: O(1) — direct access
- 💾 Space Complexity
  * O(N) — single shared array
  * No extra space used

---

## 7. Justification / Proof of Optimality

- This is the most space-efficient solution:
  * Uses one array
  * No wasted space
  * All operations are constant time
---

## 8. Variants / Follow-Ups

- Fixed half-array stacks (wastes space)
- Dynamic resizing (not allowed here)
---

## 9. Tips & Observations

- Overflow condition is shared
- top1 + 1 == top2 is the key check
- This is a classic interview problem
---

## 10. Pitfalls

- Forgetting shared overflow condition
- Confusing stack directions
- Off-by-one pointer errors
---

<!-- #endregion -->
<!-- #region 207-Balanced Brackets -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q206: Balanced Brackets</h1>

## 1. Problem Statement

- You are given a string s of length n consisting only of the characters
  * '(' , ')' , '{' , '}' , '[' , ']'.
- Your task is to determine whether the given string is balanced.
- A string is said to be balanced if:
  * Every opening bracket has a corresponding closing bracket of the same type
  * Brackets are closed in the correct order
- Input Format
  * Line 1: An integer n — length of the string
  * Line 2: A string s
- Output Format
  * Print "YES" if brackets are balanced
  * Print "NO" otherwise
- Constraints
  * 1 <= n <= 10^6
  * s contains only '()[]{}'
---

## 2. Problem Understanding

- We must validate pairing + order of brackets.
- Order matters more than count.
- Nested and sequential brackets must both work.
- Any mismatch → invalid immediately.
---

## 3. Constraints

- Large input → must be O(N)
- Stack solution fits perfectly.
---

## 4. Edge Cases

- Closing bracket appears when stack is empty → NO
- Stack not empty after traversal → NO
- Single character string → NO
- Wrong type closure like (] → NO
- Wrong order like ([)] → NO
---

## 5. Examples

```text
Input
()
Output
YES

Input
(]
Output
NO

Input
{[()]}
Output
YES

Input
([)]
Output
NO
```

---

## 6. Approaches

### Approach 1: Stack (Optimal)

**Idea:**
- Use stack to store opening brackets.
- Closing bracket must match top of stack.

**Steps:**
- Steps
- Create empty stack
- Traverse string:
  * If opening bracket → push
  * If closing bracket:
    * stack empty → NO
    * pop and check matching
- After traversal:
  * stack empty → YES
  * else → NO

**Java Code:**
```java
public String isBalanced(String s) {
    Stack<Character> st = new Stack<>();

    for (char ch : s.toCharArray()) {
        if (ch == '(' || ch == '{' || ch == '[') {
            st.push(ch);
        } else {
            if (st.isEmpty()) return "NO";

            char top = st.pop();
            if ((ch == ')' && top != '(') ||
                (ch == '}' && top != '{') ||
                (ch == ']' && top != '[')) {
                return "NO";
            }
        }
    }
    return st.isEmpty() ? "YES" : "NO";
}
```

**💭 Intuition Behind the Approach:**
- Stack follows LIFO, perfect for nested structures.
- Latest opening bracket must be closed first.
- Any violation breaks balance.

**Complexity (Time & Space):**
- Time: O(N) — one pass
- Space: O(N) — stack in worst case

---

## 7. Justification / Proof of Optimality

- Stack ensures correct order and type matching.
- Immediate failure on mismatch.
- Optimal and scalable.
---

## 8. Variants / Follow-Ups

- Valid Parentheses
- Remove Invalid Parentheses
- Minimum Add to Make Parentheses Valid
- Score of Parentheses
---

## 9. Tips & Observations

- Always check stack before pop
- Store only opening brackets
- Matching logic must be strict
---

## 10. Pitfalls

- Ignoring order
- Only counting brackets
- Forgetting final stack empty check
- Using wrong matching conditions
---

<!-- #endregion -->
<!-- #region 209-Balanced Expression -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q207: Balanced Expression</h1>

## 1. Problem Statement

- You are given a string exp representing an expression.
- The expression may contain:
- Opening brackets: '(', '{', '['
- Closing brackets: ')', '}', ']'
- Operators: + - * /
- Lowercase English letters
- You need to determine whether the expression is balanced.
- A string is balanced if:
  * Every opening bracket is closed by the same type
  * Brackets are closed in the correct order
- Return true if the expression is balanced, otherwise return false.
---

## 2. Problem Understanding

- This is not about evaluating the expression
- We only care about bracket correctness
- Non-bracket characters must be ignored
- Order matters → last opened must be closed first (LIFO)
- This directly hints at using a stack.
---

## 3. Constraints

- 1 <= exp.length <= 10^4
- Expression contains valid ASCII characters only
---

## 4. Edge Cases

- Empty string → balanced
- Closing bracket before any opening → invalid
- Mismatched types: {], (} → invalid
- Extra opening brackets left at the end → invalid
- Expression with no brackets → balanced
---

## 5. Examples

```text
"[(a+b)+{(c+d)*(e/f)}]" → true

"[(a+b)+{(c+d)*(e/f)]}" → false

"[(a+b)+{(c+d)*(e/f)}" → false

"([(a+b)+{(c+d)*(e/f)}]" → false
```

---

## 6. Approaches

### Approach 1: Brute Force (Repeated Removal)

**Idea:**
- Repeatedly remove valid bracket pairs: (), {}, []
- If the string reduces to empty → balanced
- Else → invalid

**Steps:**
- Loop while the string changes
- Replace "()", "{}", "[]" with empty
- Stop when no more replacements are possible
- Check if string has any brackets left

**Java Code:**
```java
public static boolean expBalancedBrute(String s) {
    boolean changed = true;

    while (changed) {
        String prev = s;
        s = s.replace("()", "")
             .replace("{}", "")
             .replace("[]", "");
        changed = !s.equals(prev);
    }

    for (char c : s.toCharArray()) {
        if (c == '(' || c == '{' || c == '[' ||
            c == ')' || c == '}' || c == ']') {
            return false;
        }
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Balanced brackets always contain adjacent valid pairs
- Removing inner pairs slowly collapses the expression

**Complexity (Time & Space):**
- Time: O(N^2) — repeated string scans and replacements
- Space: O(N) — string rebuilding

### Approach 2: Stack-Based (Standard & Optimal)

**Idea:**
- Use a stack to store opening brackets
- When a closing bracket appears:
  * Stack must not be empty
  * Top must be the matching opening bracket

**Steps:**
- Traverse the expression
- Push opening brackets onto stack
- On closing bracket:
  * If stack empty → invalid
  * Pop and verify matching type
- Ignore non-bracket characters
- At end, stack must be empty

**Java Code:**
```java
public static boolean expBalanced(String str) {
    Stack<Character> s = new Stack<>();

    for (int i = 0; i < str.length(); i++) {
        char c = str.charAt(i);

        if (c == '(' || c == '{' || c == '[') {
            s.push(c);
        }
        else if (c == ')' || c == '}' || c == ']') {
            if (s.isEmpty()) return false;

            char top = s.pop();
            if (c == ')' && top != '(') return false;
            if (c == '}' && top != '{') return false;
            if (c == ']' && top != '[') return false;
        }
    }

    return s.isEmpty();
}
```

**💭 Intuition Behind the Approach:**
- Stack keeps track of pending openings
- Closing bracket must match the most recent opening
- This enforces correct order automatically

**Complexity (Time & Space):**
- Time: O(N) — single pass
- Space: O(N) — worst case all openings

### Approach 3: Stack + HashMap (Cleaner Matching)

**Idea:**
- Use a map to define valid bracket pairs
- Simplifies matching logic

**Steps:**
- Map closing → opening brackets
- Push opening brackets
- On closing:
  * Stack must not be empty
  * Top must match map value

**Java Code:**
```java
public static boolean expBalancedMap(String str) {
    Stack<Character> st = new Stack<>();
    Map<Character, Character> map = new HashMap<>();

    map.put(')', '(');
    map.put('}', '{');
    map.put(']', '[');

    for (char c : str.toCharArray()) {
        if (map.containsValue(c)) {
            st.push(c);
        }
        else if (map.containsKey(c)) {
            if (st.isEmpty() || st.pop() != map.get(c)) {
                return false;
            }
        }
    }

    return st.isEmpty();
}
```

**💭 Intuition Behind the Approach:**
- Encodes bracket rules in data instead of condition chains
- More scalable if bracket types increase

**Complexity (Time & Space):**
- Time: O(N) — single traversal
- Space: O(N) — stack + map

---

## 7. Justification / Proof of Optimality

- Brackets require order-sensitive matching
- Stack naturally enforces LIFO order
- Any solution not using stack implicitly simulates one
---

## 8. Variants / Follow-Ups

- Valid Parentheses (())
- Valid Parenthesis String (*)
- Remove Invalid Parentheses
- Minimum Add to Make Parentheses Valid
---

## 9. Tips & Observations

- Ignore non-bracket characters early
- Always check stack empty before popping
- If multiple bracket types exist → stack is mandatory
---

## 10. Pitfalls

- Popping without checking stack emptiness
- Forgetting to check leftover opening brackets
- Comparing wrong bracket types
- Treating operators/characters as brackets
---

<!-- #endregion -->
<!-- #region 204-Extra Brackets -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q208: Extra Brackets</h1>

## 1. Problem Statement

- You are given a string exp representing a balanced expression containing only round brackets '(' and ')' along with operands and operators.
- Some pairs of brackets may be extra / redundant (i.e., they do not affect the evaluation of the expression).
- You need to determine whether the given expression contains extra brackets.
- Return:
  * true → if extra brackets exist
  * false → otherwise
---

## 2. Problem Understanding

- The expression is already balanced
- Only round brackets ( and ) are present
- Extra brackets are those which:
  * Enclose no operator
  * Or enclose a single element unnecessarily
- We must detect redundant parentheses
---

## 3. Constraints

- 1 <= exp.length <= 10000
- Expression contains:
  * lowercase letters
  * operators (+ - * /)
  * round brackets only
---

## 4. Edge Cases

- Single pair of brackets around one variable → extra
- Nested brackets with no operator inside
- Expression with operators inside every bracket → not extra
- Deeply nested expressions
---

## 5. Examples

```text
Example 1

Input:

((a + b) + (c + d))


Output:

false

Example 2

Input:

(a + b) + ((c + d))


Output:

true
```

---

## 6. Approaches

### Approach 1: Stack-Based Check (Optimal)

**Idea:**
- Use a stack
- Push all characters except )
- When ) is found:
  * Pop elements until matching ( is found
  * Check if there was any operator between them
- If no operator is found → extra brackets exist

**Steps:**
- Initialize an empty stack
- Traverse the expression character by character
- If character is not ) → push to stack
- If character is ):
  * Pop elements until ( is found
  * Track if any operator (+ - * /) is found
  * If no operator found → return true
- After traversal → return false

**Java Code:**
```java
class Solution {
    public static boolean ExtraBrackets(String exp) {
        Stack<Character> st = new Stack<>();

        for (char ch : exp.toCharArray()) {

            if (ch != ')') {
                st.push(ch);
            } else {
                boolean hasOperator = false;

                while (!st.isEmpty() && st.peek() != '(') {
                    char top = st.pop();
                    if (top == '+' || top == '-' || top == '*' || top == '/') {
                        hasOperator = true;
                    }
                }

                st.pop(); // pop '('

                if (!hasOperator) {
                    return true; // extra brackets found
                }
            }
        }
        return false;
    }
}

// below is in-class solution 
// Structural Check for Extra Brackets
// ------------------------------------------------
// This solution detects extra brackets by checking for empty brackets "()".
// When a closing ')' is encountered:
// - If the stack top is immediately '(', it means there was nothing inside → extra brackets.
// - Otherwise, elements are popped until '(' is found.
//
// Note:
// This approach works because the expression is assumed to be valid.
// It checks only the presence of content inside brackets,
// not whether that content contains an operator.
// For semantic correctness (checking actual usefulness of brackets),
// refer to the operator-based solution below.

  public boolean ExtraBrackets(String exp) {
        // Write your code here
        Stack<Character> st=new Stack<>();
        for(int i=0;i<exp.length();i++){
            char c=exp.charAt(i);
            if(c==')'){
            if(!st.isEmpty() && st.peek()=='(' ) return true;
            while(!st.isEmpty() && st.peek()!='(' ) st.pop();
            st.pop();
            }else st.push(c);
        }
        return false;
       
    }
```

**💭 Intuition Behind the Approach:**
- Brackets are useful only if they group an operation
- If a pair of brackets contains no operator, they are redundant
- Stack helps process the innermost brackets first
- Early detection avoids unnecessary traversal

**Complexity (Time & Space):**
- Time: O(N) — single traversal of expression
- Space: O(N) — stack storage

---

## 7. Justification / Proof of Optimality

- The stack-based approach correctly identifies redundant brackets by ensuring that every bracket pair encloses at least one operator, fulfilling the problem’s requirement efficiently.
---

## 8. Variants / Follow-Ups

- Remove redundant parentheses
- Valid Parentheses
- Balanced Brackets
- Minimum brackets removal
- Expression evaluation
---

## 9. Tips & Observations

- Balanced expression does NOT mean non-redundant
- Operators are the key to identifying useful brackets
- Always process from innermost to outermost
- Only ) triggers evaluation
---

## 10. Pitfalls

- Forgetting to check operator presence
- Treating nested valid brackets as redundant
- Missing operator characters in check
- Assuming balanced means correct
---

<!-- #endregion -->
<!-- #region 205-Valid Parenthesis String -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q209: Valid Parenthesis String</h1>

## 1. Problem Statement

- Given a string s containing only three characters '(', ')', and '*', determine whether the string is valid.
- Rules for validity:
- Every '(' must have a matching ')'
- Every ')' must have a matching '('
- '(' must come before its matching ')'
- '*' can act as:
  * '('
  * ')'
  * empty string ""
- Return true if the string is valid, otherwise return false.
---

## 2. Problem Understanding

- We must validate balanced parentheses, but with a twist:
  * '*' is flexible
- Order matters → '(' must appear before ')'
- We must check existence of at least one valid interpretation
- This is not about counting exact matches, but about possibility
---

## 3. Constraints

- 1 <= s.length <= 100
- s[i] ∈ { '(', ')', '*' }
---

## 4. Edge Cases

- String with only '*' → valid
- More ')' than possible '(' at any point → invalid
- Unmatched '(' at end → invalid
- '*' before '(' cannot act as its closing ')'
---

## 5. Examples

```text
"()" → true

"(*)" → true

"(*))" → true

"())" → false

"*)(" → false
```

---

## 6. Approaches

### Approach 1: Stack-Based (Two Stacks)

**Idea:**
- Track positions of:
  * '(' in one stack
  * '*' in another stack
- When ')' appears  :
  * Prefer matching '('
  * Else use '*' as '('
- After traversal:
  * Match remaining '(' with '*' acting as ')'
  * Ensure '*' comes after '(' (index order)

**Steps:**
- Traverse the string
- Push indices of '(' and '*'
- On ')':
  * Pop '(' if possible
  * Else pop '*'
  * Else return false
- After traversal:
  * Match leftover '(' with '*' (order check)
- If unmatched '(' remains → invalid

**Java Code:**
```java
public static boolean checkValidString(String s) {
    Stack<Integer> open = new Stack<>();
    Stack<Integer> star = new Stack<>();

    for (int i = 0; i < s.length(); i++) {
        char ch = s.charAt(i);

        if (ch == '(') {
            open.push(i);
        } else if (ch == '*') {
            star.push(i);
        } else { // ')'
            if (!open.isEmpty()) {
                open.pop();
            } else if (!star.isEmpty()) {
                star.pop();
            } else {
                return false;
            }
        }
    }

    while (!open.isEmpty() && !star.isEmpty()) {
        if (open.peek() > star.peek()) return false;
        open.pop();
        star.pop();
    }

    return open.isEmpty();
}
```

**💭 Intuition Behind the Approach:**
- '(' and ')' need order-aware matching
- '*' acts as a buffer
- Index comparison enforces correct bracket order

**Complexity (Time & Space):**
- Time: O(N) — each character is pushed/popped once
- Space: O(N) — stacks store indices

### Approach 2: Greedy Range Method (Optimal)

**Idea:**
- Track a range of possible open brackets:
  * minOpen → minimum possible '('
  * maxOpen → maximum possible '('
- '*' expands the range
- If range becomes invalid → return false

**Steps:**
- Initialize minOpen = 0, maxOpen = 0
- Traverse characters:
  * '(' → both increase
  * ')' → both decrease
  * '*' → minOpen--, maxOpen++
- If maxOpen < 0 → invalid
- Clamp minOpen to 0
- At end, if minOpen == 0 → valid

**Java Code:**
```java
public static boolean checkValidString(String s) {
    int minOpen = 0, maxOpen = 0;

    for (char ch : s.toCharArray()) {
        if (ch == '(') {
            minOpen++;
            maxOpen++;
        } else if (ch == ')') {
            minOpen--;
            maxOpen--;
        } else { // '*'
            minOpen--;
            maxOpen++;
        }

        if (maxOpen < 0) return false;
        if (minOpen < 0) minOpen = 0;
    }

    return minOpen == 0;
}
```

**💭 Intuition Behind the Approach:**
- We don’t care about exact matching — only possibility
- Track best and worst cases simultaneously
- If even the best case fails → impossible

**Complexity (Time & Space):**
- Time: O(N) — single traversal
- Space: O(1) — no extra data structures

---

## 7. Justification / Proof of Optimality

- Stack approach guarantees correctness via explicit matching
- Greedy approach compresses all valid stack states into a range
- Both correctly validate existence of at least one valid interpretation
---

## 8. Variants / Follow-Ups

- Valid Parentheses ((, ))
- Balanced Brackets (()[]{})
- Remove Invalid Parentheses
- Minimum Add to Make Parentheses Valid
---

## 9. Tips & Observations

- If order matters, stacks are natural
- If existence of solution matters, greedy often works
- '*' is not fixed, treat it as a range contributor
---

## 10. Pitfalls

- Treating this as a normal parentheses problem
- Forgetting '*' can be empty
- Not enforcing index order in stack approach
- Assuming greedy means guessing — it’s bounded logic
---

<!-- #endregion -->
<!-- #region 208-Previous Greater element -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q210: Previous Greater element</h1>

## 1. Problem Statement

- You are given an array arr of n distinct integers.
- For each element in the array, find the previous greater element — the nearest element to the left that is greater than the current element.
- If no such element exists, return -1 for that position.
---

## 2. Problem Understanding

- For every index i, we must look only on the left side (0 to i-1)
- Among those, we want:
  * An element greater than arr[i]
  * The closest one (nearest to i)
- If nothing qualifies → answer is -1
- Order matters → this is not sorting, this is relative position
---

## 3. Constraints

- 1 <= n <= 10^5
- 1 <= arr[i] <= 10^6
- All elements are distinct
- Large n → brute force O(n^2) will TLE
---

## 4. Edge Cases

- First element → always -1
- Strictly increasing array → all -1
- Strictly decreasing array → previous element always answer
- Single element array
---

## 5. Examples

```text
Example 1

Input:

10 20 30 40


Output:

-1 -1 -1 -1

Example 2

Input:

40 30 20 10


Output:

-1 40 30 20
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- For each element, scan leftwards until you find a greater element.

**Steps:**
- For each index i
- Loop from i-1 to 0
- First element greater than arr[i] → answer
- If none found → -1

**Java Code:**
```java
public static long[] prevGreater(long[] arr, int n) {
    long[] ans = new long[n];

    for (int i = 0; i < n; i++) {
        ans[i] = -1;
        for (int j = i - 1; j >= 0; j--) {
            if (arr[j] > arr[i]) {
                ans[i] = arr[j];
                break;
            }
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We directly simulate the definition of “previous greater” by checking all left elements until the closest valid one is found.

**Complexity (Time & Space):**
- Time: O(n^2) — nested loops, worst case full scan for each element
- Space: O(1) — no extra data structures

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Use a monotonic decreasing stack to keep only useful candidates for previous greater.
- Key Observation
  * Smaller elements to the left are useless once a bigger element appears
  * Stack helps us discard useless elements efficiently

**Steps:**
- Traverse array from left to right
- Pop elements from stack while stack.top <= arr[i]
- After popping:
  * If stack empty → answer is -1
  * Else → stack top is previous greater
- Push arr[i] into stack

**Java Code:**
```java
public static long[] prevGreater(long[] arr, int n) {
    long[] ans = new long[n];
    Stack<Long> st = new Stack<>();

    for (int i = 0; i < n; i++) {

        while (!st.isEmpty() && st.peek() <= arr[i]) {
            st.pop();
        }

        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(arr[i]);
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We only keep elements that can possibly act as a previous greater for future elements.
- Once a bigger element arrives, all smaller ones before it become irrelevant.

**Complexity (Time & Space):**
- Time: O(n) — each element pushed & popped at most once
- Space: O(n) — stack in worst case

---

## 7. Justification / Proof of Optimality

- The stack approach is optimal because:
  * It avoids repeated left scans
  * Maintains only valid candidates
  * Guarantees nearest greater by construction
---

## 8. Variants / Follow-Ups

- Previous Smaller Element
- Next Greater Element
- Next Smaller Element
- Stock Span Problem
- Largest Rectangle in Histogram
---

## 9. Tips & Observations

- Previous → traverse left to right
- Next → traverse right to left
- Greater → pop smaller
- Smaller → pop greater
- Always ask: “What elements are useless going forward?”
---

## 10. Pitfalls

- Popping the answer element instead of using it
- Using < instead of <= (important with duplicates)
- Confusing Previous vs Next logic
- Using stack index when value stack is simpler
---

<!-- #endregion -->
<!-- #region 202-Next Greater Element -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q211: Next Greater Element</h1>

## 1. Problem Statement

- Given an array arr of size n consisting of non-zero positive integers, find the Next Greater Element (NGE) for each element.
- The Next Greater Element of an element is the nearest element to its right that is strictly greater than the current element.
- If no such element exists, return -1 for that position.
---

## 2. Problem Understanding

- For every index i, look only to the right side
- Find the first element greater than arr[i]
- If none exists → answer is -1
- Order of output must match input order
- Brute force is too slow → need O(N) solution
---

## 3. Constraints

- 1 <= n <= 10^6
- 1 <= arr[i] <= 10^18
- Expected Time: O(N)
- Expected Space: O(N)
---

## 4. Edge Cases

- Single element array → answer is -1
- Strictly decreasing array → all -1
- Strictly increasing array → next element for all except last
- Very large values (use long)
- Duplicate values (strictly greater required)
---

## 5. Examples

```text
Example 1

Input:

4
1 3 2 4


Output:

3 4 4 -1

Example 2

Input:

5
6 8 0 1 3


Output:

8 -1 1 3 -1
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- For each element, scan all elements to its right and find the first greater one.

**Steps:**
- For each index i
- Traverse from i+1 to n-1
- Stop when a greater element is found

**Complexity (Time & Space):**
- Time: O(N^2) — nested loops
- Space: O(1) — no extra space
- Not acceptable due to constraints

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Use a monotonic decreasing stack
- Stack stores indices (not values)
- When a greater element appears, it resolves NGE for smaller elements

**Steps:**
- Initialize an empty stack
- Create result array ans of size n
- Traverse array from left to right
- While stack not empty AND
  * arr[current] > arr[stack.top()]:
    * Pop index from stack
    * Set its NGE as arr[current]
- Push current index onto stack
- After traversal, remaining indices in stack → -1

**Java Code:**
```java
import java.util.*;

class Solution {
    public static long[] nextGreaterElement(long[] arr, int n) {
        long[] ans = new long[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && arr[i] > arr[st.peek()]) {
                ans[st.pop()] = arr[i];
            }
            st.push(i);
        }

        while (!st.isEmpty()) {
            ans[st.pop()] = -1;
        }

        return ans;
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack keeps track of elements waiting for a greater element
- When a larger value arrives, it satisfies all smaller values before it
- Each element is pushed and popped only once
- This ensures linear time complexity

**Complexity (Time & Space):**
- Time: O(N) — each element pushed & popped once
- Space: O(N) — stack + result array

---
### Approach 3: Reverse Traversal (Right → Left)

**Idea:**
- Instead of waiting for a future element to resolve answers, we pre-load future elements by traversing from right to left.
- At each index:
  * The stack already contains elements to the right
  * The nearest greater element will be on top after cleanup
- This approach is the mirror image of Previous Greater Element.

**Steps:**
- Traverse the array from n-1 to 0
- Pop elements from stack while stack.top <= arr[i]
- After popping:
  * If stack is empty → ans[i] = -1
  * Else → ans[i] = stack.top
- Push arr[i] into stack

**Java Code:**
```java
public static long[] nextGreater(long[] arr, int n) {
    long[] ans = new long[n];
    Stack<Long> st = new Stack<>();

    for (int i = n - 1; i >= 0; i--) {

        while (!st.isEmpty() && st.peek() <= arr[i]) {
            st.pop();
        }

        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(arr[i]);
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- When scanning from right to left:
  * The stack always represents future elements
  * Smaller future elements are useless and removed
  * The top of the stack becomes the closest greater element on the right
- No element is left “pending”, so no cleanup loop is required.

**Complexity (Time & Space):**
- Time: O(n) — each element is pushed and popped at most once
- Space: O(n) — stack in the worst case (strictly decreasing array)

---


## 7. Justification / Proof of Optimality

- The monotonic stack approach efficiently finds the next greater element in a single traversal, meeting the required O(N) time complexity even for large inputs.
---

## 8. Variants / Follow-Ups

- Next Smaller Element
- Previous Greater Element
- Previous Smaller Element
- Circular Next Greater Element
- Stock Span Problem
- Daily Temperatures
---

## 9. Tips & Observations

- Use stack of indices, not values
- Stack remains monotonically decreasing
- For circular NGE → traverse array twice
- Always clarify strictly greater vs greater or equal
---

## 10. Pitfalls

- Using brute force for large n
- Storing values instead of indices
- Forgetting to assign -1 to remaining stack elements
- Confusing NGE with max to the right
---

<!-- #endregion -->
<!-- #region 206-Stock Span Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q212: Stock Span Problem</h1>

## 1. Problem Statement

- You are given stock prices for n consecutive days.
- For each day i, you must calculate how many consecutive days (ending at i, including i) the stock price was less than or equal to the price on day i.
- The span stops as soon as you encounter a strictly greater price to the left.
- In simple words:
  * Span = distance to the nearest greater element on the left, or the entire prefix if none exists.
---

## 2. Problem Understanding

- 1 <= n <= 100000
- 1 <= arr[i] <= 100000
- Large input → brute force will TLE
---

## 3. Edge Cases

- n = 1 → span is always 1
- Strictly increasing array → spans increase as 1,2,3,...
- Strictly decreasing array → all spans are 1
- Equal elements → span continues (<= allowed)
---

## 4. Examples

```text
Input
[100, 80, 60, 70, 60, 75, 85]

Output
[1, 1, 1, 2, 1, 4, 6]
```

---

## 5. Approaches

### Approach 1: Brute Force (Check Left for Every Day)

**Idea:**
- For each day i, move left while prices are <= arr[i] and count days.

**Steps:**
- For each index i
- Start from i-1 and move left
- Stop when you find a price > arr[i]
- Count days including current

**Java Code:**
```java
public int[] stockSpan(int[] arr) {
    int n = arr.length;
    int[] span = new int[n];

    for (int i = 0; i < n; i++) {
        int count = 1;
        int j = i - 1;
        while (j >= 0 && arr[j] <= arr[i]) {
            count++;
            j--;
        }
        span[i] = count;
    }
    return span;
}
```

**💭 Intuition Behind the Approach:**
- We directly simulate the definition of span by checking previous days one by one.

**Complexity (Time & Space):**
- Time: O(N^2) — for each day, worst case scans all previous days
- Space: O(1) — only output array

### Approach 2: Stack (Nearest Greater to Left) — Optimal

**Idea:**
- Span depends on nearest greater element on the left
- Use a monotonic decreasing stack storing indices

**Steps:**
- Create an empty stack (store indices)
- Traverse from left to right
- While stack top price <= current price, pop
- If stack empty → span = i + 1
- Else → span = i - stack.peek()
- Push current index

**Java Code:**
```java
public int[] stockSpan(int[] arr) {
    int n = arr.length;
    int[] span = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && arr[st.peek()] <= arr[i]) {
            st.pop();
        }

        if (st.isEmpty()) {
            span[i] = i + 1;
        } else {
            span[i] = i - st.peek();
        }

        st.push(i);
    }
    return span;
}
```

**💭 Intuition Behind the Approach:**
- Stack keeps only useful candidates that can block the span
- Once a smaller element is popped, it is never needed again
- Each element is pushed and popped at most once

**Complexity (Time & Space):**
- Time: O(N) — amortized, each element pushed and popped once
- Space: O(N) — stack in worst case (strictly decreasing array)

---

## 6. Justification / Proof of Optimality

- Brute force is intuitive but inefficient
- Stack approach efficiently tracks the nearest greater element
- This is a classic monotonic stack problem
---

## 7. Variants / Follow-Ups

- Nearest Greater Element to Left
- Daily Temperatures
- Largest Rectangle in Histogram
- Online Stock Span (streaming version)
---

## 8. Tips & Observations

- Span is NOT sliding window
- Condition is <=, not <
- Stack stores indices, not values
- Empty stack means full span till start
---

## 9. Pitfalls

- Using < instead of <=
- Forgetting to include the current day
- Storing values instead of indices
- Trying nested loops for large n
---

<!-- #endregion -->
<!-- #region 203-Largest Histogram Area -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q213: Largest Histogram Area</h1>

## 1. Problem Statement

- You are given a histogram consisting of N bars, where the height of each bar is given in array A.
- Each bar has a width of 1.
- Your task is to find the largest rectangular area that can be formed using contiguous bars in the histogram.
---

## 2. Problem Understanding

- Each bar can extend left and right until a smaller bar blocks it
- For every bar i, we want:
  * the first smaller bar on the left
  * the first smaller bar on the right
- The bar A[i] becomes the minimum height of the rectangle
- Width = (rightSmallerIndex - leftSmallerIndex - 1)
- Area = height * width
- We need the maximum area among all bars
---

## 3. Constraints

- 1 <= N <= 10000
- 1 <= A[i] <= 10000
- Expected Time Complexity: O(N)
- Expected Space Complexity: O(N)
---

## 4. Edge Cases

- Single bar → area = height
- All bars same height
- Strictly increasing heights
- Strictly decreasing heights
- Minimum height bars spanning full width
---

## 5. Examples

```text
Example 1

Input:

4
1 2 3 1


Output:

4

Example 2

Input:

3
2 2 2


Output:

6
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- For each bar, expand left and right until a smaller bar is found.

**Complexity (Time & Space):**
- Time: O(N^2) — expansion for each bar
- Space: O(1) — no extra space
- ❌ Not acceptable for given constraints

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Use a monotonic increasing stack
- Stack stores indices
- When a smaller bar appears, it finalizes rectangles for taller bars
- This allows computing area in one traversal

**Steps:**
- Initialize an empty stack
- Traverse bars from left to right
- While stack not empty AND
  * A[current] < A[stack.top()]:
      * Pop index h
      * Height = A[h]
      * Right boundary = current index
      * Left boundary = stack.top() (after pop) or -1
      * Width = right - left - 1
      * Update max area
- Push current index
- After traversal, pop remaining bars using N as right boundary

**Java Code:**
```java
class Solution {
    public static int largestRectangleArea(int[] A, int n) {
        Stack<Integer> st = new Stack<>();
        int maxArea = 0;

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && A[i] < A[st.peek()]) {
                int h = A[st.pop()];
                int right = i;
                int left = st.isEmpty() ? -1 : st.peek();
                int width = right - left - 1;
                maxArea = Math.max(maxArea, h * width);
            }
            st.push(i);
        }

        while (!st.isEmpty()) {
            int h = A[st.pop()];
            int right = n;
            int left = st.isEmpty() ? -1 : st.peek();
            int width = right - left - 1;
            maxArea = Math.max(maxArea, h * width);
        }

        return maxArea;
    }
}


// In class solution its slow compare to the above 1 ,

class Solution {

    public static long maximumArea(long hist[], long n) {

        long[] left = nsl(hist, (int)n);
        long[] right = nsr(hist, (int)n);

        long maxArea = 0;

        for (int i = 0; i < n; i++) {
            long width = right[i] - left[i] - 1;
            long area = hist[i] * width;
            maxArea = Math.max(maxArea, area);
        }

        return maxArea;
    }

    // Nearest Smaller to Left
    private static long[] nsl(long[] hist, int n) {
        long[] left = new long[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && hist[st.peek()] >= hist[i]) {
                st.pop();
            }
            left[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }
        return left;
    }

    // Nearest Smaller to Right
    private static long[] nsr(long[] hist, int n) {
        long[] right = new long[n];
        Stack<Integer> st = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && hist[st.peek()] >= hist[i]) {
                st.pop();
            }
            right[i] = st.isEmpty() ? n : st.peek();
            st.push(i);
        }
        return right;
    }
}
```

**💭 Intuition Behind the Approach:**
- Each bar is treated as the smallest bar in a rectangle
- Stack ensures bars are processed only when their right boundary is known
- Popping means:
  * “I have found the maximum width where this bar is the smallest”
- Each index is pushed and popped once

**Complexity (Time & Space):**
- Time: O(N) — each index pushed and popped once
- Space: O(N) — stack usage

---

## 7. Justification / Proof of Optimality

- The monotonic stack efficiently computes the largest rectangle by determining the maximum width for each bar where it acts as the minimum height, achieving optimal linear time complexity.
---

## 8. Variants / Follow-Ups

- Maximum rectangle in binary matrix
- Largest rectangle in skyline
- Trapping Rain Water
- Next Smaller Element
- Maximum area under histogram (circular)
---

## 9. Tips & Observations

- Always store indices, not heights
- Use < not <= to avoid incorrect width
- Right boundary is known only when a smaller bar appears
- Remaining stack elements extend till end of array
---

## 10. Pitfalls

- Forgetting to process remaining stack
- Incorrect width calculation
- Using values instead of indices
- Confusing NGE logic with histogram logic
---

<!-- #endregion -->
<!-- #region 210-Reverse Integer -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q214: Reverse Integer</h1>

## 1. Problem Statement

- Given a signed 32-bit integer x, return x with its digits reversed.
- If reversing x causes the value to go outside the signed 32-bit integer range
- [-2^31, 2^31 - 1], return 0.
- Important constraints:
  * You are not allowed to use 64-bit integers (long)
  * Must handle negative numbers
  * Must detect overflow
---

## 2. Problem Understanding

- We need to reverse the digits of an integer
- Sign (+ / -) must be preserved
- Leading zeros in the reversed number should be removed automatically
- Overflow must be detected before it happens
- String conversion is allowed logically but not optimal
- Key difficulty:
  * Detecting overflow during number construction, not after
---

## 3. Constraints

- -2^31 <= x <= 2^31 - 1
- Only 32-bit integer operations allowed
---

## 4. Edge Cases

- x = 0 → 0
- x = 120 → 21
- x = -120 → -21
- x = 1534236469 → overflow → 0
- x = -2147483648 → overflow → 0
---

## 5. Examples

```text
Input:  321
Output: 123

Input:  -321
Output: -123

Input:  120
Output: 21
```

---

## 6. Approaches

### Approach 1: Brute Force (String Conversion)

**Idea:**
- Convert number to string
- Reverse characters
- Parse back to integer
- Catch overflow

**Steps:**
- Convert integer to string
- Reverse digits
- Convert back to integer
- If overflow occurs → return 0

**Java Code:**
```java
public int reverseInteger(int x) {
    boolean neg = x < 0;
    String s = Integer.toString(Math.abs(x));
    StringBuilder sb = new StringBuilder(s).reverse();

    try {
        int val = Integer.parseInt(sb.toString());
        return neg ? -val : val;
    } catch (Exception e) {
        return 0;
    }
}
```

**💭 Intuition Behind the Approach:**
- Simple and readable
- Relies on language parsing to detect overflow

**Complexity (Time & Space):**
- Time: O(N) — digits count
- Space: O(N) — string storage
- ❌ Not interview-preferred

### Approach 2: Stack-Based (Practice-Oriented)

**Idea:**
- Extract digits using % 10
- Push digits into stack
- Pop and rebuild reversed number
- Check overflow while rebuilding

**Steps:**
- Extract digits and push into stack
- Pop digits and rebuild number
- Check overflow before multiplication

**Java Code:**
```java
public int reverseInteger(int x) {
    Stack<Integer> st = new Stack<>();
    int num = Math.abs(x);

    while (num > 0) {
        st.push(num % 10);
        num /= 10;
    }

    int rev = 0;
    while (!st.isEmpty()) {
        int d = st.pop();

        if (rev > Integer.MAX_VALUE / 10 ||
           (rev == Integer.MAX_VALUE / 10 && d > 7)) return 0;

        rev = rev * 10 + d;
    }

    return x < 0 ? -rev : rev;
}
```

**💭 Intuition Behind the Approach:**
- Stack reverses order naturally (LIFO)
- Used mainly for stack practice, not optimization

**Complexity (Time & Space):**
- Time: O(N) — digits
- Space: O(N) — stack
- ⚠️ Valid but not optimal

### Approach 3: al (Arithmetic Only — Interview Preferred)

**Idea:**
- Reverse digits using arithmetic only
- Detect overflow before multiplying by 10
- No extra data structures

**Steps:**
- Extract digit using % 10
- Shrink number using / 10
- Check overflow before rev * 10
- Append digit
- Repeat until number becomes 0

**Java Code:**
```java
public int reverseInteger(int x) {
    int rev = 0;

    while (x != 0) {
        int digit = x % 10;
        x /= 10;

        if (rev > Integer.MAX_VALUE / 10 ||
           (rev == Integer.MAX_VALUE / 10 && digit > 7)) return 0;

        if (rev < Integer.MIN_VALUE / 10 ||
           (rev == Integer.MIN_VALUE / 10 && digit < -8)) return 0;

        rev = rev * 10 + digit;
    }

    return rev;
}
```

**💭 Intuition Behind the Approach:**
- % 10 gives digits in reverse order naturally
- Overflow must be predicted before it happens
- Uses constant extra space

**Complexity (Time & Space):**
- Time: O(N) — digits
- Space: O(1) — no extra space

---

## 7. Justification / Proof of Optimality

- Arithmetic solution is optimal in space and clarity
- Stack and string solutions are valid but inferior
- Overflow check is the key challenge, not reversal
---

## 8. Variants / Follow-Ups

- Reverse digits in base k
- Reverse only even digits
- Palindrome number
- Add two reversed numbers
---

## 9. Tips & Observations

- % 10 naturally reverses digit order
- Overflow must be checked before multiply
- Stack usage here is optional, not required
- Avoid long even for temporary storage
---

## 10. Pitfalls

- Checking overflow after building the number
- Using long (violates constraints)
- Forgetting negative range (-8 case)
- Using stack where arithmetic suffices
---

<!-- #endregion -->
<!-- #region 211-Smallest Number Following Pattern -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q215: Smallest Number Following Pattern</h1>

## 1. Problem Statement

- You are given a pattern string str consisting of characters 'i' and 'd'.
  * 'i' means increasing
  * 'd' means decreasing
- You must construct the smallest possible number using digits from 1 to 9 without repetition, such that:
  * digits increase where pattern has 'i'
  * digits decrease where pattern has 'd'
- The length of the resulting number will be str.length + 1.
---

## 2. Problem Understanding

- Pattern length = n
- Result length = n + 1
- Digits must be:
  * unique
  * chosen from 1 to 9
- “Smallest number” means:
  * smallest lexicographically
  * smallest digits as early as possible
- Key observation:
  * A continuous sequence of 'd' forces a reverse ordering of digits.
---

## 3. Constraints

- 1 <= str.length <= 8
- Digits allowed: 1 to 9 (no repetition)
---

## 4. Edge Cases

- All 'i' → straight increasing
- All 'd' → fully decreasing
- Single character pattern
- Pattern ending with 'd'
- Mixed patterns like "iiddd"
---

## 5. Examples

```text
Input:  d
Output: 21

Input:  i
Output: 12

Input:  ddd
Output: 4321

Input:  iiddd
Output: 126543
```

---

## 6. Approaches

### Approach 1: Brute Force (Generate Permutations)

**Idea:**
- Generate all permutations of digits 1–9
- Pick the smallest number that satisfies the pattern

**Steps:**
- Generate permutations of length n+1
- Check pattern validity
- Track smallest valid number

**Java Code:**
```java
public String smallestNumberBrute(String pattern) {
    List<String> res = new ArrayList<>();
    permute("", "123456789", pattern.length() + 1, res);

    String ans = null;
    for (String s : res) {
        if (matches(s, pattern)) {
            if (ans == null || s.compareTo(ans) < 0) ans = s;
        }
    }
    return ans;
}

private void permute(String curr, String rem, int len, List<String> res) {
    if (curr.length() == len) {
        res.add(curr);
        return;
    }
    for (int i = 0; i < rem.length(); i++) {
        permute(curr + rem.charAt(i),
                rem.substring(0, i) + rem.substring(i + 1),
                len, res);
    }
}

private boolean matches(String num, String p) {
    for (int i = 0; i < p.length(); i++) {
        if (p.charAt(i) == 'i' && num.charAt(i) > num.charAt(i + 1)) return false;
        if (p.charAt(i) == 'd' && num.charAt(i) < num.charAt(i + 1)) return false;
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Try all possibilities and pick the smallest valid one

**Complexity (Time & Space):**
- Time: O(9!) — factorial permutations
- Space: O(9!) — storing permutations

### Approach 2: Greedy with Reversal (Better)

**Idea:**
- Start with increasing sequence 1 2 3 ...
- Reverse segments corresponding to 'd'

**Steps:**
- Initialize result array with 1..n+1
- For each continuous 'd' segment:
  * reverse that segment

**Java Code:**
```java
public String smallestNumberGreedy(String str) {
    int n = str.length();
    int[] arr = new int[n + 1];
    for (int i = 0; i <= n; i++) arr[i] = i + 1;

    int i = 0;
    while (i < n) {
        if (str.charAt(i) == 'd') {
            int j = i;
            while (j < n && str.charAt(j) == 'd') j++;
            reverse(arr, i, j);
            i = j;
        } else {
            i++;
        }
    }

    StringBuilder sb = new StringBuilder();
    for (int x : arr) sb.append(x);
    return sb.toString();
}

private void reverse(int[] arr, int l, int r) {
    while (l < r) {
        int t = arr[l];
        arr[l] = arr[r];
        arr[r] = t;
        l++; r--;
    }
}
```

**💭 Intuition Behind the Approach:**
- 'd' means local reversal
- Using smallest digits early ensures minimal number

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

### Approach 3: Stack-Based (Optimal & Clean)

**Idea:**
- Use stack to delay output
- Push increasing digits
- On 'i', flush stack
- Stack naturally reverses 'd' sequences

**Steps:**
- Initialize number counter from 1
- Traverse pattern:
  * Push number to stack
  * If 'i', pop all from stack
- After loop, push last number and pop all

**Java Code:**
```java
public String smallestNumber(String str) {
    StringBuilder ans = new StringBuilder();
    Stack<Integer> st = new Stack<>();

    int num = 1;
    for (int i = 0; i < str.length(); i++) {
        st.push(num++);

        if (str.charAt(i) == 'i') {
            while (!st.isEmpty()) {
                ans.append(st.pop());
            }
        }
    }

    st.push(num);
    while (!st.isEmpty()) {
        ans.append(st.pop());
    }

    return ans.toString();
}
```

**💭 Intuition Behind the Approach:**
- Stack reverses digits for 'd'
- 'i' forces output
- Uses smallest available digits first

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

---

## 7. Justification / Proof of Optimality

- Stack approach is simplest and most intuitive
- Greedy reversal is correct but harder to reason
- Brute force is only for conceptual understanding
---

## 8. Variants / Follow-Ups

- Largest number following pattern
- Pattern with 'i', 'd', '='
- Binary pattern to number mapping
---

## 9. Tips & Observations

- Result length is always pattern.length + 1
- Continuous 'd' means reverse
- Stack is ideal for delayed output problems
---

## 10. Pitfalls

- Forgetting the last digit
- Using digits beyond 9
- Outputting too early on 'd'
- Repeating digits
---

<!-- #endregion -->
<!-- #region 212-Celebrity Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q216: Celebrity Problem</h1>

## 1. Problem Statement

- You are given a party of N people.
- A matrix M of size N x N represents who knows whom:
  * M[i][j] = 1 → person i knows person j
  * M[i][i] = 0 always
- A celebrity is a person who:
  * is known by everyone else
  * knows no one
- You must return the index (0-based) of the lowest-numbered celebrity, or -1 if no celebrity exists.
---

## 2. Problem Understanding

- For a person c to be a celebrity:
  * Row c must be all 0 → c knows no one
  * Column c must be all 1 except M[c][c] → everyone knows c
- Brute checking every person works but is inefficient.
- Key idea:
  * If person A knows person B, then A cannot be a celebrity.
---

## 3. Constraints

- 2 <= N <= 300
- M[i][j] ∈ {0,1}
- M[i][i] = 0
---

## 4. Edge Cases

- No celebrity exists
- Multiple possible candidates → return lowest index
- Everyone knows someone
- Only one valid candidate after elimination
---

## 5. Examples

```text
Input:
2
0 1
0 0

Output:
1
```

---

## 6. Approaches

### Approach 1: Brute Force (Check Every Person)

**Idea:**
- Assume each person as celebrity
- Verify row and column conditions

**Steps:**
- For each person i:
  * Check row i → all zeros
  * Check column i → all ones except diagonal
- If both satisfy → return i

**Java Code:**
```java
public int celebrityBrute(int[][] M, int n) {
    for (int i = 0; i < n; i++) {
        boolean celeb = true;

        for (int j = 0; j < n; j++) {
            if (i != j) {
                if (M[i][j] == 1 || M[j][i] == 0) {
                    celeb = false;
                    break;
                }
            }
        }

        if (celeb) return i;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Directly enforce celebrity definition

**Complexity (Time & Space):**
- Time: O(N^2) — checking row + column for each person
- Space: O(1) — no extra space

### Approach 2: Stack Elimination (Better)

**Idea:**
- Push all people into stack
- Compare two people at a time
- Eliminate one who cannot be celebrity

**Steps:**
- Push all indices into stack
- Pop two persons a, b
- If a knows b → a cannot be celebrity
- Else b cannot be celebrity
- Push the remaining candidate
- Verify final candidate

**Java Code:**
```java
public int celebrityStack(int[][] M, int n) {
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) st.push(i);

    while (st.size() > 1) {
        int a = st.pop();
        int b = st.pop();

        if (M[a][b] == 1) st.push(b);
        else st.push(a);
    }

    int cand = st.pop();

    for (int i = 0; i < n; i++) {
        if (i != cand) {
            if (M[cand][i] == 1 || M[i][cand] == 0) return -1;
        }
    }

    return cand;
}
```

**💭 Intuition Behind the Approach:**
- Every comparison removes one non-celebrity
- Only one candidate can survive

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N) — stack

### Approach 3: Two-Pointer Elimination (Optimal)

**Idea:**
- Use two pointers instead of stack
- Same elimination logic with O(1) space

**Steps:**
- Initialize a = 0, b = n - 1
- While a < b:
  * If a knows b → a++
  * Else b--
- Remaining index is candidate
- Verify candidate

**Java Code:**
```java
public int celebrity(int[][] M, int n) {
    int a = 0, b = n - 1;

    while (a < b) {
        if (M[a][b] == 1) a++;
        else b--;
    }

    int cand = a;

    for (int i = 0; i < n; i++) {
        if (i != cand) {
            if (M[cand][i] == 1 || M[i][cand] == 0) return -1;
        }
    }

    return cand;
}
```

**💭 Intuition Behind the Approach:**
- Elimination logic does not require stack
- Constant space version of stack solution

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(1)

---

## 7. Justification / Proof of Optimality

- At most one celebrity can exist
- Pairwise elimination guarantees removal of non-celebrities
- Final verification ensures correctness
---

## 8. Variants / Follow-Ups

- Find all celebrities
- Celebrity in directed graph
- Party with incomplete information
---

## 9. Tips & Observations

- Knowing someone immediately disqualifies a candidate
- Being unknown disqualifies a candidate
- Elimination is more efficient than checking everyone
---

## 10. Pitfalls

- Forgetting final verification
- Assuming survivor is always celebrity
- Checking M[i][i]
- Returning first candidate without validation
---

<!-- #endregion -->
<!-- #region 213-Infix Evaluation and Conversions -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q217: Infix Evaluation and Conversions</h1>

## 1. Problem Statement

- You are given an infix expression consisting of:
  * single-digit operands (0–9)
  * operators: +, -, *, /
  * parentheses ( and )
- You must:
  * Evaluate the infix expression
  * Convert it to postfix expression
  * Convert it to prefix expression
---

## 2. Problem Understanding

- Infix notation places operators between operands
- Operator precedence matters:
  * * and / > + and -
- Parentheses override precedence
- Expression is guaranteed to be valid
- All operands are single-digit, so parsing is simpler
- Key challenge:
  * Handle evaluation + postfix + prefix conversion together while respecting precedence and parentheses.
---

## 3. Constraints

- Expression length ≤ reasonable limits
- Operands: 0–9
- Operators: + - * /
- Expression is valid
---

## 4. Edge Cases

- Nested parentheses
- Expression with only one operand
- Multiple operators with same precedence
- Division producing integer result
---

## 5. Examples

```text
Input:  ((2+((6*4)/8))-3)

Output:
2
264*8/+3-
-+2/*6483



Input:  ((1+2)+((((3*4)/5)*6)/7))

Output:
4
12+34*5/6*7/+
++12/*/*34567
```

---

## 6. Approaches

### Approach 1: Brute Force (Separate Conversions)

**Idea:**
- Convert infix → postfix
- Evaluate postfix
- Convert postfix → prefix

**Steps:**
- Convert infix to postfix using stack
- Evaluate postfix expression
- Convert postfix to prefix

**Java Code:**
```java
import java.util.*;

public class ExpressionConversion {

    // Entry method
    public void bruteInfix(String exp) {
        String postfix = infixToPostfix(exp);
        int val = evalPostfix(postfix);
        String prefix = postfixToPrefix(postfix);

        System.out.println(val);
        System.out.println(postfix);
        System.out.println(prefix);
    }

    // Operator precedence
    private int precedence(char ch) {
        if (ch == '+' || ch == '-') return 1;
        if (ch == '*' || ch == '/') return 2;
        return 0;
    }

    // Infix to Postfix
    private String infixToPostfix(String exp) {
        Stack<Character> st = new Stack<>();
        StringBuilder sb = new StringBuilder();

        for (char ch : exp.toCharArray()) {

            if (Character.isDigit(ch)) {
                sb.append(ch);
            }
            else if (ch == '(') {
                st.push(ch);
            }
            else if (ch == ')') {
                while (!st.isEmpty() && st.peek() != '(') {
                    sb.append(st.pop());
                }
                st.pop(); // remove '('
            }
            else { // operator
                while (!st.isEmpty() && precedence(st.peek()) >= precedence(ch)) {
                    sb.append(st.pop());
                }
                st.push(ch);
            }
        }

        while (!st.isEmpty()) {
            sb.append(st.pop());
        }

        return sb.toString();
    }

    // Evaluate Postfix
    private int evalPostfix(String exp) {
        Stack<Integer> st = new Stack<>();

        for (char ch : exp.toCharArray()) {
            if (Character.isDigit(ch)) {
                st.push(ch - '0');
            } else {
                int b = st.pop();
                int a = st.pop();

                if (ch == '+') st.push(a + b);
                else if (ch == '-') st.push(a - b);
                else if (ch == '*') st.push(a * b);
                else if (ch == '/') st.push(a / b);
            }
        }
        return st.pop();
    }

    // Postfix to Prefix
    private String postfixToPrefix(String exp) {
        Stack<String> st = new Stack<>();

        for (char ch : exp.toCharArray()) {
            if (Character.isDigit(ch)) {
                st.push(String.valueOf(ch));
            } else {
                String b = st.pop();
                String a = st.pop();
                st.push(ch + a + b);
            }
        }
        return st.pop();
    }
}

```

**💭 Intuition Behind the Approach:**
- Break problem into known sub-problems
- Easy to reason but inefficient and repetitive

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

### Approach 2: Stack-Based (Better, Separate Stacks)

**Idea:**
- Use stacks for:
  * values
  * postfix strings
  * prefix strings
  * operators
- Process operators when precedence rules apply

**Steps:**
- Traverse expression
- Push operands into value/postfix/prefix stacks
- Push operators into operator stack
- On closing parenthesis or precedence violation:
  * pop operator
  * evaluate + build postfix & prefix

**Java Code:**
```java
public void evaluateInfix(String exp) {

    Stack<Integer> values = new Stack<>();
    Stack<String> postfix = new Stack<>();
    Stack<String> prefix = new Stack<>();
    Stack<Character> ops = new Stack<>();

    for (char ch : exp.toCharArray()) {

        if (ch == '(') {
            ops.push(ch);
        }
        else if (Character.isDigit(ch)) {
            values.push(ch - '0');
            postfix.push(ch + "");
            prefix.push(ch + "");
        }
        else if (ch == '+' || ch == '-' || ch == '*' || ch == '/') {
            while (!ops.isEmpty() && ops.peek() != '(' &&
                   precedence(ops.peek()) >= precedence(ch)) {
                process(values, postfix, prefix, ops);
            }
            ops.push(ch);
        }
        else if (ch == ')') {
            while (ops.peek() != '(') {
                process(values, postfix, prefix, ops);
            }
            ops.pop();
        }
    }

    while (!ops.isEmpty()) {
        process(values, postfix, prefix, ops);
    }

    System.out.println(values.pop());
    System.out.println(postfix.pop());
    System.out.println(prefix.pop());
}
```

### 💭 Intuition Behind the Approach

- Infix expressions cannot be evaluated directly from left to right because:
  - operator precedence matters
  - parentheses override precedence
- Therefore, an **operator stack** is used to delay operations until they are safe to apply.

- At the same time, whenever an operator is applied:
  - it combines the **last two operands**
  - the same combination logic can be reused to:
    - compute the numeric value
    - build the postfix expression
    - build the prefix expression

- The key idea is:

  > **Whenever an operator becomes ready to execute,  
  > resolve it once and update all three representations together.**

- Multiple stacks are used:
  - `values` → stores evaluated integers
  - `postfix` → stores postfix strings
  - `prefix` → stores prefix strings
  - `ops` → controls when an operation should be applied

- This keeps evaluation and conversions **synchronized** and avoids repeated passes.

---

### ⏱️ Complexity

- **Time:** `O(N)` — each character is pushed and popped at most once across stacks
- **Space:** `O(N)` — stacks store operands, operators, and intermediate expressions

---

### 🔒 Note

> **This approach is already optimal.**  
> Any further “optimal” approach is the same algorithm, just classified differently.
 ---
### Approach 3: Optimal (Single-Pass Multi-Stack)

**Idea:**
- Solve evaluation + postfix + prefix simultaneously
- Each operator resolution does three operations together

**Steps:**
- Maintain 4 stacks:
  * values, postfix, prefix, operators
- On operand:
  * push into all 3 stacks
- On operator:
  * resolve based on precedence
- On ):
  * resolve until (

**Java Code:**
```java
private void process(Stack<Integer> values,
                     Stack<String> postfix,
                     Stack<String> prefix,
                     Stack<Character> ops) {

    char op = ops.pop();

    int b = values.pop();
    int a = values.pop();
    values.push(apply(a, b, op));

    String pb = postfix.pop();
    String pa = postfix.pop();
    postfix.push(pa + pb + op);

    String prb = prefix.pop();
    String pra = prefix.pop();
    prefix.push(op + pra + prb);
}

private int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    return 2;
}

private int apply(int a, int b, char op) {
    if (op == '+') return a + b;
    if (op == '-') return a - b;
    if (op == '*') return a * b;
    return a / b;
}
```

**💭 Intuition Behind the Approach:**
- Every operator combines:
  * two values
  * two postfix strings
  * two prefix strings
- Synchronizing them avoids redundant passes

**Complexity (Time & Space):**
- Time: O(N) — each character processed once
- Space: O(N) — stacks

---

## 7. Justification / Proof of Optimality

- Infix expressions require stack due to precedence
- Multi-stack solution avoids repeated conversions
- Clean and scalable approach
---

## 8. Variants / Follow-Ups

- Infix → Postfix only
- Infix → Prefix only
- Expression evaluation with variables
- Multi-digit operands
---

## 9. Tips & Observations

- Always resolve higher precedence first
- Parentheses force immediate resolution
- Prefix & postfix are mirror forms of expression tree
---

## 10. Pitfalls

- Forgetting operator precedence
- Reversing operand order (a op b)
- Ignoring parentheses
- Doing evaluation after full conversion
---

<!-- #endregion -->
<!-- #region 214-Postfix Evaluation And Conversions   -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q218: Postfix Evaluation And Conversions</h1>

## 1. Problem Statement

- You are given a postfix expression consisting of:
  - single-digit operands (0–9)
  - operators: +, -, \*, /
- You must:
  - Evaluate the postfix expression
  - Convert it to an infix expression (with brackets)
  - Convert it to a prefix expression

---

## 2. Problem Understanding

- In postfix notation, operators come after operands
- Operator precedence is already encoded in postfix
- No parentheses are present in input
- Every operator always applies to the two most recent operands
- Key insight:
  - Postfix expressions are evaluated naturally using a stack.

---

## 3. Constraints

- Expression is valid
- Operands are single-digit numbers
- Operators: + - \* /

---

## 4. Edge Cases

- Single operand only
- Nested operations
- Division producing integer result
- Order-sensitive operators (-, /)

---

## 5. Examples

```text
Input:  264*8/+3-

Output:
2
((2+((6*4)/8))-3)
-+2/*6483


Input:  12+34*5/6*7/+

Output:
4
((1+2)+((((3*4)/5)*6)/7))
++12/*/*34567
```

---

## 6. Approaches

### Approach 1: Brute Force (Expression Tree)

**Idea:**

- Build an expression tree from postfix
- Traverse tree to:
  - evaluate
  - generate infix
  - generate prefix

**Steps:**

- Create tree nodes for operands
- On operator:
  - pop two nodes
  - make them children
- Traverse tree

**Java Code:**

```java
class Node {
    char val;
    Node left, right;
}

public void postfixBrute(String exp) {
    Stack<Node> st = new Stack<>();

    for (char c : exp.toCharArray()) {
        Node node = new Node();
        node.val = c;

        if (Character.isDigit(c)) {
            st.push(node);
        } else {
            node.right = st.pop();
            node.left = st.pop();
            st.push(node);
        }
    }

    Node root = st.pop();
    System.out.println(eval(root));
    System.out.println(infix(root));
    System.out.println(prefix(root));
}
```

**💭 Intuition Behind the Approach:**

- Postfix naturally represents an expression tree
- Tree traversal gives all notations

**Complexity (Time & Space):**

- Time: O(N)
- Space: O(N)

### Approach 2: Stack-Based (Standard)

**Idea:**

- Use three stacks:
  - values
  - infix strings
  - prefix strings
- Process postfix left to right

**Steps:**

- Traverse postfix expression
- On operand:
  - push into all stacks
- On operator:
  - pop two operands
  - evaluate
  - build infix & prefix

**Java Code:**

```java
public void postfixStack(String exp) {
    Stack<Integer> val = new Stack<>();
    Stack<String> inf = new Stack<>();
    Stack<String> pre = new Stack<>();

    for (char c : exp.toCharArray()) {
        if (Character.isDigit(c)) {
            val.push(c - '0');
            inf.push(c + "");
            pre.push(c + "");
        } else {
            int b = val.pop();
            int a = val.pop();
            val.push(apply(a, b, c));

            String infB = inf.pop();
            String infA = inf.pop();
            inf.push("(" + infA + c + infB + ")");

            String preB = pre.pop();
            String preA = pre.pop();
            pre.push(c + preA + preB);
        }
    }

    System.out.println(val.pop());
    System.out.println(inf.pop());
    System.out.println(pre.pop());
}

private int apply(int a, int b, char op) {
    if (op == '+') return a + b;
    if (op == '-') return a - b;
    if (op == '*') return a * b;
    return a / b;
}


```

### 💭 Intuition Behind the Approach (Approach 2: Stack-Based)

- In postfix expressions, **operator precedence is already resolved**, so no operator stack is needed.
- The expression is processed **left to right**:
  - Operands are pushed onto stacks.
  - When an operator appears, it always applies to the **two most recent operands**.
- The same pop order (`left operand`, `right operand`) can be reused to:
  - compute the numeric value
  - construct the infix expression (with brackets)
  - construct the prefix expression
- Using **three parallel stacks** keeps all representations synchronized:
  - `values` stack → evaluates the expression
  - `infix` stack → builds bracketed infix form
  - `prefix` stack → builds prefix form

> **Key idea:**  
> Every operator resolution combines exactly two operands, and that single combination updates
> evaluation, infix, and prefix simultaneously.

---

### ⏱️ Complexity

- **Time:** `O(N)`  
  — each character of the postfix expression is pushed and popped at most once

- **Space:** `O(N)`  
  — stacks store operands and intermediate infix/prefix strings

---

### 🔒 Note

> This approach is both **standard and optimal** for postfix evaluation and conversions.  
> Any further “optimal” approach is the same algorithm, just labeled differently.

---

## 7. Justification / Proof of Optimality

- Postfix evaluation is naturally stack-based
- No need for operator precedence handling
- Clean, linear-time solution

---

## 8. Variants / Follow-Ups

- Prefix evaluation
- Infix to Postfix conversion
- Infix to Prefix conversion
- Multi-digit operands
- Variable-based expressions (a+b\*c)
- Expression Tree construction

---

## 9. Tips & Observations

- Always pop right operand first
- Wrap infix expressions in brackets
- Order matters for - and /

---

## 10. Pitfalls

- Reversing operand order
- Forgetting parentheses in infix
- Treating postfix like infix
- Using recursion unnecessarily

---

<!-- #endregion -->
<!-- #region 215-  Prefix Evaluation and Conversion -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q219: Prefix Evaluation and Conversion</h1>

## 1. Problem Statement

- You are given a prefix expression consisting of:
  * single-digit operands (0–9)
  * operators: +, -, *, /
- You must:
  * Evaluate the prefix expression
  * Convert it to an infix expression (with brackets)
  * Convert it to a postfix expression
---

## 2. Problem Understanding

- In prefix notation, operators come before operands
- Each operator always applies to the next two operands
- Operator precedence is already encoded in prefix
- Expression is guaranteed to be valid
- All operands are single-digit, simplifying parsing
- Key insight:
  * Prefix expressions are best processed from right to left using a stack.
---

## 3. Constraints

- Expression is valid
- Operands are single-digit numbers
- Operators: + - * /
---

## 4. Edge Cases

- Single operand only
- Nested prefix expressions
- Order-sensitive operators (-, /)
- Division results are integer-based
---

## 5. Examples

```text
Input:

-+2/*6483


Output:

2
((2+((6*4)/8))-3)
264*8/+3-


Input:

++123


Output:

6
((1+2)+3)
12+3+
```

---

## 6. Approaches

### Approach 1: Brute Force (Expression Tree)

**Idea:**
- Build an expression tree from prefix expression
- Traverse the tree to:
  * evaluate the expression
  * generate infix
  * generate postfix

**Steps:**
- Read prefix expression from right to left
- On operand → create node and push
- On operator:
  * pop two nodes
  * make them left and right children
- Traverse the tree

**Java Code:**
```java
class Node {
    char val;
    Node left, right;
}

public void prefixBrute(String exp) {
    Stack<Node> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char c = exp.charAt(i);
        Node node = new Node();
        node.val = c;

        if (Character.isDigit(c)) {
            st.push(node);
        } else {
            node.left = st.pop();
            node.right = st.pop();
            st.push(node);
        }
    }

    Node root = st.pop();
    System.out.println(eval(root));
    System.out.println(infix(root));
    System.out.println(postfix(root));
}
```

**💭 Intuition Behind the Approach:**
- Prefix naturally maps to an expression tree
- Tree traversal gives all forms

**Complexity (Time & Space):**
- Time: O(N)  Space: O(N)

### Approach 2: Stack-Based (Standard & Optimal)

**Idea:**
- Use stacks to evaluate and convert simultaneously
- Process prefix from right to left
- On operator, combine the last two operands

**Steps:**
- Traverse expression from right to left
- On operand:
  * push into value, infix, postfix stacks
- On operator:
  * pop two operands
  * evaluate value
  * build infix and postfix strings
- Final stack top contains result

**Java Code:**
```java
public void evaluatePrefix(String exp) {

    Stack<Integer> values = new Stack<>();
    Stack<String> infix = new Stack<>();
    Stack<String> postfix = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);

        if (Character.isDigit(ch)) {
            values.push(ch - '0');
            infix.push(ch + "");
            postfix.push(ch + "");
        } else {
            int a = values.pop();
            int b = values.pop();
            values.push(apply(a, b, ch));

            String ia = infix.pop();
            String ib = infix.pop();
            infix.push("(" + ia + ch + ib + ")");

            String pa = postfix.pop();
            String pb = postfix.pop();
            postfix.push(pa + pb + ch);
        }
    }

    System.out.println(values.pop());
    System.out.println(infix.pop());
    System.out.println(postfix.pop());
}
```

**💭 Intuition Behind the Approach:**
- Prefix evaluation requires right-to-left traversal
- Stack ensures correct operand pairing
- The same pop order builds:
  * evaluated value
  * infix expression
  * postfix expression
- Evaluation and conversions stay synchronized

**Complexity (Time & Space):**
- Time: O(N) — each character processed once
- Space: O(N) — stacks store intermediate results

---

## 7. Justification / Proof of Optimality

- Prefix expressions remove precedence ambiguity
- Stack guarantees correct operand order
- No better asymptotic solution exists
---

## 8. Variants / Follow-Ups

- Prefix evaluation only
- Prefix to infix conversion
- Prefix to postfix conversion
- Expression evaluation with variables
---

## 9. Tips & Observations

- Always process prefix from right to left
- Operand order matters for - and /
- Wrap infix expressions in brackets
- Stack size reduces by one on every operator
---

## 10. Pitfalls

- Traversing left to right (wrong)
- Reversing operand order
- Forgetting parentheses in infix
- Treating prefix like postfix
---

<!-- #endregion -->
<!-- #region 216-Trapping Rainwater Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q220: Trapping Rainwater Problem</h1>

## 1. Problem Statement

- You are given n non-negative integers representing an elevation map, where the width of each bar is 1.
- Compute how much water can be trapped after raining.
---

## 2. Problem Understanding

- Water can be trapped between two taller bars
- At any index i, trapped water depends on:
  * the maximum height to its left
  * the maximum height to its right
- Water at index i is:
  * water[i] = min(leftMax[i], rightMax[i]) - height[i]
- (if positive)
---

## 3. Constraints

- 1 <= n <= 2 * 10^4
- 0 <= height[i] <= 10^5
---

## 4. Edge Cases

- n <= 2 → no water
- All bars same height → no water
- Strictly increasing or decreasing heights → no water
- Large height values
---

## 5. Examples

```text
Trapping Rain Water — Text Diagram (Example 1)

Input

height = [0,1,0,2,1,0,1,3,2,1,2,1]

Histogram View (bars only)
        █
        █ █
    █   █ █ █
    █ █ █ █ █ █
  █ █ █ █ █ █ █ █
--------------------------------
 0 1 0 2 1 0 1 3 2 1 2 1

Histogram + Trapped Water (~ = water)
        █
        █ █
    █~~~█ █~█
    █~█~█ █~█ █
  █ █~█~█ █~█ █ █
--------------------------------
 0 1 0 2 1 0 1 3 2 1 2 1

Index-wise Explanation
Index :  0 1 2 3 4 5 6 7 8 9 10 11
Height:  0 1 0 2 1 0 1 3 2 1  2  1
Water :  0 0 1 0 1 2 1 0 0 1  0  0

Total Water Trapped
= 1 + 1 + 2 + 1 + 1
= 6 units
```

---

## 6. Approaches

### Approach 1: Brute Force

**Idea:**
- For every index:
  * find the maximum height on the left
  * find the maximum height on the right
  * calculate trapped water

**Steps:**
- For each index i
- Scan left to find leftMax
- Scan right to find rightMax
- Add min(leftMax, rightMax) - height[i] if positive

**Java Code:**
```java
public int trapBrute(int[] height) {
    int n = height.length;
    int water = 0;

    for (int i = 0; i < n; i++) {
        int leftMax = 0, rightMax = 0;

        for (int l = 0; l <= i; l++)
            leftMax = Math.max(leftMax, height[l]);

        for (int r = i; r < n; r++)
            rightMax = Math.max(rightMax, height[r]);

        water += Math.max(0, Math.min(leftMax, rightMax) - height[i]);
    }
    return water;
}
```

**💭 Intuition Behind the Approach:**
- Water at a bar depends on the shorter boundary
- Directly simulate the definition

**Complexity (Time & Space):**
- Time: O(N^2) — nested scans
- Space: O(1) — no extra space

### Approach 2: Dynamic Programming (Prefix & Suffix Max)

**Idea:**
- Precompute:
  * maximum height to the left of every index
  * maximum height to the right of every index
- Use formula directly

**Steps:**
- Build leftMax[]
- Build rightMax[]
- For each index:
  * add trapped water

**Java Code:**
```java
public int trapDP(int[] height) {
    int n = height.length;
    int[] leftMax = new int[n];
    int[] rightMax = new int[n];

    leftMax[0] = height[0];
    for (int i = 1; i < n; i++)
        leftMax[i] = Math.max(leftMax[i - 1], height[i]);

    rightMax[n - 1] = height[n - 1];
    for (int i = n - 2; i >= 0; i--)
        rightMax[i] = Math.max(rightMax[i + 1], height[i]);

    int water = 0;
    for (int i = 0; i < n; i++)
        water += Math.max(0, Math.min(leftMax[i], rightMax[i]) - height[i]);

    return water;
}
```

**💭 Intuition Behind the Approach:**
- Avoid repeated scans by storing boundary heights
- Trade space for time

**Complexity (Time & Space):**
- Time: O(N) — three linear passes
- Space: O(N) — prefix & suffix arrays

### Approach 3: Two Pointers (Optimal)

**Idea:**
- Use two pointers moving inward
- Maintain leftMax and rightMax
- Decide side based on smaller boundary

**Steps:**
- Initialize left = 0, right = n - 1
- Maintain leftMax, rightMax
- Move pointer with smaller height
- Accumulate trapped water

**Java Code:**
```java
public int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int leftMax = 0, rightMax = 0;
    int water = 0;

    while (left <= right) {
        if (height[left] <= height[right]) {
            if (height[left] >= leftMax)
                leftMax = height[left];
            else
                water += leftMax - height[left];
            left++;
        } else {
            if (height[right] >= rightMax)
                rightMax = height[right];
            else
                water += rightMax - height[right];
            right--;
        }
    }
    return water;
}
```

**💭 Intuition Behind the Approach:**
- Water is limited by the shorter side
- If left side is smaller, right side boundary is guaranteed
- Eliminates extra arrays

**Complexity (Time & Space):**
- Time: O(N) — single traversal
- Space: O(1) — constant extra space
- ✅ Interview-Preferred Solution

### Approach 4: Monotonic Stack (Valley Trapping)

**Idea:**
- Use a monotonic decreasing stack that stores indices of bars.
- The stack helps detect “valleys” between two taller bars where water can be trapped.
- When we find a bar taller than the bar at the top of the stack:
  * the popped bar becomes the bottom of the valley
  * the current bar is the right boundary
  * the new stack top is the left boundary
- Water trapped is calculated using:
  * width = rightIndex − leftIndex − 1
  * height = min(leftHeight, rightHeight) − valleyHeight

**Steps:**
- Traverse the array from left to right
- Maintain a stack of indices with decreasing heights
- While current height is greater than the height at stack top:
  * pop the valley index
  * if stack becomes empty, break (no left boundary)
  * calculate trapped water using left and right boundaries
- Push current index into the stack

**Java Code:**
```java
public int trapStack(int[] height) {
    Stack<Integer> st = new Stack<>();
    int water = 0;

    for (int i = 0; i < height.length; i++) {
        while (!st.isEmpty() && height[i] > height[st.peek()]) {
            int mid = st.pop();   // valley

            if (st.isEmpty()) break;

            int left = st.peek();
            int width = i - left - 1;
            int h = Math.min(height[left], height[i]) - height[mid];

            water += width * h;
        }
        st.push(i);
    }
    return water;
}
```

**💭 Intuition Behind the Approach:**
- public int trapStack(int[] height) {
    * Stack<Integer> st = new Stack<>();
    * int water = 0;
    * for (int i = 0; i < height.length; i++) {
        * while (!st.isEmpty() && height[i] > height[st.peek()]) {
            * int mid = st.pop();   // valley
            * if (st.isEmpty()) break;
            * int left = st.peek();
            * int width = i - left - 1;
            * int h = Math.min(height[left], height[i]) - height[mid];
            * water += width * h;
        * }
        * st.push(i);
    * }
    * return water;
- }

**Complexity (Time & Space):**
- Time: O(N) — each index is pushed and popped at most once
- Space: O(N) — stack stores indices
- Note
  * This approach is mainly used to teach the monotonic stack pattern.
  * The two-pointer approach is still the most optimal and interview-preferred solution.

---

## 7. Justification / Proof of Optimality

- Brute force matches definition but inefficient
- DP optimizes time with extra space
- Two-pointer solution achieves optimal balance
---

## 8. Variants / Follow-Ups

- Container With Most Water
- Maximum area histogram
- Trapping rainwater II (2D grid)
---

## 9. Tips & Observations

- Always think in terms of boundaries
- Minimum of left & right max controls water
- Two pointers eliminate redundant memory
---

## 10. Pitfalls

- Forgetting to clamp negative water
- Using wrong boundary in two-pointer logic
- Confusing with container problem
- Off-by-one errors in loops
---

<!-- #endregion -->
<!-- #region 217- Merge Intervals -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q221: Merge Intervals</h1>

## 1. Problem Statement

- You are given n intervals, where each interval is represented by a start and end time.
- Your task is to merge all overlapping intervals and return the resulting list of mutually exclusive intervals.
- Each merged interval should be printed on a new line.
---

## 2. Problem Understanding

- Intervals may be given in any order
  * Two intervals overlap if:
  * current.start <= previous.end
- Overlapping intervals must be merged into a single interval:
  * merged.start = min(start)
  * merged.end   = max(end)
- Non-overlapping intervals remain separate
- Key insight:
  * Sorting intervals by start time makes merging straightforward.
---

## 3. Constraints

- 1 <= n <= 10000
- Interval values can be large
- Output should be sorted by start time
---

## 4. Edge Cases

- Single interval
- All intervals overlapping
- No intervals overlapping
- Intervals fully contained within others
- Intervals touching at boundaries ([1,3] and [3,5] → overlapping)
---

## 5. Examples

```text
Input:
1 3
2 4
6 8
9 10

Output:
1 4
6 8
9 10
Explanation

Given intervals: [1,3],[2,4],[6,8],[9,10], we have only two overlapping intervals here,[1,3] and [2,4]. Therefore we will merge these two and return [1,4],[6,8], [9,10].


Input:
6 8
1 9
2 4
6 7

Output:
1 9
```

---

## 6. Approaches

### Approach 1: Brute Force (Repeated Merging)

**Idea:**
- Compare every interval with every other interval
- Merge overlapping ones repeatedly until no more merges are possible

**Steps:**
- Loop through all pairs of intervals
- Merge if overlapping
- Repeat until no changes occur

**Java Code:**
```java
public List<int[]> mergeBrute(List<int[]> intervals) {
    boolean merged;
    do {
        merged = false;
        for (int i = 0; i < intervals.size(); i++) {
            for (int j = i + 1; j < intervals.size(); j++) {
                int[] a = intervals.get(i);
                int[] b = intervals.get(j);

                if (a[1] >= b[0] && b[1] >= a[0]) {
                    a[0] = Math.min(a[0], b[0]);
                    a[1] = Math.max(a[1], b[1]);
                    intervals.remove(j);
                    merged = true;
                    break;
                }
            }
            if (merged) break;
        }
    } while (merged);

    return intervals;
}
```

**💭 Intuition Behind the Approach:**
- Keep merging overlapping intervals until no overlap remains
- Direct but inefficient

**Complexity (Time & Space):**
- Time: O(N^2) — repeated pairwise checks
- Space: O(1) — in-place merging

### Approach 2: Sorting + Linear Merge (Greedy)

**Idea:**
- Sort intervals by start time
- Merge intervals greedily in one pass

**Steps:**
- Sort intervals based on start time
- Initialize current interval
- For each next interval:
  * If overlapping → extend current interval
  * Else → store current and move on

**Java Code:**
```java
public List<int[]> mergeIntervals(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    List<int[]> res = new ArrayList<>();

    int start = intervals[0][0];
    int end = intervals[0][1];

    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] <= end) {
            end = Math.max(end, intervals[i][1]);
        } else {
            res.add(new int[]{start, end});
            start = intervals[i][0];
            end = intervals[i][1];
        }
    }
    res.add(new int[]{start, end});
    return res;
}
```

**💭 Intuition Behind the Approach:**
- Sorting ensures overlapping intervals are adjacent
- Greedy merge avoids unnecessary comparisons

**Complexity (Time & Space):**
- Time: O(N log N) — sorting
- Space: O(N) — output list

### Approach 3: Stack-Based Merge (Alternative Optimal)

**Idea:**
- Sort intervals
- Use a stack to merge overlapping intervals

**Steps:**
- Sort intervals by start
- Push first interval to stack
- For each interval:
  * If overlapping → merge with stack top
  * Else → push new interval

**Java Code:**
```java
public List<int[]> mergeStack(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    Stack<int[]> st = new Stack<>();

    for (int[] in : intervals) {
        if (st.isEmpty() || st.peek()[1] < in[0]) {
            st.push(in);
        } else {
            st.peek()[1] = Math.max(st.peek()[1], in[1]);
        }
    }

    return new ArrayList<>(st);
}
```

**💭 Intuition Behind the Approach:**
- Stack stores already merged intervals
- Similar to linear greedy merge but explicit

**Complexity (Time & Space):**
- Time: O(N log N) — sorting
- Space: O(N) — stack

---

## 7. Justification / Proof of Optimality

- Sorting is required to group overlapping intervals
- Linear merging after sorting is optimal
- Stack-based version is optional and conceptually similar
---

## 8. Variants / Follow-Ups

- Insert interval
- Interval intersection
- Non-overlapping intervals count
- Meeting room problems
---

## 9. Tips & Observations

- Always sort by start time first
- Touching intervals (end == start) are overlapping
- Greedy works because earlier start dominates
---

## 10. Pitfalls

- Forgetting to add last interval
- Not sorting input
- Confusing overlapping vs disjoint intervals
- Modifying input without caution
---

<!-- #endregion -->
<!-- #region 218-Minimum Stack -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q222: Minimum Stack</h1>

## 1. Problem Statement

- Design a stack that supports the following operations in constant time O(1):
  * push(x) → insert element x
  * pop() → remove and return top element (-1 if empty)
  * getMin() → return the minimum element in the stack (-1 if empty)
---

## 2. Problem Understanding

- You are implementing a custom stack
- Along with normal stack behavior (LIFO), you must track the minimum element at every point
- getMin() must be O(1), not by scanning the stack
- Stack values are positive integers
- Key challenge:
  * How do we always know the minimum without traversing the stack?
---

## 3. Constraints

- 1 <= Number of operations <= 100
- 1 <= stack values <= 100
- Stack size is small, but time complexity still matters
---

## 4. Edge Cases

- Calling pop() on empty stack → return -1
- Calling getMin() on empty stack → return -1
- Popping the current minimum
- Duplicate minimum values
---

## 5. Examples

```text
Example 1
push(2)
push(3)
pop()      → 3
getMin()   → 2
push(1)
getMin()   → 1

Example 2
push(5)
push(8)
push(4)
push(6)
getMin()   → 4
pop()      → 6
pop()      → 4
getMin()   → 5
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Use a normal stack
- For getMin(), scan the entire stack to find minimum

**Steps:**
- Push → normal push
- Pop → normal pop
- getMin → traverse stack to find min

**Java Code:**
```java
int getMin(Stack<Integer> st) {
    if (st.isEmpty()) return -1;
    int min = Integer.MAX_VALUE;
    for (int x : st) {
        min = Math.min(min, x);
    }
    return min;
}
```

**💭 Intuition Behind the Approach:**
- Simple but inefficient — each getMin() recomputes the minimum from scratch.

**Complexity (Time & Space):**
- Time: O(N) — scanning stack every time
- Space: O(1) — no extra data structure

### Approach 2: : Two Stacks (Standard Optimal Solution)

**Idea:**
- Maintain two stacks:
  * mainStack → stores all values
  * minStack → stores minimum so far at each level

**Steps:**
- push(x)
  * push x to mainStack
  * push min(x, minStack.peek()) to minStack
- pop()
  * pop from both stacks
- getMin()
  * return minStack.peek()

**Java Code:**
```java
Stack<Integer> st = new Stack<>();
Stack<Integer> minSt = new Stack<>();

void push(int x) {
    st.push(x);
    if (minSt.isEmpty()) {
        minSt.push(x);
    } else {
        minSt.push(Math.min(x, minSt.peek()));
    }
}

int pop() {
    if (st.isEmpty()) return -1;
    minSt.pop();
    return st.pop();
}

int getMin() {
    if (minSt.isEmpty()) return -1;
    return minSt.peek();
}
```

**💭 Intuition Behind the Approach:**
- At every level, we remember the minimum till that point, so we never recompute.

**Complexity (Time & Space):**
- Time: O(1) — all operations
- Space: O(N) — extra stack for min values

### Approach 3: Single Stack with Encoding (Most Optimized)

**Idea:**
- Use one stack
- Encode values when pushing a new minimum
- Formula:
  * encodedValue = 2*x - currentMin

**Steps:**
- Maintain min variable
- push(x)
  * if stack empty → push x, min = x
  * if x >= min → push x
  * else → push encoded value, update min
- pop()
  * if popped < min → decode previous min
- getMin()
  * return min

**Java Code:**
```java
Stack<Integer> st = new Stack<>();
int min = Integer.MAX_VALUE;

void push(int x) {
    if (st.isEmpty()) {
        st.push(x);
        min = x;
    } else if (x >= min) {
        st.push(x);
    } else {
        st.push(2 * x - min);
        min = x;
    }
}

int pop() {
    if (st.isEmpty()) return -1;

    int top = st.pop();
    if (top >= min) {
        return top;
    } else {
        int originalMin = min;
        min = 2 * min - top;
        return originalMin;
    }
}

int getMin() {
    if (st.isEmpty()) return -1;
    return min;
}
```

**💭 Intuition Behind the Approach:**
- Encoded values act as markers
- They allow restoring the previous minimum during pop

**Complexity (Time & Space):**
- Time: O(1) — all operations
- Space: O(1) — no extra stack

---

## 7. Justification / Proof of Optimality

- Approach 2 is most readable and interview-safe
- Approach 3 is space-optimal and shows deep understanding
- Brute force fails the O(1) requirement
---

## 8. Variants / Follow-Ups

- Max Stack (track maximum instead of minimum)
- Min Stack with duplicate handling
- Stack supporting getMin() and getMax()
---

## 9. Tips & Observations

- If interviewer allows extra space → Two Stack approach
- If space optimized solution needed → Encoding approach
- Always clarify whether duplicate minimums exist (they do here)
---

## 10. Pitfalls

- Forgetting to sync minStack during pop
- Not updating min correctly during encoded pop
- Returning wrong value when popping encoded numbers
- Treating this like a normal stack without min tracking
---

<!-- #endregion -->
<!-- #region 219-Reverse Substrings Between Each Pair of Parentheses -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q223: Reverse Substrings Between Each Pair of Parentheses</h1>

## 1. Problem Statement

- You are given a string s consisting of:
  * lowercase English alphabets
  * parentheses '(' and ')'
- You must:
  * reverse the substring inside each pair of matching parentheses
  * start reversing from the innermost parentheses
  * remove all parentheses in the final result
- The input string is guaranteed to be balanced.
---

## 2. Problem Understanding

- Parentheses indicate a region that must be reversed
- Innermost parentheses are processed first
- After reversing, parentheses are removed
- Nested parentheses cause multiple reversals
- Key observation:
  * This is a nested structure problem → stack fits naturally.
---

## 3. Constraints

- 1 <= s.length <= 2000
- String is balanced
- Only lowercase letters and parentheses
---

## 4. Edge Cases

- No parentheses → return string as is
- Single pair of parentheses
- Deeply nested parentheses
- Consecutive parentheses
- Entire string inside parentheses
---

## 5. Examples

```text
Input:  (abcd)
Output: dcba


Input:  (accio(job))
Output: joboicca
```

---

## 6. Approaches

### Approach 1: Brute Force (Repeated String Reversal)

**Idea:**
- Find the innermost () pair
- Reverse the substring inside it
- Remove the parentheses
- Repeat until no parentheses remain

**Steps:**
- While string contains '(':
- Find the last '('
- Find the first ')' after it
- Reverse the substring in between
- Replace (sub) with reverse(sub)

**Java Code:**
```java
public String reverseBrute(String s) {
    while (s.contains("(")) {
        int l = s.lastIndexOf('(');
        int r = s.indexOf(')', l);
        String rev = new StringBuilder(s.substring(l + 1, r)).reverse().toString();
        s = s.substring(0, l) + rev + s.substring(r + 1);
    }
    return s;
}
```

**💭 Intuition Behind the Approach:**
- Innermost parentheses have no nested brackets inside
- Replacing them simplifies the problem step by step

**Complexity (Time & Space):**
- Time: O(N^2) — repeated substring operations
- Space: O(N) — string rebuilding

### Approach 2: Stack of Characters

**Idea:**
- Push characters into a stack
- On encountering ')', pop characters until '('
- Reverse popped characters and push back

**Steps:**
- Traverse string character by character
- Push characters onto stack
- When ')' is found:
  * pop until '('
  * reverse collected substring
  * push it back

**Java Code:**
```java
public String reverseStackChar(String s) {
    Stack<Character> st = new Stack<>();

    for (char c : s.toCharArray()) {
        if (c == ')') {
            StringBuilder sb = new StringBuilder();
            while (st.peek() != '(') {
                sb.append(st.pop());
            }
            st.pop(); // remove '('
            for (char ch : sb.toString().toCharArray()) {
                st.push(ch);
            }
        } else {
            st.push(c);
        }
    }

    StringBuilder res = new StringBuilder();
    while (!st.isEmpty()) res.append(st.pop());
    return res.reverse().toString();
}
```

**💭 Intuition Behind the Approach:**
- Stack naturally handles nested parentheses
- Characters inside parentheses are reversed using pop order

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N) — stack

### Approach 3: Stack of StringBuilder (Optimal & Clean)

**Idea:**
- Use a stack of StringBuilder
- Each ( starts a new context
- Each ) ends a context and reverses it

**Steps:**
- Maintain a StringBuilder curr
- On '(':
  * push curr onto stack
  * start new curr
- On ')':
  * reverse curr
  * pop previous string and append
- On letter:
  * append to curr

**Java Code:**
```java
public String reverseParentheses(String s) {
    Stack<StringBuilder> st = new Stack<>();
    StringBuilder curr = new StringBuilder();

    for (char ch : s.toCharArray()) {
        if (ch == '(') {
            st.push(curr);
            curr = new StringBuilder();
        } else if (ch == ')') {
            curr.reverse();
            StringBuilder prev = st.pop();
            prev.append(curr);
            curr = prev;
        } else {
            curr.append(ch);
        }
    }
    return curr.toString();
}
```

**💭 Intuition Behind the Approach:**
- Each parentheses level builds its own substring
- Reversal happens exactly once per pair
- Avoids repeated character-level operations

**Complexity (Time & Space):**
- Time: O(N) — each character processed once
- Space: O(N) — stack + builders

---

## 7. Justification / Proof of Optimality

- Nested structure demands stack usage
- StringBuilder avoids unnecessary string copying
- Clean, readable, and optimal
---

## 8. Variants / Follow-Ups

- Decode string problems
- Remove outer parentheses
- Reverse words inside brackets
- Expression parsing
---

## 9. Tips & Observations

- Always process innermost parentheses first
- Stack naturally enforces nesting order
- StringBuilder is faster than String concatenation
---

## 10. Pitfalls

- Forgetting to remove parentheses
- Reversing too early
- Using recursion unnecessarily
- Mishandling nested brackets
---

<!-- #endregion -->
<!-- #region 220-Sum of Subarray Minimums -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q224: Sum of Subarray Minimums</h1>

## 1. Problem Statement

- Sum of Subarray Minimums
- Given an integer array arr, find the sum of the minimum element of every contiguous subarray.
- Since the result can be large, return the answer modulo 10^9 + 7.
---

## 2. Problem Understanding

- You must consider all possible contiguous subarrays
- For each subarray, take its minimum element
- Add all those minimums together
- Brute force is infeasible due to large constraints
- Key realization:
  * Instead of iterating over all subarrays, count how many subarrays consider a given element as the minimum.
---

## 3. Constraints

- 1 <= n <= 3 * 10^4
- 1 <= arr[i] <= 3 * 10^4
- Answer must be returned modulo 10^9 + 7
---

## 4. Edge Cases

- Single element array
- Strictly increasing array
- Strictly decreasing array
- Duplicate values (important for monotonic stack logic)
---

## 5. Examples

```text
Example 1
Input

4
3 1 2 4
Output
17

Explanation

Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4].
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.

Example 2
Input

3
1 2 3
Output
10

Explanation

Subarrays are [1], [2], [3], [1,2], [2,3], [1,2,3].
Minimums are 1, 2, 3, 1, 2, 1.
Sum is 10.
```

---

## 6. Approaches

### Approach 1: Brute Force (TLE)

**Idea:**
- Generate all subarrays
- Find minimum of each
- Add them up

**Steps:**
- Fix starting index i
- Extend subarray till j
- Track minimum while extending

**Java Code:**
```java
public long minSubarraySum(int[] arr, int n) {
    long sum = 0;
    for (int i = 0; i < n; i++) {
        int min = Integer.MAX_VALUE;
        for (int j = i; j < n; j++) {
            min = Math.min(min, arr[j]);
            sum += min;
        }
    }
    return sum;
}
```

**💭 Intuition Behind the Approach:**
- Every subarray is explicitly evaluated, but this repeats work heavily.

**Complexity (Time & Space):**
- Time: O(N^2) — nested loops over all subarrays
- Space: O(1) — no extra space

### Approach 2: Contribution Technique + Previous/Next Smaller (Optimal)

**Idea:**
- Each element arr[i] contributes to the answer as:
  * arr[i] * (number of subarrays where arr[i] is the minimum)
- To compute this:
  * Find Previous Smaller Element (PSE)
  * Find Next Smaller Element (NSE)
- Then:
  * left  = i - PSE
  * right = NSE - i
  * contribution = arr[i] * left * right

**Steps:**
- Use monotonic increasing stack to find:
  * PSE (strictly smaller → >)
  * NSE (smaller or equal → >=)
- For each index:
  * Calculate how many subarrays take arr[i] as minimum
- Add contribution modulo 10^9 + 7

**Java Code:**
```java
public int minSubarraySum(int[] arr, int n) {
    int MOD = 1000000007;

    int[] pse = new int[n];
    int[] nse = new int[n];

    Stack<Integer> st = new Stack<>();

    // Previous Smaller Element
    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && arr[st.peek()] > arr[i]) {
            st.pop();
        }
        pse[i] = st.isEmpty() ? -1 : st.peek();
        st.push(i);
    }

    st.clear();

    // Next Smaller Element
    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
            st.pop();
        }
        nse[i] = st.isEmpty() ? n : st.peek();
        st.push(i);
    }

    long sum = 0;
    for (int i = 0; i < n; i++) {
        long left = i - pse[i];
        long right = nse[i] - i;
        sum = (sum + arr[i] * left % MOD * right % MOD) % MOD;
    }

    return (int) sum;
}
```

**💭 Intuition Behind the Approach:**
- Each element becomes the minimum for a range of subarrays
- PSE decides how far left it can dominate
- NSE decides how far right it can dominate
- Multiplying both gives the number of valid subarrays

**Complexity (Time & Space):**
- Time: O(N) — each element pushed and popped once
- Space: O(N) — stacks and auxiliary arrays

---

## 7. Justification / Proof of Optimality

- Brute force fails for large n
- Contribution technique converts the problem into counting dominance
- Monotonic stacks ensure linear time
- This is the standard optimal solution asked in interviews
---

## 8. Variants / Follow-Ups

- Sum of Subarray Maximums
- Largest Rectangle in Histogram
- Maximal Rectangle
- Minimum Cost Tree From Leaf Values
---

## 9. Tips & Observations

- Use > for PSE and >= for NSE to avoid double counting
- This is NOT a sliding window problem
- Always think in terms of element contribution, not subarrays
---

## 10. Pitfalls

- Using same comparison (> or >=) on both sides
- Forgetting modulo during multiplication
- Using absolute counts instead of distances
- Overwriting first occurrence in stack logic
- Treating it as brute-force sliding window
---

<!-- #endregion -->
<!-- #region 221-132 Pattern -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">225: 132 Pattern</h1>

## 1. Problem Statement

- Given an integer array nums of size n, determine if there exists a subsequence of three integers
- nums[i], nums[j], nums[k] such that:
  * i < j < k
  * nums[i] < nums[k] < nums[j]
- Return true if such a pattern exists, otherwise return false.
---

## 2. Problem Understanding

- We are not looking for contiguous elements → this is a subsequence
- Order matters: i < j < k
- Value condition defines the 132 shape:
  * nums[j] is the largest
  * nums[i] is the smallest
  * nums[k] lies between them
- Key difficulty:
  * Brute force checking all triplets is too slow for n = 2 * 10^5.
---

## 3. Constraints

- 1 <= n <= 2 * 10^5
- -10^9 <= nums[i] <= 10^9
- Need solution better than O(N^2)
---

## 4. Edge Cases

- Strictly increasing array → always false
- Strictly decreasing array → always false
- Presence of negative numbers
- Duplicate values
- Very large input size
---

## 5. Examples

```text
Example 1
Input

4
1 2 3 4
Output

false
Explanation

There is no 132 pattern in the sequence.

Example 2
Input

4
3 1 4 2
Output

true
Explanation

There is a 132 pattern in the sequence: [1, 4, 2]

Example 3
Input

4
-1 3 2 0
Output

true
Explanation

There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].
```

---

## 6. Approaches

### Approach 1: Brute Force (TLE)

**Idea:**
- Try all triplets (i, j, k)
- Check condition directly

**Steps:**
- Three nested loops
- Check i < j < k and value condition

**Java Code:**
```java
public boolean find132pattern(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                if (nums[i] < nums[k] && nums[k] < nums[j]) {
                    return true;
                }
            }
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Direct and obvious, but repeats massive work.

**Complexity (Time & Space):**
- Time: O(N^3) — impossible for large n
- Space: O(1)

### Approach 2: Prefix Min + Middle Check (Still TLE)

**Idea:**
- Fix j as the middle element
- Track smallest value on the left

**Steps:**
- Precompute minLeft[j]
- For each j, search k > j such that
- minLeft[j] < nums[k] < nums[j]

**Java Code:**
```java
public boolean find132pattern(int[] nums) {
    int n = nums.length;
    int[] minLeft = new int[n];
    minLeft[0] = nums[0];

    for (int i = 1; i < n; i++) {
        minLeft[i] = Math.min(minLeft[i - 1], nums[i]);
    }

    for (int j = 1; j < n; j++) {
        for (int k = j + 1; k < n; k++) {
            if (minLeft[j - 1] < nums[k] && nums[k] < nums[j]) {
                return true;
            }
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- We reduce one loop but still search linearly for k.

**Complexity (Time & Space):**
- Time: O(N^2) — still too slow
- Space: O(N)

### Approach 3: Monotonic Stack (Optimal)

**Idea:**
- Traverse from right to left and treat:
  * nums[j] → potential 3
  * Stack elements → potential 2
  * Maintain a variable third → best candidate for 2

**Steps:**
- Initialize third = -∞
- Traverse array from right to left
- If nums[i] < third → 132 found
- While stack top < nums[i]:
  * Update third = stack.pop()
- Push nums[i] to stack

**Java Code:**
```java
public boolean find132pattern(int[] nums) {
    int n = nums.length;
    Stack<Integer> st = new Stack<>();
    int third = Integer.MIN_VALUE;

    for (int i = n - 1; i >= 0; i--) {
        if (nums[i] < third) return true;

        while (!st.isEmpty() && st.peek() < nums[i]) {
            third = st.pop();
        }
        st.push(nums[i]);
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- We try to fix nums[j] first (largest value)
- Stack maintains decreasing order → possible nums[k]
- third stores the maximum valid nums[k] seen so far
- If we ever find nums[i] < third, pattern is complete

**Complexity (Time & Space):**
- Time: O(N) — each element pushed and popped once
- Space: O(N) — stack

---

## 7. Justification / Proof of Optimality

- Brute force and prefix-based approaches fail due to time
- Right-to-left monotonic stack cleverly enforces ordering
- This is the standard interview solution for 132 Pattern
---

## 8. Variants / Follow-Ups

- 231 Pattern
- 213 Pattern
- Increasing Triplet Subsequence
- Pattern detection using stacks
---

## 9. Tips & Observations

- Always think which index is fixed first
- Right-to-left traversal is the key insight
- Stack represents candidates for the 2 in 132
- third must be the largest possible valid middle
---

## 10. Pitfalls

- Iterating left-to-right (fails logic)
- Resetting third incorrectly
- Using stack for indices instead of values
- Confusing this with contiguous subarray problems
---

<!-- #endregion -->
<!-- #region 226-Height Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q226: Height Problem</h1>

## 1. Problem Statement

- n people are standing in a line from left to right.
- Each person wants to know the height of the closest person on the left who is strictly shorter than them.
- If there are multiple such people, choose the closest one.
- If no such person exists, return -1.
---

## 2. Problem Understanding

- For each index i:
  * Look only to the left (0 … i-1)
  * Among people with height < arr[i]
  * Pick the nearest one
  * Output their height, not index
- This is a Previous Smaller Element (PSE) problem.
---

## 3. Constraints

- 2 ≤ n ≤ 10^5
- 0 ≤ arr[i] ≤ 10^9
- Large n → brute force will TLE
---

## 4. Edge Cases

- First person → always -1
- Strictly increasing array
- Strictly decreasing array
- Equal heights (not allowed, must be less than)
---

## 5. Examples

```text
Input

5
1 2 3 4 5


Output

-1 1 2 3 4
```

---

## 6. Approaches

### Approach 1: Brute Force (Naive) 

**Idea:**
- For each person, scan leftwards until you find a shorter height.

**Steps:**
- For each i:
  * Start from j = i-1
  * Move left while arr[j] >= arr[i]
  * First arr[j] < arr[i] → answer
  * If none found → -1

**Java Code:**
```java
public int[] heightProblemBrute(int[] arr) {
    int n = arr.length;
    int[] ans = new int[n];

    for (int i = 0; i < n; i++) {
        ans[i] = -1;
        for (int j = i - 1; j >= 0; j--) {
            if (arr[j] < arr[i]) {
                ans[i] = arr[j];
                break;
            }
        }
    }
    return ans;
}
```

**Complexity (Time & Space):**
- Time: O(N^2) — nested loop, worst case decreasing array
- Space: O(1) — output array excluded

### Approach 2: Monotonic Stack (Optimal)

**Idea:**
- Maintain a monotonic increasing stack of heights.
- Stack invariant:
  * Stack stores candidates for previous smaller
  * Pop elements that are ≥ current height

**Steps:**
- Create empty stack
- Traverse array from left to right
- While stack is not empty and stack.peek() >= arr[i], pop
- If stack is empty → answer -1
- Else → answer stack.peek()
- Push arr[i] to stack

**Java Code:**
```java
public int[] heightProblem(int[] arr) {
    int n = arr.length;
    int[] ans = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && st.peek() >= arr[i]) {
            st.pop();
        }

        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(arr[i]);
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We only care about nearest smaller on the left
- Taller or equal people are useless for future elements
- Stack keeps only valid smaller candidates
- Each element is pushed and popped once

**Complexity (Time & Space):**
- Time: O(N) — each element pushed & popped once
- Space: O(N) — stack in worst case (increasing array)

---

## 7. Justification / Proof of Optimality

- The monotonic stack approach is optimal because:
  * Brute force fails for large n
  * Stack reduces redundant comparisons
  * Guarantees nearest smaller element efficiently
---

## 8. Variants / Follow-Ups

- Previous Greater Element
- Next Smaller Element
- Next Greater Element
- Stock Span Problem
- Largest Rectangle in Histogram
---

## 9. Tips & Observations

- Condition is strictly smaller (<)
- Use >= while popping to handle equals
- Output requires value, not index
---

## 10. Pitfalls

- Using > instead of >= in pop condition
- Scanning right side instead of left
- Confusing PSE with NSE
---

<!-- #endregion -->
<!-- #region 227-Asteroid Collision -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q227: Asteroid Collision</h1>

## 1. Problem Statement

- You are given an integer array asteroids representing asteroids in a row.
- The absolute value of asteroids[i] represents the size of the asteroid.
- The sign represents the direction:
  * Positive → moving right
  * Negative → moving left
- Asteroids collide based on the following rules:
  * If two asteroids of different sizes meet, the smaller one explodes
  * If two asteroids of equal sizes meet, both explode
  * Asteroids moving in the same direction never meet
- If no asteroid remains, print -1.
---

## 2. Problem Understanding

- Collision is possible only when:
  * A right-moving asteroid (+) is followed by a left-moving asteroid (-)
- We must simulate collisions in order
- Multiple collisions may occur for one asteroid
- This is a classic stack simulation problem.
---

## 3. Constraints

- 2 ≤ n ≤ 10^4
- -1000 ≤ asteroids[i] ≤ 1000
- asteroids[i] ≠ 0
- Efficient solution required → O(N)
---

## 4. Edge Cases

- All asteroids moving in same direction
- Chain collisions (one asteroid destroys multiple)
- Equal-sized collision
- Final result empty → print -1
---

## 5. Examples

```text
Input

3
5 10 -5


Output

5 10
```

---

## 6. Approaches

### Approach 1: Brute Force Simulation (Not Optimal)

**Idea:**
- Repeatedly scan the array and resolve one collision at a time until no more collisions are possible.
- This is slow but conceptually simple.

**Steps:**
- Convert array to list
- Scan from left to right
- If a[i] > 0 and a[i+1] < 0, resolve collision
- Modify list
- Restart scan
- Stop when no collision occurs

**Java Code:**
```java
public static List<Integer> asteroidCollisionBrute(int[] asteroids) {
    List<Integer> list = new ArrayList<>();
    for (int a : asteroids) list.add(a);

    boolean changed = true;

    while (changed) {
        changed = false;
        for (int i = 0; i < list.size() - 1; i++) {
            int a = list.get(i);
            int b = list.get(i + 1);

            if (a > 0 && b < 0) {
                if (Math.abs(a) > Math.abs(b)) {
                    list.remove(i + 1);
                } else if (Math.abs(a) < Math.abs(b)) {
                    list.remove(i);
                } else {
                    list.remove(i + 1);
                    list.remove(i);
                }
                changed = true;
                break;
            }
        }
    }

    return list.size() == 0 ? Arrays.asList(-1) : list;
}
```

**Complexity (Time & Space):**
- Time: O(N^2) — repeated scans
- Space: O(N) — list storage

### Approach 2: Stack Simulation (Optimal)

**Idea:**
- Use a stack to simulate collisions efficiently.
- Only collide when:
  * stack.peek() > 0 && current < 0

**Steps:**
- Initialize an empty stack
- Traverse asteroids from left to right
- For each asteroid a:
  * Assume it is alive
  * While:
    * stack is not empty
    * stack top is > 0
    * current asteroid a < 0
    * → collision is possible
- Compare sizes:
  * If |stack.peek()| < |a|
  * → stack top explodes (pop)
  * If |stack.peek()| == |a|
  * → both explode (pop + discard a)
  * If |stack.peek()| > |a|
  * → current asteroid explodes
- If current asteroid survives all collisions → push it into stack
- Stack contents after traversal represent final state

**Java Code:**
```java
public int[] asteroidCollision(int[] asteroids) {
    Stack<Integer> st = new Stack<>();

    for (int a : asteroids) {
        boolean alive = true;

        while (alive && !st.isEmpty() && st.peek() > 0 && a < 0) {
            if (Math.abs(st.peek()) < Math.abs(a)) {
                st.pop();
            } else if (Math.abs(st.peek()) == Math.abs(a)) {
                st.pop();
                alive = false;
            } else {
                alive = false;
            }
        }

        if (alive) st.push(a);
    }

    if (st.isEmpty()) return new int[]{-1};

    int[] res = new int[st.size()];
    for (int i = st.size() - 1; i >= 0; i--) {
        res[i] = st.pop();
    }
    return res;
}
```

**💭 Intuition Behind the Approach:**
- Stack stores asteroids that haven’t collided yet
- Only the nearest previous asteroid can collide with the current one
- Right-moving asteroids wait on the stack
- Left-moving asteroids may cause chain reactions
- Each asteroid is:
  * pushed once
  * popped at most once
- This guarantees linear processing.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Time: O(N) —
  * Each asteroid is pushed and popped at most once, so total operations are linear.
- 💾 Space Complexity
  * Space: O(N) —
  * Stack can store all asteroids in the worst case (all moving in same direction).

### Approach 3: Push-Then-Resolve Stack

**Idea:**
- Push current asteroid first
- Then repeatedly check top two elements
- Resolve collision immediately if needed

**Java Code:**
```java
public static List<Integer> asteroidCollision(int[] asteroids) {
    Stack<Integer> s = new Stack<>();

    for (int z : asteroids) {
        s.push(z);

        while (s.size() > 1) {
            int as2 = s.pop();
            int as1 = s.pop();

            if (!(as1 > 0 && as2 < 0)) {
                s.push(as1);
                s.push(as2);
                break;
            } else {
                int m = Math.abs(as1);
                int n = Math.abs(as2);

                if (m == n) {
                    break; // both destroyed
                } else if (m < n) {
                    s.push(as2);
                } else {
                    s.push(as1);
                }
            }
        }
    }

    List<Integer> res = new ArrayList<>();
    while (!s.isEmpty()) res.add(s.pop());
    Collections.reverse(res);

    return res.size() == 0 ? Arrays.asList(-1) : res;
}
```

**💭 Intuition Behind the Approach:**
- Stack always stores valid asteroids
- Collision is checked only for adjacent top elements
- Ensures nearest collision first
- Same O(N) guarantee

**Complexity (Time & Space):**
- Time: O(N) — each asteroid pushed/popped once
- Space: O(N) — stack storage

---

## 7. Justification / Proof of Optimality

- Brute force helps understand collisions
- Stack optimizes collision handling
- Your solution is a clean alternate stack formulation
---

## 8. Variants / Follow-Ups

- Circular asteroid collision
- Asteroids with velocity
- 2D collision simulation
---

## 9. Tips & Observations

- Collision condition is direction-based
- Always compare absolute values
- Chain collisions are the tricky part
---

## 10. Pitfalls

- Missing equal-size case
- Allowing same-direction collision
- Forgetting -1 when empty
---

<!-- #endregion -->
