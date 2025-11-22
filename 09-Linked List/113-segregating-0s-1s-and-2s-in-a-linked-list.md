<!-- #region 113-Segregating 0s, 1s, and 2s in a Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q113: Segregating 0s, 1s, and 2s in a Linked List</h1>

## 1. Problem Understanding

- You are given a linked list with nodes containing only 0, 1, and 2.
- You must rearrange the linked list so that:
  * All 0s come first
  * then 1s
  * then 2s
- Order among equal elements does not need to be preserved strictly (but in-place methods preserve relative order).
---

## 2. Constraints

- 1 ≤ n ≤ 1000
- Node values: 0, 1, or 2
- List is singly linked
---

## 3. Edge Cases

- List of only 0s, only 1s, or only 2s
- Empty list
- Single element
- Already sorted
- Reverse sorted
- All duplicates clustered
---

## 4. Examples

```text
Example 1

Input:
0 1 1 1 2 0 1 0 1 1

Output:
0 0 0 1 1 1 1 1 1 2

Example 2

Input:
0 1 2 0 1

Output:
0 0 1 1 2
```

---

## 5. Approaches

### Approach 1: Counting Method (Optimal, Simplest logic)

**Idea:**
- Count number of 0, 1, 2
- Then traverse list again and overwrite values.

**Steps:**
- Traverse list and count c0, c1, c2
- Traverse list again:
  * First fill c0 times 0
  * Then c1 times 1
  * Finally c2 times 2
- Return head

**Java Code:**
```java
static Node segregate(Node head) {
    int c0=0, c1=0, c2=0;

    Node curr = head;
    while (curr != null) {
        if (curr.data == 0) c0++;
        else if (curr.data == 1) c1++;
        else c2++;
        curr = curr.next;
    }

    curr = head;
    while (c0-- > 0) { curr.data = 0; curr = curr.next; }
    while (c1-- > 0) { curr.data = 1; curr = curr.next; }
    while (c2-- > 0) { curr.data = 2; curr = curr.next; }

    return head;
}
```

**💭 Intuition Behind the Approach:**
- This is the linked-list version of Dutch National Flag.
- We’re not changing nodes, just updating values.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
    * One pass to count + one pass to rewrite values.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only integer counters used.

### Approach 2: Three Dummy Lists

**Idea:**
- Create 3 separate lists:
  * One for 0s
  * One for 1s
  * One for 2s
- Use original nodes (no new nodes).
- Then connect:
  * 0-list → 1-list → 2-list

**Steps:**
- Create 3 dummy heads: zero, one, two
- Traverse input list:
  * If data is 0 → move node to zero list
  * If 1 → move to one list
  * If 2 → move to two list
- Connect:
  * zero → one → two
  * Return zero.next

**Java Code:**
```java
static Node segregate(Node head) {
    Node zeroD = new Node(-1), oneD = new Node(-1), twoD = new Node(-1);
    Node zero = zeroD, one = oneD, two = twoD;

    Node curr = head;

    while (curr != null) {
        if (curr.data == 0) {
            zero.next = curr;
            zero = zero.next;
        }
        else if (curr.data == 1) {
            one.next = curr;
            one = one.next;
        }
        else {
            two.next = curr;
            two = two.next;
        }
        curr = curr.next;
    }

    // Connect lists
    zero.next = oneD.next != null ? oneD.next : twoD.next;
    one.next = twoD.next;
    two.next = null;

    return zeroD.next;
}
```

**💭 Intuition Behind the Approach:**
- Instead of creating new nodes (your mistake),
- we reuse the original nodes and rearrange pointers.
- This is fully in-place.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
    * One single pass through list.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only uses dummy heads (constant nodes).

### Approach 3: Dutch National Flag with Pointer Swaps

**Idea:**
- Instead of counting or splitting the list into multiple lists,
- we perform a 3-way partition directly on the linked list —
- just like the Dutch National Flag algorithm.
- We maintain 3 regions while traversing:
  * Region of 0s
  * Region of 1s
  * Region of 2s
- But since it's a singly linked list, we can't use array-like indices.
- Instead, we use three pointers to maintain boundaries and swap data values.
- Important:
- This method only swaps node.data, not links.

**Steps:**
- Use three pointers:
  * ptr0 → last node where data = 0
  * ptr1 → last node where data = 1
  * ptr2 → current scanning node
- Traverse list using ptr2:
- If ptr2.data == 0
  * Swap ptr2.data with ptr0.next.data
  * Move both ptr0 and ptr2
- If ptr2.data == 1
  * Swap with ptr1.next.data
  * Move ptr1 and ptr2
- If ptr2.data == 2
  * Just move ptr2
- Goal:
  * [0 region]  [1 region]  [Unprocessed region]
  * This builds 0s first, then 1s, then 2s.

**Java Code:**
```java
static Node segregate(Node head) {
    if (head == null || head.next == null)
        return head;

    // ptr0 will mark end of 0s region
    Node ptr0 = new Node(-1);
    ptr0.next = head;
    ptr0 = ptr0.next;

    // ptr1 will mark end of 1s region
    Node ptr1 = ptr0;

    // ptr2 scans list
    Node ptr2 = ptr0.next;

    while (ptr2 != null) {
        if (ptr2.data == 0) {
            // expand 0-region -> swap ptr2 with ptr0
            int temp = ptr2.data;
            ptr2.data = ptr0.data;
            ptr0.data = temp;

            ptr0 = ptr0.next;
            if (ptr1 == ptr0) ptr1 = ptr1.next;

            ptr2 = ptr2.next;
        }
        else if (ptr2.data == 1) {
            // expand 1-region -> swap ptr2 with ptr1
            int temp = ptr2.data;
            ptr2.data = ptr1.data;
            ptr1.data = temp;

            ptr1 = ptr1.next;
            ptr2 = ptr2.next;
        }
        else { 
            // data == 2 → unprocessed region
            ptr2 = ptr2.next;
        }
    }

    return head;
}
```

**💭 Intuition Behind the Approach:**
- This uses the exact logic of Dutch National Flag:
- Move 0s to front
- Move 1s to the middle
- Leave 2s at the back
- But since linked lists can’t jump back and forth,
- we simulate the 3-way partition by maintaining pointers to the boundaries
- and swapping values into correct regions.
- We simulate the 3 bins (0,1,2) in-place, rearranging each item into its correct region by swapping data fields.
- This avoids:
- Creating new nodes
- Managing multiple lists
- Using counting passes
- Instead, it dynamically rearranges data during one traversal.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * WHY?
    * Each of the nodes is visited exactly once by ptr2.
- 💾 Space Complexity
  * O(1)
  * WHY?
    * Only a few pointers (ptr0, ptr1, ptr2) and temp swaps are used.
- When to Use / Avoid
   * Use when:
    * You want in-place partitioning
    * No extra memory allowed
    * You’re allowed to modify node data
  * Avoid when:
    * Data swap not allowed
    * Nodes must not be modified except via linking
    * List contains large objects (not primitive values)

---

## 6. Justification / Proof of Optimality

- The optimal solutions are:
- Approach 1 (Counting) → simplest, fastest
- Approach 2 (3-lists in-place) → best pointer-based solution
- Your old solution was close to Approach 2, but created new nodes, making it O(n) space.
- Correct approach uses no new nodes, just pointer attachments.
---

## 7. Variants / Follow-Ups

- Sort linked list of 0–1 only
- Sort linked list with arbitrary integers (merge sort)
- Sort linked list based on custom 3-way criteria
- Sort linked list but preserve stability
---

## 8. Tips & Observations

- For values limited to 0, 1, 2 → counting is easiest
- For general linked list sorting → merge sort is required
- Avoid creating new nodes—pointer manipulation is faster & memory-efficient
---

<!-- #endregion -->
