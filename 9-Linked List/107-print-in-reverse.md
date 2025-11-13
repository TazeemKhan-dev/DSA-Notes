<!-- #region 107-Print in Reverse -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q107: Print in Reverse</h1>

## 1. Problem Understanding

- Given the head of a singly linked list, reverse the list and return the new head.
- Example:
- Input: 1 → 2 → 3 → 4 → 5
- Output: 5 → 4 → 3 → 2 → 1
- Important:
  * The list must be modified in-place
  * No new nodes should be created
  * Return the new head of the reversed list
---

## 2. Constraints

- Number of nodes: 1 ≤ n ≤ 5000
- Node values are integers
- The list is singly linked
---

## 3. Edge Cases

- Single node list → return same node
- Empty list → return null
- Already reversed pattern doesn’t matter
- Large list (iterative preferred over recursion due to stack depth)
---

## 4. Examples

```text
Example 1

Input:
2 6 8 10 1
Output:
1 10 8 6 2

Example 2

Input:
1 2 3 4 5 6
Output:
6 5 4 3 2 1
```

---

## 5. Approaches

### Approach 1: Iterative Reversal (MOST OPTIMAL)

**Idea:**
- Use 3 pointers: prev, curr, next.
- Reverse each link one by one.

**Steps:**
- Initialize:
  * prev = null
  * curr = head
- For each node:
  * Store next → next = curr.next
  * Reverse pointer → curr.next = prev
  * Move prev → curr
  * Move curr → next
- When loop finishes, prev is the new head.

**Java Code:**
```java
static Node reverse(Node head) {
    Node prev = null;
    Node curr = head;

    while (curr != null) {
        Node next = curr.next;  // store next
        curr.next = prev;       // reverse pointer
        prev = curr;            // move prev
        curr = next;            // move curr
    }

    return prev; // new head
}
```

**💭 Intuition Behind the Approach:**
- We walk through the list once.
- Each link is reversed so that direction becomes backward.
- At the end, the last node becomes the new head.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
  * Each of the n nodes is visited exactly once.
- 💾 Space Complexity
  * O(1)
  * Why?
  * Only uses constant extra pointers (prev, curr, next).

### Approach 2: Recursion (Elegant but Not Safe for Large n)

**Idea:**
- Break the list into two parts:
  * Reverse the rest of the list
  * Attach the first node at the end

**Steps:**
- Base case:
  * If head is null or head.next == null → return head
- Recursively reverse the remaining list
- Point head.next.next = head
- Set head.next = null
- Return new head from recursion

**Java Code:**
```java
static Node reverse(Node head) {
    if (head == null || head.next == null)
        return head;

    Node newHead = reverse(head.next);

    head.next.next = head; // reverse link
    head.next = null;

    return newHead;
}
```

**💭 Intuition Behind the Approach:**
- Recursion unwinds from the last node upward.
- Each call flips the direction of a single link.
- The last node naturally becomes the head.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
  * Each node participates in one recursive call.
- 💾 Space Complexity
  * O(n)
  * Why?
  * Recursion depth = number of nodes (n)
  * Each call uses stack space.

---

## 6. Justification / Proof of Optimality

- The iterative pointer reversal approach is the optimal solution because:
- It visits each node once
- It requires constant memory (O(1))
- It modifies the list in-place
- It avoids recursion depth issues
- This is the version expected in interviews and coding rounds.
---

## 7. Variants / Follow-Ups

- Reverse a list in groups of k
- Reverse a sub-list between indices [m, n]
- Reverse even/odd-position nodes
- Reverse using recursion in tail-call optimized languages
- Reverse doubly linked list (constant time per node)
---

## 8. Tips & Observations

- Always maintain next pointer before modifying curr.next
- Iterative solution is the fastest and safest
- Recursion works beautifully but stack overflow is a risk
- Do not use extra data structures unless required
---

<!-- #endregion -->
