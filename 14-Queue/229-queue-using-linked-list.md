<!-- #region 229-Queue Using Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q229: Queue Using Linked List</h1>

## 1. Problem Statement

- Implement a Queue using a Linked List that supports the following operations:
  * push(x) → Insert integer x at the rear of the queue
  * pop() → Remove and return the front element of the queue
  * front() → Return the front element without removing it
  * size() → Return the number of elements currently in the queue
- All operations must work efficiently.
---

## 2. Problem Understanding

- A Queue follows the FIFO (First In First Out) principle.
- Using a Linked List:
- Each node stores data and a reference to the next node
- We maintain:
    * front → points to first element
    * rear → points to last element
  * Insertions happen at rear
  * Deletions happen at front
- 👉 Unlike array-based queues, no fixed size and no shifting needed.
---

## 3. Constraints

- At most 1000 elements in the queue
- Operations belong to {1, 2, 3, 4}
- All operations should run in O(1)
---

## 4. Edge Cases

- Pop on empty queue
- Front on empty queue
- Queue becomes empty after pop
- First insertion into empty queue
---

## 5. Examples

```text
Operations:
push(1) → [1]
push(2) → [1, 2]
push(3) → [1, 2, 3]
front() → 1
pop()   → removes 1 → [2, 3]
front() → 2
size()  → 2
```

---

## 6. Approaches

### Approach 1: Queue Using Linked List (Optimal)

**Idea:**
- Maintain front and rear pointers
- Insert at rear in O(1)
- Remove from front in O(1)
- Update both pointers correctly when queue becomes empty

**Java Code:**
```java
class Queue {
    class Node {
        int data;
        Node next;
        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    Node front, rear;
    int size;

    Queue() {
        front = null;
        rear = null;
        size = 0;
    }

    void push(int x) {
        Node newNode = new Node(x);

        if (rear == null) {
            front = rear = newNode;
        } else {
            rear.next = newNode;
            rear = newNode;
        }
        size++;
    }

    int pop() {
        if (front == null) return -1;

        int val = front.data;
        front = front.next;

        if (front == null) {
            rear = null;
        }

        size--;
        return val;
    }

    int front() {
        if (front == null) return -1;
        return front.data;
    }

    int size() {
        return size;
    }
}
```

**Complexity (Time & Space):**
- Time: O(1) — insert and delete using pointers only
- Space: O(N) — one node per element

---

## 7. Justification / Proof of Optimality

- No space wastage as in array queue
- No overflow until memory is exhausted
- Constant time for all queue operations
- Clean and interview-preferred implementation
---

## 8. Variants / Follow-Ups

- Queue using Array
- Circular Queue
- Deque using Linked List
---

## 9. Tips & Observations

- Always maintain both front and rear
- When queue becomes empty, set both to null
- Linked List queue is better when size is unknown
---

## 10. Pitfalls

- Forgetting to update rear when last element is removed
- Returning wrong value on empty pop
- Not maintaining size correctly
---

<!-- #endregion -->
