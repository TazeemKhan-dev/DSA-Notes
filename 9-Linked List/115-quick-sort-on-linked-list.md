<!-- #region 115-Quick Sort on Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q115: Quick Sort on Linked List</h1>

## 1. Problem Understanding

- You are given the head of a linked list.
- You must sort the list using Quick Sort, but without converting it to an array.
- Quick Sort for linked list works differently from arrays:
- No random access
- We use partition by swapping data, not nodes
- We recursively sort left and right partitions
---

## 2. Constraints

- 1 <= n <= 20000
- Linked list nodes contain integers
- Must use Quick Sort, not Merge Sort
- Must maintain linked list structure
---

## 3. Edge Cases

- Single-node list → already sorted
- Already sorted list
- Reverse sorted list
- Duplicate values
- Very large list (ensure tail-next=null)
- Pivot at end may cause skew → still valid
---

## 4. Examples

```text
Example 1

Input:
3
1 6 2

Output:
1 2 6

Example 2

Input:
4
1 9 3 8

Output:
1 3 8 9
```

---

## 5. Approaches

### Approach 1: QuickSort Using Last Node as Pivot

**Idea:**
- Choose last node as pivot (end).
- Partition list so:
  * Nodes with value < pivot → left side
  * Nodes with value ≥ pivot → right side
- Swap values, not nodes (much easier).
- Recursively sort left and right sublists.

**Steps:**
- Find the tail of the list.
- Call quickSort(head, tail).
- Partition around pivot (the tail).
- Get pivot’s correct position.
- QuickSort left part.
- QuickSort right part.

**Java Code:**
```java
// Partition the list around the end node as pivot
static Node partition(Node head, Node end, Node[] newHead, Node[] newEnd) {
    Node pivot = end;
    Node prev = null, curr = head, tail = pivot;

    // Initially new head and new end will be assigned later
    while (curr != pivot) {
        if (curr.data < pivot.data) {
            if (newHead[0] == null)
                newHead[0] = curr;
            prev = curr;
            curr = curr.next;
        } else {
            // Move nodes >= pivot to end
            if (prev != null)
                prev.next = curr.next;

            Node temp = curr.next;
            curr.next = null;
            tail.next = curr;
            tail = curr;
            curr = temp;
        }
    }

    if (newHead[0] == null)
        newHead[0] = pivot;

    newEnd[0] = tail;
    return pivot;
}


// Recursively apply quick sort
static Node quickSortRecur(Node head, Node end) {
    if (head == null || head == end)
        return head;

    Node[] newHead = new Node[1];
    Node[] newEnd = new Node[1];

    Node pivot = partition(head, end, newHead, newEnd);

    // If pivot is not the smallest element
    if (newHead[0] != pivot) {
        Node temp = newHead[0];

        while (temp.next != pivot)
            temp = temp.next;

        temp.next = null;

        newHead[0] = quickSortRecur(newHead[0], temp);

        temp = getTail(newHead[0]);
        temp.next = pivot;
    }

    pivot.next = quickSortRecur(pivot.next, newEnd[0]);

    return newHead[0];
}


// Returns the tail of a linked list
static Node getTail(Node head) {
    while (head != null && head.next != null)
        head = head.next;
    return head;
}


// Main QuickSort function
static Node quickSort(Node head) {
    Node tail = getTail(head);
    return quickSortRecur(head, tail);
}
```

**💭 Intuition Behind the Approach:**
- QuickSort works best when we split partitions.
- Since linked lists don’t allow random access, swapping nodes is hard.
- Instead, we swap data, making partition simpler.
- Pivot is chosen as last node for consistency.
- Recursion builds sorted sublists automatically.
- Final linking maintains correct order.

**Complexity (Time & Space):**
- Time Complexity
  * Best/Average: O(N log N)
  * Worst (already sorted / reverse): O(N^2)
- Space Complexity
  * Recursion depth: O(log N) (best)
  * Worst-case recursion: O(N)

### Approach 2: QuickSort on Values Only

**Idea:**
- Copy all values to an array → apply QuickSort → rebuild list.
- ❌ Not allowed here (problem requires QuickSort on list itself).

---

## 6. Justification / Proof of Optimality

- QuickSort is chosen because:
- Partitioning is straightforward using last node as pivot.
- Works in-place; no extra arrays.
- Allowed by problem even though MergeSort is normally preferred.
---

## 7. Variants / Follow-Ups

- QuickSort with random pivot
- QuickSort by node swapping instead of data swapping
- 3-way partition QuickSort (useful with many duplicates)
---

## 8. Tips & Observations

- Always use last node as pivot for simplicity.
- Use data swapping, not node swapping.
- Ensure to properly cut and reattach sublists.
- Linked list QuickSort is trickier than array QuickSort because of no random access.
- For interviews, MergeSort is usually preferred — but QuickSort is asked for concept testing.
---

<!-- #endregion -->
