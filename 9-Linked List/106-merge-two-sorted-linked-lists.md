<!-- #region 106-Merge Two Sorted Linked Lists -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q106: Merge Two Sorted Linked Lists</h1>

## 1. Problem Understanding

- You are given two sorted singly linked lists.
- Your task is to merge them into one sorted linked list by rearranging pointers (not creating new nodes).
- Return the head of the merged list.
- Important:
  * Both lists are sorted in non-decreasing order.
  * Output must also be sorted.
  * Lists may be empty.
---

## 2. Constraints

- Number of nodes: 0 ≤ n, m ≤ 50
- Node value: -100 ≤ val ≤ 100
- Both input lists are sorted.
- Linked lists are singly linked.
---

## 3. Edge Cases

- One list empty → return the other list
- Both empty → return null
- Duplicate values (e.g., 1→2→4 and 1→1→3)
- All elements of one list smaller than the other
- Lists of different lengths
- Negative values
---

## 4. Examples

```text
Example 1

Input:

1 2 4
1 3 4


Output:

1 1 2 3 4 4

Example 2

Input:

1 5 9
1 3 4


Output:

1 1 3 4 5 9
```

---

## 5. Approaches

### Approach 1: Iterative Merge (Optimal, In-Place)

**Idea:**
- Use a dummy node.
- Keep pointers cx and cy on both lists.
- Attach the smaller node to the result and move the pointer.
- No new nodes are created.

**Steps:**
- Create a dummy node to simplify linking.
- Maintain tail pointer — end of merged list.
- Compare cx.data and cy.data:
- Attach the smaller one to tail.next
- Move that pointer
- When one list is exhausted, attach the remaining list.
- Return dummy.next.

**Java Code:**
```java
static Node merge(Node x, Node y) {
    Node dummy = new Node(-1);
    Node tail = dummy;

    while (x != null && y != null) {
        if (x.data <= y.data) {
            tail.next = x;
            x = x.next;
        } else {
            tail.next = y;
            y = y.next;
        }
        tail = tail.next;
    }

    if (x != null) tail.next = x;
    if (y != null) tail.next = y;

    return dummy.next;
}
```

**💭 Intuition Behind the Approach:**
- We treat merging like merging two sorted arrays:
- always pick the smaller front element.
- Linked lists allow us to do this without copying values, we only move pointers.
- This ensures:
  * Correct ordering
  * O(1) extra space
  * Minimal pointer operations

**Complexity (Time & Space):**
- Time: O(n + m)
- Space: O(1) (in-place merge)

### Approach 2: Recursive Merge (Elegant but Uses O(n+m) Stack Space)

**Idea:**
- Pick the smaller head between the two lists.
- That node becomes the head of the merged list.
- Recursively merge the rest.

**Steps:**
- Base cases: if one list is null → return the other
- Compare heads
- The smaller node becomes head of merged list
- Recursively merge remaining part

**Java Code:**
```java
static Node merge(Node a, Node b) {
    if (a == null) return b;
    if (b == null) return a;

    if (a.data <= b.data) {
        a.next = merge(a.next, b);
        return a;
    } else {
        b.next = merge(a, b.next);
        return b;
    }
}
```

**💭 Intuition Behind the Approach:**
- The smaller head must always come first.
- Recursively apply the same logic on the remaining lists.
- Result naturally builds up as recursion unwinds.

**Complexity (Time & Space):**
- Time: O(n + m)
- Space: O(n + m) due to recursion stack

---

## 6. Justification / Proof of Optimality

- The in-place iterative merge is the best solution because:
- It processes each node exactly once → O(n+m)
- No extra nodes → O(1) space
- Avoids recursion stack overhead
- Preserves the original nodes
- This matches the optimal behavior expected for merging sorted linked lists.
---

## 7. Variants / Follow-Ups

- Merge k sorted linked lists → use a min-heap (O(N log k))
- Merge arrays instead of linked lists
- Merge two descending sorted lists
- Merge lists where duplicates should be removed
- Merge circular linked lists
- Merge based on custom comparator (e.g., absolute value)
---

## 8. Tips & Observations

- Always use a dummy node to simplify code.
- Avoid creating new nodes unless required by the question.
- Recursion is elegant but not memory-efficient.
- When merging k lists, use:
  * Min-heap, or
  * Divide & Conquer (merge sort style)
---

<!-- #endregion -->
