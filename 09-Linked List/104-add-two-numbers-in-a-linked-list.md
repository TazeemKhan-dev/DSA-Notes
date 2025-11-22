<!-- #region 104-Add Two Numbers in a Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q104: Add Two Numbers in a Linked List</h1>

## 1. Problem Understanding

- We are given two non-empty singly linked lists l1 and l2, where each node represents a single digit of a number.
- The digits are stored in reverse order — meaning the 1’s digit is at the head of the list.
- We must add the two numbers and return the sum as a new linked list, also in reverse order.
- Example:
- l1 = [5 → 4] represents 45
- l2 = [4] represents 4
- Sum = 49 → Output: [9 → 4]
---

## 2. Constraints

- 1 <= Number of nodes in each list <= 100
- 0 <= value of each node <= 9
- Lists have no leading zeros (except when the number itself is 0).
---

## 3. Edge Cases

- One list is longer than the other.
- Carry remains after the final addition.
- One list could represent zero (e.g., [0]).
- Both lists contain only one node.
---

## 4. Examples

```text
Example 1:
Input:
l1 = [5 → 4], l2 = [4]
Output:
[9 → 4]
Explanation: 45 + 4 = 49.

Example 2:
Input:
l1 = [4 → 5 → 6], l2 = [1 → 2 → 3]
Output:
[5 → 7 → 9]
Explanation: 654 + 321 = 975.

Example 3:
Input:
l1 = [1], l2 = [8 → 7]
Output:
[9 → 7]
Explanation: 1 + 78 = 79.
```

---

## 5. Approaches

### Approach 1: Iterative Addition with Carry (Optimal)

**Idea:**
- Perform addition like manual digit-by-digit addition:
  * Traverse both linked lists simultaneously.
  * Sum digits + carry.
  * Create a new node for each resulting digit.
  * Continue until both lists are processed and carry is zero.

**Steps:**
- Initialize a dummy node and a carry = 0.
- Traverse both lists until both are null:
  * Compute sum = (l1.val if exists) + (l2.val if exists) + carry.
  * Create a new node with sum % 10.
  * Update carry = sum / 10.
- If carry > 0 after traversal, create one more node.
- Return dummy.next.

**Java Code:**
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode curr = dummy;
        int carry = 0;

        while (l1 != null || l2 != null) {
            int sum = carry;
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
        }

        if (carry > 0) {
            curr.next = new ListNode(carry);
        }

        return dummy.next;
    }
}
```

**💭 Intuition Behind the Approach:**
- The process mirrors normal addition from right to left — except the digits are already stored in reverse.
- Each step adds the corresponding digits and carries over the overflow (like adding column-wise).

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(max(N, M))
  * (where N and M are lengths of l1 and l2)
- 💾 Space Complexity
  * O(max(N, M)) (for the new linked list)

### Approach 2: Recursive Approach

**Idea:**
- Use recursion to simulate the digit-by-digit addition from the start of both lists.
- Each recursive call handles one pair of digits and the carry.

**Steps:**
- Base case: If both lists are null and carry is 0, return null.
- Sum up current nodes’ values + carry.
- Create a node with sum % 10.
- Recurse to process the next nodes and pass the carry (sum / 10).
- Link the newly created node to the result of recursion.

**Java Code:**
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        return addHelper(l1, l2, 0);
    }

    private ListNode addHelper(ListNode l1, ListNode l2, int carry) {
        if (l1 == null && l2 == null && carry == 0) return null;

        int sum = carry;
        if (l1 != null) sum += l1.val;
        if (l2 != null) sum += l2.val;

        ListNode node = new ListNode(sum % 10);
        node.next = addHelper(
            (l1 != null ? l1.next : null),
            (l2 != null ? l2.next : null),
            sum / 10
        );
        return node;
    }
}
```

**💭 Intuition Behind the Approach:**
- Recursion naturally handles the propagation of carry and traversal of both lists.
- Each call represents the computation of a single digit and delegates the remaining work to the next call.
- It’s a clean, functional approach — just like stack-based addition.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
- O(max(N, M))
- 💾 Space Complexity
- O(max(N, M)) (recursive call stack + new list nodes)

---

## 6. Justification / Proof of Optimality

- Both approaches perform exactly one traversal over each list, ensuring linear time and minimal space.
- The iterative version is slightly more memory-efficient, but the recursive version is more elegant.
---

## 7. Variants / Follow-Ups

- Digits stored in forward order: Use stacks or reverse the lists first.
- Addition with large numbers in strings: Convert strings to digits and simulate addition.
- Add K numbers represented as linked lists: Use repeated pairwise addition or a priority queue.
---

## 8. Tips & Observations

- Always use a dummy node to simplify result list creation.
- Keep careful track of carry propagation.
- Recursive solutions are elegant but can cause stack overflow if lists are extremely long.
- Iterative is preferred for production; recursion is good for conceptual clarity.
---

<!-- #endregion -->
