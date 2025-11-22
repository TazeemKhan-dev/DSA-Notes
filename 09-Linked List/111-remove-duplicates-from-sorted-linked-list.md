<!-- #region 111-Remove Duplicates From Sorted Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q111: Remove Duplicates From Sorted Linked List</h1>

## 1. Problem Understanding

- You are given the head of a sorted singly linked list.
- Your task is to remove all duplicates so that each element appears only once.
- Return the final head.
- Since the list is sorted, duplicates always appear next to each other.
---

## 2. Constraints

- 1 ≤ n ≤ 300
- Node values range from -100 to 100
- List is sorted in non-decreasing order
- Only deletion of duplicates required, not rearrangement
---

## 3. Edge Cases

- Single node list → no change
- All nodes identical (e.g., 1 1 1 1)
- No duplicates at all
- Duplicates only at the start
- Duplicates only at the end
- Negative values
- Mixed duplicates
---

## 4. Examples

```text
Example 1

Input:

1 1 2


Output:

1 2

Example 2

Input:

1 1 2 3 3


Output:

1 2 3
```

---

## 5. Approaches

### Approach 1: One-Pass Iterative (Optimal)

**Idea:**
- Because the list is sorted, duplicates appear consecutively.
- We just check if next node has the same value, and skip it.

**Steps:**
- Use a pointer curr starting at head
- If curr.data == curr.next.data:
- Skip the next node → curr.next = curr.next.next
- Else move curr = curr.next
- Continue until list ends
- Return head

**Java Code:**
```java
static Node deleteDuplicates(Node head) {
    if (head == null) return null;

    Node curr = head;

    while (curr != null && curr.next != null) {
        if (curr.data == curr.next.data) {
            curr.next = curr.next.next;
        } else {
            curr = curr.next;
        }
    }

    return head;
}
```

**💭 Intuition Behind the Approach:**
- Since the list is sorted, duplicates will always be together.
- We don’t need extra memory—just skip equal consecutive nodes.
- Each duplicate is simply removed by updating pointers.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
    * We traverse the list once, checking each pair only once.
- 💾 Space Complexity
  * O(1)
  * Why?
    * We only use a few pointers, no additional data structures.

### Approach 2: Recursive Removal (Elegant but Costly)

**Idea:**
- Recursively remove duplicates from the next nodes,
- then compare head with the next returned node.

**Steps:**
- If list empty or 1 node → return head
- Recursively call for head.next
- If head.data == head.next.data, skip next
- Else keep as it is

**Java Code:**
```java
static Node deleteDuplicates(Node head) {
    if (head == null || head.next == null) return head;

    head.next = deleteDuplicates(head.next);

    if (head.data == head.next.data) {
        return head.next;
    } else {
        return head;
    }
}
```

**💭 Intuition Behind the Approach:**
- Work on the tail first, then fix head comparison.
- Cleaner approach but uses recursion stack.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
- O(n)
- 💾 Space Complexity
  * O(n) due to recursion stack
  * Why?
    * Worst-case recursion depth = list length.

---

## 6. Justification / Proof of Optimality

- The one-pass iterative method is the best because:
  * Works in a single traversal
  * Uses zero extra memory
  * Avoids recursion
  * Guaranteed correct due to sorted property
- This is the method expected in interviews.
---

## 7. Variants / Follow-Ups

- Remove duplicates from unsorted list → use Set or sorting
- Remove duplicates so elements appear at most twice
- Keep only nodes that appear exactly once
- Remove duplicates from doubly linked list
- Remove duplicates from a circular list
---

## 8. Tips & Observations

- Sorted property is key → simplifies logic
- Always check curr != null && curr.next != null
- Avoid moving curr forward on removing duplicates
- Perfect warm-up for pointer manipulation problems
- Beware of infinite loops when skipping nodes
---

<!-- #endregion -->
