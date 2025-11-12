<!-- #region Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: Linked List</h1>

## 1. Problem Understanding

- A Linked List is a linear data structure where elements (called nodes) are stored in separate memory locations.
- Each node is connected using a reference (pointer) to the next node.
- Unlike arrays, elements in a linked list are not stored in contiguous memory.
- It solves the problem of fragmented memory by dynamically allocating nodes.

- **Intuition Behind the Concept**
    - Imagine a chain of people: each person holds their own data and the hand of the next person.
    - This allows flexibility — people (nodes) can stand anywhere, yet still stay connected.
    - The chain starts at the head — the only way to access everyone else.

- **Node Structure**
     ``` 
    class Node {
         int data;     // stores data
         Node next;    // stores address of next node
     }
     ```
    - Each node = data + reference to next node.
    - The linked list = collection of connected nodes.
    - Size of linked list = number of nodes.

- **Core Terms**
    - Head → First node of the linked list. Access point for the entire list.
    - Tail → Last node, whose next is null (or head, in circular lists).
    - Current → A temporary pointer used to traverse the list safely.
    - Size → Total number of nodes in the list.
    - Always remember:
      * We never move the head directly.
      * Instead, create a pointer like Node current = head; and move it.

- **Types of Linked Lists**
    - Singly Linked List (SLL)
      * Each node points to the next node only.
      * Can move in one direction (forward).
      * The last node’s next pointer is null.
    - Doubly Linked List (DLL)
      * Each node contains both next and previous references.
      * Can move in both directions (forward and backward).
      * Head’s previous is null; tail’s next is null.
    - Circular Linked List (CLL)
      * The last node points back to the head.
      * No node has a null reference.
      * Traversal loops continuously.

- **Why Use Linked Lists**
    - Supports dynamic memory allocation.
    - Insertion and deletion are efficient (no shifting like arrays).
    - Useful when frequent insertions/deletions are needed.
    - Can grow or shrink at runtime.

- **Strategy to Solve Linked List Questions**
    - Always draw a general diagram of 4–5 nodes.
    - Dry run pointer movement step by step.
    - Always handle two edge cases:
      * Empty list → if (head == null)
      * Single node → if (head.next == null)
    - Use separate pointers (current, prev, next) for clarity.
    - Preserve head to maintain access to the list.
---

## 2. Edge Cases

- If list is empty → head == null.
- If list has one node → head.next == null.
- Always check for null before accessing next or prev.
- Do not move the head; always use a separate pointer (current).
---

## 3. Approaches

### Approach 1: Traversal

**Java Code:**
```java
Singly Linked List

Node current = head;
while (current != null) {
    System.out.print(current.data + " ");
    current = current.next;
}


Doubly Linked List

Node current = head;
while (current != null) {
    System.out.print(current.data + " ");
    current = current.next;
}


Circular Linked List

Node current = head;
if (head != null) {
    do {
        System.out.print(current.data + " ");
        current = current.next;
    } while (current != head);
}
```

### Approach 2: Insertion

**Java Code:**
```java
Singly Linked List

Insert at beginning:

Node newNode = new Node(data);
newNode.next = head;
head = newNode;


Insert at end:

Node newNode = new Node(data);
if (head == null) { head = newNode; return; }
Node current = head;
while (current.next != null)
    current = current.next;
current.next = newNode;


Insert at specific position:

Node current = head;
for (int i = 1; i < pos - 1 && current != null; i++)
    current = current.next;
newNode.next = current.next;
current.next = newNode;


Doubly Linked List

Insert at beginning:

Node newNode = new Node(data);
newNode.next = head;
if (head != null) head.prev = newNode;
head = newNode;


Insert at end:

Node newNode = new Node(data);
Node current = head;
while (current.next != null)
    current = current.next;
current.next = newNode;
newNode.prev = current;


Insert at specific position:

Node current = head;
for (int i = 1; i < pos - 1; i++)
    current = current.next;
newNode.next = current.next;
if (current.next != null) current.next.prev = newNode;
current.next = newNode;
newNode.prev = current;


Circular Linked List

Insert at beginning:

Node newNode = new Node(data);
if (head == null) {
    head = newNode;
    head.next = head;
} else {
    Node temp = head;
    while (temp.next != head)
        temp = temp.next;
    temp.next = newNode;
    newNode.next = head;
    head = newNode;
}


Insert at end:

Node newNode = new Node(data);
if (head == null) {
    head = newNode;
    head.next = head;
} else {
    Node temp = head;
    while (temp.next != head)
        temp = temp.next;
    temp.next = newNode;
    newNode.next = head;
}
```

### Approach 3: Deletion

**Java Code:**
```java
Singly Linked List

Delete at beginning:

if (head == null) return;
head = head.next;


Delete at end:

if (head == null || head.next == null) { head = null; return; }
Node current = head;
while (current.next.next != null)
    current = current.next;
current.next = null;


Delete by value:

Node current = head, prev = null;
if (current != null && current.data == key) { head = current.next; return; }
while (current != null && current.data != key) {
    prev = current;
    current = current.next;
}
if (current == null) return;
prev.next = current.next;


Doubly Linked List

Delete at beginning:

if (head == null) return;
head = head.next;
if (head != null) head.prev = null;


Delete at end:

if (head == null) return;
Node current = head;
while (current.next != null)
    current = current.next;
if (current.prev != null)
    current.prev.next = null;
else head = null;


Delete by value:

Node current = head;
while (current != null && current.data != key)
    current = current.next;
if (current == null) return;
if (current.prev != null) current.prev.next = current.next;
else head = current.next;
if (current.next != null) current.next.prev = current.prev;


Circular Linked List

Delete head node:

if (head == null) return;
if (head.next == head) { head = null; return; }
Node temp = head;
while (temp.next != head)
    temp = temp.next;
temp.next = head.next;
head = head.next;
```

### Approach 4: Interview Patterns on Linked List

**Java Code:**
```java
1️⃣ Reverse a Linked List

Iterative Approach

Node prev = null, current = head, next = null;
while (current != null) {
    next = current.next;
    current.next = prev;
    prev = current;
    current = next;
}
head = prev;


Intuition: Reverse the link direction step-by-step.
Complexity: Time O(N), Space O(1).

2️⃣ Find Middle Element

Two-pointer approach

Node slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
System.out.println("Middle Node: " + slow.data);


Intuition: Fast moves twice as quickly as slow; when fast reaches the end, slow is at the middle.
Complexity: Time O(N), Space O(1).

3️⃣ Detect Loop in Linked List (Floyd’s Cycle Detection)
Node slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) {
        System.out.println("Loop detected");
        return;
    }
}
System.out.println("No loop");


Intuition: If a loop exists, slow and fast will eventually meet.
Complexity: Time O(N), Space O(1).

4️⃣ Remove Loop in Linked List
Node slow = head, fast = head;
boolean loop = false;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) { loop = true; break; }
}
if (loop) {
    slow = head;
    while (slow.next != fast.next) {
        slow = slow.next;
        fast = fast.next;
    }
    fast.next = null;
}


Intuition: Reset one pointer to head; move both one step until they meet at loop start, then break the loop.

5️⃣ Remove Duplicates (from Sorted Linked List)
Node current = head;
while (current != null && current.next != null) {
    if (current.data == current.next.data)
        current.next = current.next.next;
    else
        current = current.next;
}


Intuition: Skip nodes that have the same data consecutively.
Complexity: Time O(N), Space O(1).

6️⃣ Find Nth Node from End

Two-pointer approach

Node first = head, second = head;
for (int i = 0; i < n; i++) {
    if (first == null) return;
    first = first.next;
}
while (first != null) {
    first = first.next;
    second = second.next;
}
System.out.println("Nth node from end: " + second.data);


Intuition: Maintain a gap of n between two pointers.
Complexity: Time O(N), Space O(1).
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity Summary
  * Traversal → O(N)
  * Insertion (head) → O(1)
  * Insertion (end) → O(N)
  * Deletion (head) → O(1)
  * Deletion (end) → O(N)
  * Search → O(N)
  * Reversal → O(N)
  * Loop detection → O(N)
  * Middle element → O(N)
- 💾 Space Complexity
  * Most operations use only pointer variables → O(1)
  * The list itself uses O(N) memory for N nodes.

---

## 4. Tips & Observations

- Visualize every pointer change.
- Always protect head.
- Watch out for NullPointerException.
- Handle empty and single-node lists.
- Use the two-pointer technique for efficient traversal-based questions.
- Draw, dry run, and code — in that order.

- **Summary**
    - Linked List = dynamic, flexible, pointer-based structure.
    - Node = data + next (and sometimes prev).
    - Head = entry point to entire list.
    - Use current pointer for traversal.
    - Always check for nulls and edge cases.
    - Know the main types:
      * Singly Linked List → forward only
      * Doubly Linked List → both directions
      * Circular Linked List → continuous loop
    - Master key patterns:
      * Reversal
      * Middle element
      * Loop detection & removal
      * Remove duplicates
      * Nth node from end
---

<!-- #endregion -->
