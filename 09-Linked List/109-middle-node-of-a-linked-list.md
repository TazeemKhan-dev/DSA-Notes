<!-- #region 109-Middle Node of a Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q109: Middle Node of a Linked List</h1>

## 1. Problem Understanding

- You are given the head of a singly linked list.
- Your task: Return the middle node.
- Rules:
  * If n is odd → return the exact middle
  * If n is even → return the second middle
  * (Example: [5, 4, 3, 2] → two middles: 4, 3 → return 3)
- Driver code will print the linked list starting from the returned middle node.
---

## 2. Constraints

- 1 ≤ n ≤ 10^5
- Node values are integers
- Linked list is singly linked
- Function should run in linear time
---

## 3. Edge Cases

- Single node → return head
- Two nodes → return second node
- Even sized list → return n/2 + 1 node
- Long list (up to 1e5 nodes) → recursion should be avoided
- List containing duplicates
---

## 4. Examples

```text
Example 1

Input:

5 4 3 2


Output:

3 2

Example 2

Input:

5 7 1


Output:

7 1
```

---

## 5. Approaches

### Approach 1: Slow & Fast Pointers (Optimal)

**Idea:**
- Use two pointers:
  * slow moves one step
  * fast moves two steps
- When fast reaches the end:
- → slow is at the middle.
- This automatically returns the second middle for even-length lists.

**Steps:**
- Initialize:
- slow = head
- fast = head
- While fast != null and fast.next != null:
- slow = slow.next
- fast = fast.next.next
- When loop ends → slow is middle
- Return slow

**Java Code:**
```java
static Node midpointOfLinkedList(Node head) {
    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;   
}
```

**💭 Intuition Behind the Approach:**
- Fast moves twice as fast.
- So when fast finishes the list:
  * If odd length → slow is exactly middle
  * If even length → slow is second middle
- This matches the required behavior perfectly.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
    * Every iteration advances fast by 2 and slow by 1.
    * Together, they traverse at most n steps.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only two pointers used, regardless of list size.

### Approach 2: Count Length + Traverse (Two-pass Method)

**Idea:**
- Traverse list to calculate length L
- Middle index = L/2 (integer division, gives second middle)
- Traverse again to reach middle

**Steps:**
- Count nodes
- Stop at index L/2
- Return that node

**Java Code:**
```java
static Node midpointOfLinkedList(Node head) {
    int length = 0;
    Node curr = head;

    while (curr != null) {
        length++;
        curr = curr.next;
    }

    int midIndex = length / 2;

    curr = head;
    for (int i = 0; i < midIndex; i++) {
        curr = curr.next;
    }

    return curr;
}
```

**💭 Intuition Behind the Approach:**
- Straightforward method:
- Just get total length and go to L/2 index.
- Simple but requires two complete passes.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) for counting length
  * O(n) for reaching middle
  * Total: O(n)
  * Why?
    * Two linear passes over list.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only counters and pointers used.

---

## 6. Justification / Proof of Optimality

- The fast & slow pointer approach is the optimal method because:
- It finishes in a single pass
- It uses constant memory
- It naturally returns the second middle
- Works for large inputs (n = 100000) without stack risk
- This is the approach expected in interviews.
---

## 7. Variants / Follow-Ups

- Return the first middle for even length
- Find the node before middle
- Delete the middle node
- Find middle in circular linked list
- Get the middle element at each insertion (useful for practice problems)
---

## 8. Tips & Observations

- Always prefer fast & slow pointers for mid-related linked list problems
- Avoid recursion for deep lists
- For even length lists, common definition is to return second middle
- Two-pointer technique is also used in:
  * Cycle detection
  * Palindrome check
  * Finding kth element from end
---

<!-- #endregion -->
