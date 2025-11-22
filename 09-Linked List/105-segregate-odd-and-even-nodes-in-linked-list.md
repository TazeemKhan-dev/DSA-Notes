<!-- #region 105-Segregate Odd and Even Nodes in Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q105: Segregate Odd and Even Nodes in Linked List</h1>

## 1. Problem Understanding

- You are given the head of a singly linked list.
- You need to group all nodes at odd indices first, followed by all nodes at even indices — maintaining their original relative order.
- Note:
  * The index starts from 1 (i.e., the head is the 1st node).
- You must rearrange nodes in place and return the new head of the reordered list.
---

## 2. Constraints

- 1 <= Number of nodes <= 100
- Node values can be any integer.
- The relative order within odd and even groups must remain the same.
- Must use O(1) extra space (rearrange in-place).
---

## 3. Edge Cases

- Empty list (head == null)
- List with only one node
- List with only two nodes
- All odd or all even length lists (no structural issue should occur)
---

## 4. Examples

```text
Example 1:
Input:
head -> 1 -> 2 -> 3 -> 4 -> 5
Output:
head -> 1 -> 3 -> 5 -> 2 -> 4
Explanation: Odd index nodes are [1, 3, 5], even index nodes are [2, 4].

Example 2:
Input:
head -> 4 -> 3 -> 2 -> 1
Output:
head -> 4 -> 2 -> 3 -> 1
Explanation: Odd index nodes → [4, 2], even index nodes → [3, 1].
```

---

## 5. Approaches

### Approach 1: Two Pointer (In-place Rearrangement — Optimal)

**Idea:**
- Use two pointers — one for odd nodes and one for even nodes.
- We can separate the linked list into two sublists:
  * One containing odd-indexed nodes.
  * One containing even-indexed nodes.
- Then, simply link the end of the odd list to the head of the even list.

**Steps:**
- Handle base cases: if head == null or head.next == null, return head.
- Initialize:
  * odd = head
  * even = head.next
  * evenHead = even (store head of even list)
- While both odd.next and even.next exist:
  * Connect odd.next = even.next
  * Move odd = odd.next
  * Connect even.next = odd.next
  * Move even = even.next
- After loop ends, link odd.next = evenHead.
- Return the modified head.

**Java Code:**
```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode odd = head;
        ListNode even = head.next;
        ListNode evenHead = even;

        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;

            even.next = odd.next;
            even = even.next;
        }

        odd.next = evenHead;
        return head;
    }
}
```

**💭 Intuition Behind the Approach:**
- Think of this as unzipping the linked list into two separate sequences:
  * All nodes in odd positions
  * All nodes in even positions
- We keep track of both sequences while traversing once and reconnect them at the end.
- This maintains order and uses no extra space — just pointer manipulation.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) — single traversal of the linked list
- 💾 Space Complexity
  * O(1) — no extra space used, in-place rearrangement

### Approach 2: Construct New Lists (Using Two Dummy Nodes)

**Idea:**
- Instead of rearranging in place, we can create two new lists:
  * One for odd nodes
  * One for even nodes
- Then connect the two lists at the end and return the new head.

**Steps:**
- Create two dummy nodes — oddDummy and evenDummy.
- Traverse the original list, keeping a position counter i.
  * If i is odd → add node to odd list.
  * If i is even → add node to even list.
- After traversal, link the end of the odd list to the start of the even list.
- Set the end of even list’s next = null.
- Return oddDummy.next

**Java Code:**
```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;

        ListNode oddDummy = new ListNode(-1);
        ListNode evenDummy = new ListNode(-1);
        ListNode odd = oddDummy, even = evenDummy;
        int index = 1;

        while (head != null) {
            if (index % 2 != 0) {
                odd.next = head;
                odd = odd.next;
            } else {
                even.next = head;
                even = even.next;
            }
            head = head.next;
            index++;
        }

        even.next = null;     // terminate even list
        odd.next = evenDummy.next; // connect lists

        return oddDummy.next;
    }
}
```

**💭 Intuition Behind the Approach:**
- We explicitly build two separate linked lists based on node indices, then merge them.
- It’s simpler to understand but uses extra references for dummy nodes (still O(1) auxiliary space if pointer reuse is allowed).

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) — each node visited once
- 💾 Space Complexity
  * O(1) — constant extra space (if we reuse original nodes)

---

## 6. Justification / Proof of Optimality

- Both approaches achieve linear time and constant space.
- However, the two-pointer in-place version (Approach 1) is preferred — it is more efficient and elegant without needing dummy nodes.
---

## 7. Variants / Follow-Ups

- Segregate even and odd values (instead of indices).
- Group nodes by multiple conditions (e.g., multiples of 3).
- Rearrange alternatingly (odd, even, odd, even).
---

## 8. Tips & Observations

- Don’t confuse index parity with node value parity.
- Always store the head of the even list before rearranging — otherwise, it gets lost.
- Maintain relative ordering within groups.
- Avoid creating new nodes unnecessarily; re-link existing ones.
---

<!-- #endregion -->
