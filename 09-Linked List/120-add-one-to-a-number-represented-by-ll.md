<!-- #region 120-Add one to a number represented by LL -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q120: Add one to a number represented by LL</h1>

## 1. Problem Understanding

- You are given a singly linked list where each node stores a digit of a positive integer.
- The leftmost digit is the head.
- Your task is to add 1 to the number represented by this linked list and return the updated head.
---

## 2. Constraints

- 0 <= number of nodes <= 100000
- Each node value is between 0 and 9
- No leading zeros unless number is exactly 0
- Large constraints mean O(N) solution required
---

## 3. Edge Cases

- All digits are 9 → requires increasing list length
  * e.g., 9 → 9 → 1 → 0 → 0
- Empty list (0 nodes) → treat as 0 and return node with value 1
- Last digit < 9 → only last node changes, no carry propagation
- Long chain of carries (1 → 9 → 9 → 9)
---

## 4. Examples

```text
Example 1

Input: 1 → 2 → 3
Output: 1 → 2 → 4

Example 2

Input: 9 → 9
Output: 1 → 0 → 0

Example 3

Input: 9
Output: 1 → 0
```

---

## 5. Approaches

### Approach 1: Reverse, Add, Reverse Back

**Idea:**
- Reverse the linked list.
- Add 1 starting from the least significant digit (head after reverse).
- Propagate carry.
- Reverse again to restore original order.

**Steps:**
- Reverse the list.
- Start from head:
  * Add 1.
  * If digit < 10, done.
  * Else set digit = 0 and continue.
  * If carry remains, add a new node with value 1.
- Reverse again.
- Return new head.

**Java Code:**
```java
public ListNode addOne(ListNode head) {
    head = reverse(head);

    ListNode temp = head;
    int carry = 1;

    while (temp != null) {
        int sum = temp.val + carry;
        temp.val = sum % 10;
        carry = sum / 10;

        if (carry == 0) break;

        if (temp.next == null && carry == 1) {
            temp.next = new ListNode(1);
            carry = 0;
            break;
        }

        temp = temp.next;
    }

    return reverse(head);
}

private ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode nxt = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nxt;
    }
    return prev;
}
```

**💭 Intuition Behind the Approach:**
- Numbers are normally added from right-to-left, but the LL stores digits left-to-right.
- Reversing allows us to perform addition naturally.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Why?
    * One reverse → O(N)
    * One traversal to add → O(N)
    * One reverse → O(N)
    * Total remains linear because constants are ignored.
- 💾 Space Complexity
  * O(1)
  * Why?
    * We only use a few pointers (prev, curr, etc.) and no extra data structures.

### Approach 2: Recursive Right-to-Left Addition

**Idea:**
- Use recursion to reach the last digit first (like reverse traversal), add 1, and propagate carry back naturally during recursion unwind.

**Steps:**
- Define recursive function returning carry.
- Recursively call on head.next.
- Add carry to current node.
- Set node value = sum % 10, carry = sum / 10.
- At end, if carry = 1, add new node at front.

**Java Code:**
```java
public ListNode addOne(ListNode head) {
    int carry = helper(head);

    if (carry == 1) {
        ListNode newNode = new ListNode(1);
        newNode.next = head;
        head = newNode;
    }
    return head;
}

private int helper(ListNode node) {
    if (node == null) return 1;

    int carry = helper(node.next);
    int sum = node.val + carry;

    node.val = sum % 10;
    return sum / 10;
}
```

**💭 Intuition Behind the Approach:**
- Recursion allows us to simulate reverse traversal without reversing the linked list.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Why?
    * Every node is visited exactly once during recursion.
- 💾 Space Complexity
  * O(N) (recursion stack)
  * Why?
    * Recursive calls go N levels deep (for N nodes).
    * Not constant space.

### Approach 3: Single Pass Without Reversal (Optimized Marking Last Non-9)

**Idea:**
- Smart observation:
- Only digits after the last non-9 need to be reset to 0.
- Example:
- 1 → 2 → 9 → 9 → 9
- Last non-9 = 2
- Steps:
  * Increase last non-9 by 1
  * Set all digits after it to 0

**Steps:**
- Track pointer lastNonNine.
- Traverse once:
  * Keep updating lastNonNine whenever val != 9.
- If no non-9 exists (all 9s):
  * Create new head with value 1
  * Make all nodes after it 0
- Else:
  * lastNonNine.val++
  * Set all following nodes to 0
- Return head

**Java Code:**
```java
public ListNode addOne(ListNode head) {
    ListNode lastNonNine = null, curr = head;

    while (curr != null) {
        if (curr.val != 9) lastNonNine = curr;
        curr = curr.next;
    }

    if (lastNonNine == null) {
        ListNode newNode = new ListNode(1);
        newNode.next = head;
        head = newNode;
        curr = head.next;
        while (curr != null) {
            curr.val = 0;
            curr = curr.next;
        }
    } else {
        lastNonNine.val++;
        lastNonNine = lastNonNine.next;
        while (lastNonNine != null) {
            lastNonNine.val = 0;
            lastNonNine = lastNonNine.next;
        }
    }

    return head;
}
```

**💭 Intuition Behind the Approach:**
- All carries originate from the rightmost digits.
- Carry stops as soon as we find the last digit that is not 9.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Why?
    * Single traversal to find last non-9
    * Possibly another traversal to make trailing digits 0
    * Still linear.
- 💾 Space Complexity
  * O(1)
  * Why?
    * No extra structures; just pointer variables.

---

## 6. Justification / Proof of Optimality

- Adding one requires carry propagation starting from the end.
- All approaches handle carry efficiently:
  * Reversal → simulate right-to-left
  * Recursion → reach tail first
  * Last-non-9 → mathematical insight, fastest constant-space approach
---

## 7. Variants / Follow-Ups

- Add K instead of 1
- Add two numbers represented as LL
- Convert LL → integer (if fits)
- Remove leading zeros
- Subtract 1 from LL
---

## 8. Tips & Observations

- Prefer Approach 3 in interviews → optimal & elegant.
- Reverse method is easiest & most accepted by interviewers.
- Recursive approach is intuitive but not good for large constraints (N=100000).
---

<!-- #endregion -->
