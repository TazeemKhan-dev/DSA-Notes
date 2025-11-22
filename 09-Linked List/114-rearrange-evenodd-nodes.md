<!-- #region 114-Rearrange Even–Odd Nodes -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q114: Rearrange Even–Odd Nodes</h1>

## 1. Problem Understanding

- Given the head of a singly linked list, rearrange the list so that:
  * All even-valued nodes come before all odd-valued nodes
  * Relative order inside even group must remain preserved
  * Relative order inside odd group must remain preserved
  * Modify the list in-place (no creation of new actual nodes)
---

## 2. Constraints

- 1 ≤ n ≤ 1000
- Node values are integers
- Must preserve original ordering within each group
- Must not create new nodes representing list values
---

## 3. Edge Cases

- All values even → no change
- All values odd → no change
- Single element
- Mixed order
- Already partitioned
- Even values appear after some odd values
---

## 4. Examples

```text
Example 1

Input:

1 2 3 4 5


Output:

2 4 1 3 5

Example 2

Input:

2 4 6 8 10 1 3 5 7 9


Output:

2 4 6 8 10 1 3 5 7 9
```

---

## 5. Approaches

### Approach 1: In-Place Stable Partition Using Dummy Nodes (Optimal)

**Idea:**
- Use two pointer chains:
  * even list to gather all nodes with value % 2 == 0
  * odd list to gather all nodes with value % 2 == 1
- We reuse the same nodes, we simply attach them into two separate chains, then connect:
- even-list → odd-list

**Steps:**
- Create two dummy nodes evenD, oddD
- Traverse original list:
  * If node.val is even → append to even chain
  * Else → append to odd chain
- Connect even chain tail → odd chain head
- end odd list with null
- Return evenD.next

**Java Code:**
```java
public Node rearrangeList(Node head) {
    if (head == null || head.next == null) return head;

    Node evenD = new Node(-1), oddD = new Node(-1);
    Node even = evenD, odd = oddD;

    Node curr = head;

    while (curr != null) {
        if (curr.val % 2 == 0) {
            even.next = curr;
            even = even.next;
        } else {
            odd.next = curr;
            odd = odd.next;
        }
        curr = curr.next;
    }

    odd.next = null;             // end odd list
    even.next = oddD.next;       // connect even + odd

    return evenD.next;           // head of rearranged list
}
```

**💭 Intuition Behind the Approach:**
- Partitioning the original list into two linked chains maintains the order.
- Even list grows first, preserving all even nodes as they appear.
- Odd list grows next, preserving all odd nodes as they appear.
- Finally attaching the two lists produces the required output.
- This is exactly like stable partition in arrays but done with pointer adjustments.
- Dummy nodes are in-place because they do not store real data nodes, do not become part of the final list, and use only constant memory. The final list contains only original nodes.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
    * Single traversal collecting even and odd chains.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Dummy nodes are constant-memory and do not scale with input size.

---

## 6. Variants / Follow-Ups

- Partition around any pivot value
- Stable partition without extra memory
- Partition doubly linked list
- Partition around multiple conditions (e.g., negative/zero/positive)
---

## 7. Tips & Observations

- Dummy nodes make merging logic easier
- Always end the final list with null
- Avoid creating new nodes with original values
- Use pointer manipulation for true in-place modification
---

<!-- #endregion -->
