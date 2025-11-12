<!-- #region 102-Delete Kth Element of a Doubly Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q102: Delete Kth Element of a Doubly Linked List</h1>

## 1. Problem Understanding

- You are given the head of a doubly linked list (DLL) and an integer k.
- Your task is to delete the node at the kth position (1-based index) and return the head of the modified DLL.
---

## 2. Constraints

- 1 <= n <= 100 (where n = number of nodes)
- 0 <= Node.val <= 100
- 1 <= k <= n
---

## 3. Edge Cases

- k = 1 → delete the head node (head must be updated).
- k = n → delete the last node (tail must be updated).
- List has only one node → after deletion, list becomes empty (head = null).
- Middle node deletion → need to adjust both prev and next links.
---

## 4. Examples

```text
Example 1

Input:
head -> 2 <-> 5 <-> 7 <-> 9, k = 2
Output:
head -> 2 <-> 7 <-> 9
Explanation: Node with value 5 (2nd node) is deleted.

Example 2

Input:
head -> 2 <-> 5 <-> 7, k = 1
Output:
head -> 5 <-> 7
Explanation: Node with value 2 is deleted (head updated).

Example 3

Input:
head -> 2 <-> 5 <-> 7, k = 3
Output:
head -> 2 <-> 5
Explanation: Node with value 7 (tail) is deleted.
```

---

## 5. Approaches

### Approach 1: Iterative Traversal

**Idea:**
- Traverse from the head to reach the kth node.
- Once found:
  * If it's the head → update head = head.next.
  * If it has a previous node → update prev.next.
  * If it has a next node → update next.prev.
- Return the new head.

**Steps:**
- If head == null, return null.
- Traverse the list until count == k.
- When you find the node:
  * If it's head → move head to head.next.
  * If it has a prev → link prev.next = curr.next.
  * If it has a next → link next.prev = curr.prev.
- Return the updated head.

**Java Code:**
```java
Node deleteNode(Node head, int k) {
    if (head == null) return null;

    Node curr = head;
    int count = 1;

    // Traverse to kth node
    while (curr != null && count < k) {
        curr = curr.next;
        count++;
    }

    // If node to delete doesn't exist
    if (curr == null) return head;

    // If deleting head node
    if (curr.prev == null) {
        head = curr.next;
        if (head != null) head.prev = null;
    } 
    // If deleting middle or last node
    else {
        curr.prev.next = curr.next;
        if (curr.next != null) {
            curr.next.prev = curr.prev;
        }
    }

    return head;
}

🌲 Recursion Tree (Conceptual)

This problem is primarily iterative, but conceptually:
If you imagined recursion, it would traverse nodes until k == 1, then delete and backtrack.
For head -> 2 <-> 5 <-> 7, k = 2:

delete(2, k=2)
 └── delete(5, k=1) → removes node 5
Backtrack → fix links
Result: 2 <-> 7
```

**💭 Intuition Behind the Approach:**
- In a doubly linked list, every node maintains pointers to both its previous and next nodes.
- To delete the kth node:
- Traverse to the kth node.
- Update its neighbors’ pointers:
  * prev.next = next
  * next.prev = prev
- Handle special cases when the node is head or tail.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Traversal to kth node → O(k)
  * Pointer updates → O(1)
  * ➡️ Total: O(k)
- 💾 Space Complexity
  * No extra memory used → O(1)

---

## 6. Justification / Proof of Optimality

- Deletion in DLL is efficient since both directions are accessible.
- No extra data structures are needed.
- Updates are O(1) after reaching the node.
---

## 7. Variants / Follow-Ups

- Delete all occurrences of a given value.
- Delete last node or middle node dynamically.
- Delete node when pointer to the node (not head) is given.
---

## 8. Tips & Observations

- Always handle head and tail cases separately.
- Don’t forget to update both prev and next pointers.
- In interviews, emphasize constant space and clean pointer handling.
---

<!-- #endregion -->
