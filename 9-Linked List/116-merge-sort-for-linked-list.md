<!-- #region 116-Merge Sort for Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q116: Merge Sort for Linked List</h1>

## 1. Problem Understanding

- You are given the head of a linked list with n nodes.
- Your task is to sort the linked list using Merge Sort.
- Merge Sort is ideal for linked lists because:
- It does not need random access
- It works in O(N log N) time
- Merging can be done efficiently using pointers
- This is the standard, most optimal sorting method for linked lists.
---

## 2. Constraints

- 1 <= n <= 100000
- 1 <= node.data <= 1000
- Must use Merge Sort (not arrays, not quick sort)
- Large input size → must be O(N log N) or better
---

## 3. Edge Cases

- Only 1 node → already sorted
- 2 nodes swapped → should sort correctly
- Repeated values
- Negative values? (Not required here, but logic supports it)
- Very long list (ensure no stack overflow)
---

## 4. Examples

```text
Example 1

Input:
5
3 5 2 4 1

Output:
1 2 3 4 5

Example 2

Input:
6
3 5 2 4 1 6

Output:
1 2 3 4 5 6
```

---

## 5. Approaches

### Approach 1: Classic Merge Sort on Linked List (Optimal)

**Idea:**
- Find mid using slow & fast pointers
- Split list into two halves
- Recursively sort each half
- Merge both halves using a sorted merge function

**Steps:**
- Base case: if head is null or head.next is null → return head
- Find mid: using slow–fast pointers
- Split list: left = head, right = mid.next
- Sort both halves:
  * left = mergeSort(left)
  * right = mergeSort(right)
- Merge: return merge(left, right)

**Java Code:**
```java
Find Middle Node
static Node getMid(Node head) {
    Node slow = head, fast = head.next;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}

Merge Two Sorted Lists
static Node merge(Node a, Node b) {
    if (a == null) return b;
    if (b == null) return a;

    Node dummy = new Node(-1);
    Node temp = dummy;

    while (a != null && b != null) {
        if (a.data <= b.data) {
            temp.next = a;
            a = a.next;
        } else {
            temp.next = b;
            b = b.next;
        }
        temp = temp.next;
    }

    if (a != null) temp.next = a;
    else temp.next = b;

    return dummy.next;
}

Merge Sort on Linked List
static Node mergeSort(Node head) {
    if (head == null || head.next == null)
        return head;

    Node mid = getMid(head);
    Node rightHead = mid.next;
    mid.next = null;

    Node left = mergeSort(head);
    Node right = mergeSort(rightHead);

    return merge(left, right);
}
```

**💭 Intuition Behind the Approach:**
- Merge Sort is naturally suited for linked lists.
- Splitting using slow–fast traversal is efficient (O(N)).
- Merging is pointer-based and does not require shifting values.
- Unlike arrays, QuickSort is inefficient on linked lists due to no random access.
- Merge Sort ensures stable, predictable O(N log N) behavior.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N log N)
  * (N for each level merge, log N levels of recursion)
- 💾 Space Complexity
  * O(log N) recursion stack
  * No extra list or array created

### Approach 2: (Not Recommended): Convert to Array and Sort

**Idea:**
- Copy values into an array
- Sort array
- Rebuild list
- ❌ Violates problem requirement
- ❌ Uses extra O(N) space

---

## 6. Justification / Proof of Optimality

- Merge Sort is the most optimal and preferred method for sorting linked lists because:
- Linked lists allow cheap merging
- Do not support random access for QuickSort
- Merge Sort guarantees good performance in worst case
- Stable sorting method
---

## 7. Variants / Follow-Ups

- Merge sort on Doubly Linked List
- Bottom-up Merge Sort (iterative)
- Sorting K-sorted linked list using merge logic
- Merging K sorted lists (Heap + Merge logic)
---

## 8. Tips & Observations

- Always break the list into exact halves to avoid infinite recursion.
- Use slow–fast technique to find the middle reliably.
- Merge function should not create new nodes — re-link existing ones.
- For very large lists, iterative bottom-up merge sort avoids recursion depth issues.
- After merge, ensure next pointers are not accidentally left pointing to old references.
---

<!-- #endregion -->
