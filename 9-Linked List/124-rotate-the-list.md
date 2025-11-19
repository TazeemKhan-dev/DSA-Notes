<!-- #region 124-Rotate the List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q124: Rotate the List</h1>

## 1. Problem Understanding

- We want to rotate a linked list by k positions.
- There are THREE related problems:
  * Rotate Right by k
    * 1 → 2 → 3 → 4 → 5, k = 2
    * Output: 4 → 5 → 1 → 2 → 3
  * Rotate Left by k
    * 1 → 2 → 3 → 4 → 5, k = 2
    * Output: 3 → 4 → 5 → 1 → 2
  * Rotate a Circular Linked List
    * Same logic but the list is already circular.
- They are variations of the same pointer technique.
---

## 2. Constraints

- 0 ≤ n ≤ 1000
- k ≥ 0
- Linked list may be:
  * singly linked
  * singly linked but circular
---

## 3. Edge Cases

- k == 0
- head == null
- head.next == null
- k % n == 0
- circular list with 1 node
---

## 4. Examples

```text
Rotate Right
1 2 3 4 5, k = 2 → 4 5 1 2 3

Rotate Left
1 2 3 4 5, k = 2 → 3 4 5 1 2

Circular Rotate (already circular)
(1)—(2)—(3)—(4)—(loops to 1)
k = 2
Result head = 3
```

---

## 5. Approaches

### Approach 1: Brute Force (for left & right)

**Idea:**
- Rotate list one step repeatedly for k times.

**Steps:**
- For right rotation: move last node to front
- For left rotation: move head to tail

**💭 Intuition Behind the Approach:**
- Simulates rotation exactly as defined but is slow
- Not efficient for large k

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(k * n)
  * Because each rotation is O(n)
- 💾 Space Complexity
  * O(1)

### Approach 2: Optimal (Length + Cycle Trick)

**Idea:**
- All rotations can be solved by:
  * Making the list circular
  * Moving a pointer
  * Breaking the cycle at the new position
- This unifies all 3 problems.

**Steps:**
- Given a list:
- 1 → 2 → 3 → 4 → 5
- Let n = 5.
- RIGHT rotation (comment: this is for RIGHT rotation)
  * Rotate right by k = 2:
  * New head index = n - (k % n)
                 * = 5 - 2
                 * = index 3 → value 4
- LEFT rotation (comment: this is for LEFT rotation)
  * Rotate left by k = 2:
  * New head index = k % n
                 * = 2
                 * = index 2 → value 3
- CIRCULAR list (comment: this is for CIRCULAR LIST)
  * Circular linked list already forms a cycle, so:
  * we do NOT need to join tail → head
  * simply move a pointer k steps
  * new head = node at position k

**Java Code:**
```java
public ListNode rotate(ListNode head, int k, boolean rightRotate, boolean circular) {
    if (head == null || head.next == null || k == 0) return head;

    // Step 1: find length + last node
    ListNode tail = head;
    int n = 1;
    while (tail.next != null && !circular) { // if circular, stop
        tail = tail.next;
        n++;
    }

    // If list is circular, we do NOT compute length by traversal
    if (circular) {
        // Must count nodes manually since next never becomes null
        ListNode cur = head.next;
        n = 1;
        while (cur != head) {
            n++;
            cur = cur.next;
        }
        tail = head;
        while (tail.next != head) tail = tail.next;
    }

    k = k % n;
    if (k == 0) return head;

    // STEP 2: Make list circular if not already
    if (!circular) tail.next = head;

    // STEP 3: Decide how many steps to move
    int steps;
    if (rightRotate) {
        // (comment: for RIGHT rotation)
        steps = n - k;
    } else {
        // (comment: for LEFT rotation)
        steps = k;
    }

    // STEP 4: Move to new tail
    ListNode newTail = head;
    for (int i = 1; i < steps; i++) {
        newTail = newTail.next;
    }

    // STEP 5: New head is after newTail
    ListNode newHead = newTail.next;

    // Break circularity only if original list was NOT circular
    if (!circular) newTail.next = null;

    return newHead;
}
```

**💭 Intuition Behind the Approach:**
- A rotation is fundamentally a shift of the starting point of a list
- By turning the list into a circle, all rotations become:
  * move pointer → break link
- This unifies left, right, and circular rotation in one logic
- Instead of doing k steps repeatedly, we compute the exact position using formula

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Finding length = O(n)
  * Moving steps = O(n)
  * Breaking cycle = O(1)
- 💾 Space Complexity
  * O(1)
  * Only pointers

---

## 6. Justification / Proof of Optimality

- Brute force is too slow for large k
- The cycle trick is optimal
- Same logic solves:
- Rotate Right
- Rotate Left
- Rotate Circular LL
- Clean, powerful, minimal pointer operations
---

## 7. Variants / Follow-Ups

- Circular left rotation
- Circular right rotation
- Rotation in doubly-linked list
- Rotation in arrays using same math (k % n)
- Rotation in strings using the same circular shift idea
---

## 8. Tips & Observations

- Always normalize k by k = k % n
- In circular list: NEVER break the cycle unless required
- For right rotation, think “end goes to front”
- For left rotation, think “start moves to back”
- The circular method is the GOLDEN TRICK for all rotations
---

<!-- #endregion -->
