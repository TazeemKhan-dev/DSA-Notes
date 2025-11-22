<!-- #region 122-Segregate Node of LinkedList Over last Index  -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q122: Segregate Node of LinkedList Over last Index</h1>

## 1. Problem Understanding

- You are given the head of a singly linked list.
- You must partition/segregate the list based on the value of the last node (pivot).
  * Let pivot = value of last node.
  * Move all nodes with values ≤ pivot to the left side.
  * Move all nodes with values > pivot to the right side.
  * Preserve relative order inside both partitions.
- This is similar to partitioning a linked list around a pivot, but the pivot is always the last element.
---

## 2. Constraints

- 1 <= N <= 10^5
- Node values can be negative or positive.
- Linked list may contain duplicate values.
- Must preserve stable ordering (original order inside partitions).
- Must use O(1) extra space.
---

## 3. Edge Cases

- Single element list → return as-is
- All elements <= pivot → list remains same
- All elements > pivot → pivot moves to end of list
- Duplicate values equal to pivot
- Already partitioned list
- Pivot is smallest / largest element
---

## 4. Examples

```text
Example 1

Input: 10 → 3 → 5 → 2 → 8 → 5
Last value = 5

Output:
3 → 5 → 2 → 5 → 10 → 8

Example 2

Input: 4 → 1 → 9 → 7
Pivot = 7
Output: 4 → 1 → 7 → 9
```

---

## 5. Approaches

### Approach 1: Using Two Lists (Stable Partitioning)

**Idea:**
- Idea
- Scan list once.
- Build two new linked segments:
  * ≤ pivot
  * > pivot
- Join them.
- This preserves order without extra data structures.

**Steps:**
- Find the pivot value (value of last node).
- Create four pointers:
  * smallHead, smallTail
  * largeHead, largeTail
- Traverse list:
  * if node.val <= pivot → add to small list
  * else → add to large list
- Join lists:
  * smallTail.next = largeHead
- Return smallHead (if exists), else return largeHead.

**Java Code:**
```java
public ListNode segregate(ListNode head) {
    if (head == null || head.next == null) return head;

    // Step 1: find pivot (last node's value)
    ListNode temp = head;
    while (temp.next != null) temp = temp.next;
    int pivot = temp.val;

    ListNode smallHead = null, smallTail = null;
    ListNode largeHead = null, largeTail = null;

    temp = head;

    // Step 2: partition into two lists
    while (temp != null) {
        if (temp.val <= pivot) {
            if (smallHead == null) {
                smallHead = smallTail = temp;
            } else {
                smallTail.next = temp;
                smallTail = temp;
            }
        } else {
            if (largeHead == null) {
                largeHead = largeTail = temp;
            } else {
                largeTail.next = temp;
                largeTail = temp;
            }
        }
        temp = temp.next;
    }

    // Step 3: merge lists
    if (smallTail != null) smallTail.next = largeHead;
    if (largeTail != null) largeTail.next = null;

    return smallHead != null ? smallHead : largeHead;
}
```

**💭 Intuition Behind the Approach:**
- Instead of rearranging values or swapping nodes (which complicates pointers), we logically create two segments based on pivot comparison.
- Because each list is built by appending, relative order remains intact.
- Finally, concatenating the two lists gives the desired segregated list.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * One traversal to find pivot: O(N)
  * One traversal to build lists: O(N)
  * Overall: O(N)
  * Why: Each node is visited exactly once.
- 💾 Space Complexity
  * Extra pointers only: O(1)
  * Why: We are not creating new nodes; just rearranging pointers.

---

## 6. Justification / Proof of Optimality

- This approach keeps the original stability (no order break).
- It runs in linear time, optimal for linked lists.
- Uses constant space, required for in-place processing.
---

## 7. Variants / Follow-Ups

- Partition around any given value, not necessarily last element.
- Partition around median.
- Partition a doubly linked list.
- Partition without preserving order (can be done in-place faster).
---

## 8. Tips & Observations

- Using two pointers (small/large) is the cleanest way to maintain order.
- Avoid swapping node values; it breaks stable partitioning.
- Linked list partitioning is similar to stable quicksort partition.
---

<!-- #endregion -->
