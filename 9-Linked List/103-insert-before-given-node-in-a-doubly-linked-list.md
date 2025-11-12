<!-- #region 103-Insert Before Given Node in a Doubly Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q103: Insert Before Given Node in a Doubly Linked List</h1>

## 1. Problem Understanding

- You are given a reference to a node in a Doubly Linked List (DLL) and an integer X.
- You need to insert a new node with value X before the given node, while maintaining the DLL’s bidirectional linkage integrity.
- You are not given the head of the list, and it is guaranteed that the given node is not the head of the list.
---

## 2. Constraints

- 2 <= n <= 100 (where n = number of nodes in the DLL)
- 0 <= ListNode.val <= 100
- 0 <= X <= 100
- The given node will always exist in the DLL.
- The given node will never be the head (so node.prev != null).
---

## 3. Edge Cases

- The given node is the tail → still valid, should insert before it.
- DLL has only two nodes → ensure correct re-linking of both sides.
- Multiple nodes have the same value → insertion depends on the node reference, not the value.
---

## 4. Examples

```text
Example 1

Input:
head -> 1 <-> 2 <-> 6, node = 2, X = 7
Output:
head -> 1 <-> 7 <-> 2 <-> 6
Explanation:
The node 7 is inserted before node 2.

Example 2

Input:
head -> 7 <-> 5 <-> 5, node = 5 (last one), X = 10
Output:
head -> 7 <-> 5 <-> 10 <-> 5
Explanation:
The second 5 (the last node) is referenced; new node 10 is inserted before it.

Example 3

Input:
head -> 7 <-> 6 <-> 5, node = 5, X = 10
Output:
head -> 7 <-> 6 <-> 10 <-> 5
Explanation:
Node 10 inserted before node 5.
```

---

## 5. Approaches

### Approach 1: Explicit Pointer Linking

**Idea:**
- We already have a reference to the node.
- To insert before it, we just need to connect the new node between node.prev and node.
- This requires adjusting four pointers.

**Steps:**
- Store prevNode = node.prev.
- Create the new node.
- Set the links:
  * newNode.prev = prevNode
  * newNode.next = node
- Adjust the existing neighbors:
  * prevNode.next = newNode
  * node.prev = newNode

**Java Code:**
```java
class Solution {
    public void insertBeforeGivenNode(ListNode node, int X) {
        if (node == null || node.prev == null) return; // safety check
        
        ListNode newNode = new ListNode(X);
        ListNode prevNode = node.prev;

        newNode.prev = prevNode;
        newNode.next = node;
        prevNode.next = newNode;
        node.prev = newNode;
    }
}
```

**💭 Intuition Behind the Approach:**
- In a DLL, each node maintains pointers in both directions.
- To insert before a node, we only need to reconnect four links around the insertion point — no traversal or head access is required.
- This makes the operation constant time (O(1)) and very efficient.

**Complexity (Time & Space):**
- Time Complexity: O(1)
- Space Complexity: O(1)

### Approach 2: Using Constructor for Cleaner Code

**Idea:**
- If the ListNode class provides a constructor with (int val, ListNode next, ListNode prev),
- we can simplify the code by assigning links directly during node creation.

**Steps:**
- Store prevNode = node.prev.
- Create a new node using the parameterized constructor:
  * ListNode newNode = new ListNode(X, node, prevNode);
- Update only the neighboring pointers:
  * prevNode.next = newNode
  * node.prev = newNode

**Java Code:**
```java
class Solution {
    public void insertBeforeGivenNode(ListNode node, int X) {
        ListNode prev = node.prev;
        ListNode newNode = new ListNode(X, node, prev);
        prev.next = newNode;
        node.prev = newNode;
    }
}
```

**💭 Intuition Behind the Approach:**
- This is a more compact version of the explicit pointer approach.
- The constructor sets up both links immediately, reducing boilerplate while maintaining correctness.
- It’s ideal for clean, production-level code when the constructor is available.

**Complexity (Time & Space):**
- Time Complexity: O(1)
- Space Complexity: O(1)

---

## 6. Justification / Proof of Optimality

- Both approaches perform only local pointer updates — no traversal or data shifting.
- The correctness follows from maintaining DLL bidirectional linkage (prev and next) consistently for all involved nodes.
---

## 7. Variants / Follow-Ups

- Insert After Given Node in DLL → Similar logic but swap pointer direction.
- Insert Before Head (with head reference) → Special handling since node.prev == null.
- Insert at Given Position (1-based index) → Requires traversal to that position first.
---

## 8. Tips & Observations

- Always ensure both prev and next links are updated symmetrically.
- Avoid accessing node.prev without checking null — unless guaranteed not to be head.
- Prefer constructor-based shorthand when supported, but show pointer logic explicitly in interviews.
- All insertions in DLL can be done in constant time when node reference is available.
---

<!-- #endregion -->
