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
