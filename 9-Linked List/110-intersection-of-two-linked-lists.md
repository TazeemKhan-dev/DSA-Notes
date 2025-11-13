<!-- #region 110-Intersection of Two Linked Lists -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q110: Intersection of Two Linked Lists</h1>

## 1. Problem Understanding

- You are given two singly linked lists which eventually merge into a common tail.
- Your task: Find the intersection point (node value).
- Points to note:
  * Intersection means same node by reference, not same value.
  * After intersection, both lists share the same nodes.
  * Lists can have different lengths.
  * Must return the value of the intersection node.
---

## 2. Constraints

- 1 ≤ T ≤ 10
- 1 ≤ N, M ≤ 10^4
- Node values up to 10^5
- Lists are non-empty
- Intersection is guaranteed (based on input structure)
---

## 3. Edge Cases

- Intersection at head
- Intersection at very last node
- One list is significantly longer
- Large lists (10k nodes) → avoid recursion
- No intersection (not given in this problem, but generally possible)
---

## 4. Examples

```text
Example 1

Input:

5 1 3
3 6 9 15 30
10


Output:

15

Example 2

Input:

5 1 3
1 2 3 4 5
3


Output:

4
```

---

## 5. Approaches

### Approach 1: Length Difference Method (Optimal & Standard)

**Idea:**
- Compute lengths of both lists.
- Advance the pointer of the longer list by |len1 - len2| steps.
- Now both pointers are the same distance from intersection.
- Move both one step at a time until they meet → intersection node.

**Steps:**
- Count length of L1 → len1
- Count length of L2 → len2
- Set ptr1 = head1, ptr2 = head2
- If one list is longer, advance its pointer
- Move both until ptr1 == ptr2
- Return ptr1 (intersection)

**Java Code:**
```java
static Node intersection(Node head1, Node head2) {
    int len1 = 0, len2 = 0;

    Node temp1 = head1, temp2 = head2;

    while (temp1 != null) {
        len1++;
        temp1 = temp1.next;
    }

    while (temp2 != null) {
        len2++;
        temp2 = temp2.next;
    }

    temp1 = head1;
    temp2 = head2;

    int diff = Math.abs(len1 - len2);

    if (len1 > len2) {
        while (diff-- > 0) temp1 = temp1.next;
    } else {
        while (diff-- > 0) temp2 = temp2.next;
    }

    while (temp1 != temp2) {
        temp1 = temp1.next;
        temp2 = temp2.next;
    }

    return temp1;
}
```

**💭 Intuition Behind the Approach:**
- If you skip extra nodes from the longer list,
- both pointers will reach the intersection at the same time.
- Now they walk together until they meet.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N + M)
  * Why?
    * One pass for length counting + one pass for alignment + one pass to find intersection.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only pointers and counters used.

### Approach 2: Two Pointer Magic (Elegant, No Length Calculation)

**Idea:**
- This is the most elegant and interview-famous solution.
- Let pointers p1 and p2 traverse both lists.
- When a pointer reaches the end, redirect it to the other list's head.
- Why does this work?
- Because both pointers travel exactly:
- len1 + len2 distance → guarantees meeting point.

**Steps:**
- Init p1 = head1, p2 = head2
- Move both pointers forward
- When pointer reaches null → reset it to other list’s head
- Eventually both pointers meet at intersection

**Java Code:**
```java
static Node intersection(Node head1, Node head2) {
    Node p1 = head1;
    Node p2 = head2;

    while (p1 != p2) {
        p1 = (p1 == null) ? head2 : p1.next;
        p2 = (p2 == null) ? head1 : p2.next;
    }

    return p1; // or p2
}
```

**💭 Intuition Behind the Approach:**
- After switching heads:
  * Both pointers travel the same total distance
  * So they meet exactly at intersection
  * If no intersection → both reach null together
  * (not required in this problem, but useful truth)

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N + M)
  * Why?
    * Each pointer traverses both lists exactly once.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only two pointers used.

---

## 6. Justification / Proof of Optimality

- The Two Pointer Magic method is the most elegant, one-pass, and constant-space solution.
- The Length Difference method is more intuitive but equally optimal.
- Both satisfy the constraints perfectly.
---

## 7. Variants / Follow-Ups

- Detect intersection without guarantee
- Find intersection of two circular linked lists
- Merge two lists and find intersection
- Find intersection of more than 2 lists
- Intersection where values equal but references differ (not considered here)
---

## 8. Tips & Observations

- Intersection means same node, not just same value
- Two-pointer method simplifies logic
- Good for interviewer discussion
- Hashing method is simple but not optimal
- Avoid brute-force for large lists
---

<!-- #endregion -->
