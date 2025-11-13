<!-- #region 108-Remove Nth Node From End of List, -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q108: Remove Nth Node From End of List,</h1>

## 1. Problem Understanding

- You are given a singly linked list and an integer n.
- Your task: Remove the nth node from the end and return the new head of the list.
- Key Details:
  * The list has at least 1 node.
  * Removing the nth from end can also mean removing the head (if n == length).
  * Problem expects pointer manipulation, not converting to array.
---

## 2. Constraints

- 1 ≤ k ≤ 30 (length of list)
- 1 ≤ Node.val < 100
- 1 ≤ n ≤ k
- List is singly linked
---

## 3. Edge Cases

- Remove the first node (if n == k)
- Remove the last node (n == 1)
- Single node list (k = 1)
- Removing a middle node
- Removing when duplicates exist
---

## 4. Examples

```text
Example 1

Input:

1 2 3 4 5 6
n = 2


Output:

1 2 3 4 6


Explanation: 2nd from end = value 5 → remove it.

Example 2

Input:

7 6 5 4 3
n = 4


Output:

7 5 4 3


Explanation: 4th from end = value 6 → remove it.
```

---

## 5. Approaches

### Approach 1: Two Pointer Technique (Optimal Approach)

**Idea:**
- Use two pointers:
- Move fast pointer n steps ahead
- Then move slow and fast together until fast reaches end
- slow will be just before the node to delete
- This avoids counting the total length.

**Steps:**
- Create a dummy node before head
- Move fast pointer n + 1 steps
- Move both slow and fast until fast == null
- Now slow.next is the node to remove
- Do slow.next = slow.next.next

**Java Code:**
```java
static Node removeNthFromEnd(Node head, int n) {
    Node dummy = new Node(-1);
    dummy.next = head;

    Node fast = dummy;
    Node slow = dummy;

    // Move fast n+1 steps
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }

    // Move both until fast reaches end
    while (fast != null) {
        fast = fast.next;
        slow = slow.next;
    }

    // Delete node
    slow.next = slow.next.next;

    return dummy.next;
}
```

**💭 Intuition Behind the Approach:**
- If fast is n nodes ahead of slow,
- when fast reaches the end,
- slow will be exactly at the node before the one to remove.
- This eliminates the need for an explicit length calculation.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(k)
  * Why?
    * Both pointers traverse the list once.
    * No extra nested loops.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only a few pointers are used regardless of list size.

### Approach 2: Length Counting (Simpler but 2-pass)

**Idea:**
- Traverse the list to count total length
- Compute position from front: pos = length - n
- Traverse again and delete that node

**Steps:**
- First pass: count nodes
- If removing head → return head.next
- Second pass: stop at pos-1
- Remove using pointer skip

**Java Code:**
```java
static Node removeNthFromEnd(Node head, int n) {
    int length = 0;
    Node curr = head;

    while (curr != null) {
        length++;
        curr = curr.next;
    }

    // Remove head case
    if (n == length) return head.next;

    curr = head;
    int steps = length - n - 1;

    for (int i = 0; i < steps; i++) {
        curr = curr.next;
    }

    curr.next = curr.next.next;
    return head;
}
```

**💭 Intuition Behind the Approach:**
- This method directly calculates the index to remove.
- It’s straightforward but requires two complete passes.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(k) for counting
  * O(k) for deleting
  * Total: O(k)
  * Why?
    * Two linear passes → still linear.
- 💾 Space Complexity
  * O(1)
    * Why?
      * Only counters and pointers, no extra structures.

---

## 6. Justification / Proof of Optimality

- The two-pointer technique is the optimal solution because:
  * It completes in one pass
  * Uses constant memory
  * Works for all cases including removing the head
  * Cleaner and preferred in interviews
---

## 7. Variants / Follow-Ups

- Remove nodes with a given value
- Remove duplicates in sorted list
- Remove middle node
- Remove nth node from start
- Remove kth node using recursion
- Remove last occurrence of a value
- Delete node in circular linked list
---

## 8. Tips & Observations

- Always use a dummy node to avoid special cases
- Two-pointer approach is the standard interview solution
- Avoid stack/recursion due to unnecessary memory
- After deleting, check if list becomes empty
---

<!-- #endregion -->
