<!-- #region 125-Swap Kth Nodes from End -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q125: Swap Kth Nodes from End</h1>

## 1. Problem Understanding

- You are given a singly linked list of size N and an integer K.
- You must swap the K-th node from the beginning with the K-th node from the end,
- by changing links, not by swapping values.
- Example:
  * List: 1 → 2 → 3 → 4, K = 4
  * Kth from start = 4
  * Kth from end   = 1   (N - K + 1)
  * Swap nodes → 4 → 2 → 3 → 1
- You must update pointers correctly, handling:
  * head changes
  * adjacent nodes
  * same node case
---

## 2. Constraints

- 1 ≤ N ≤ 1000
- 1 < K < N
- Singly linked list
- Swapping values is NOT allowed, only pointers
---

## 3. Edge Cases

- K > N → no swap
- 2*K - 1 == N → both nodes are same → no swap
- one of the nodes is head
- nodes are adjacent
- list size is very small
---

## 4. Examples

```text
Example 1

Input:

5 3
1 2 3 4 5


Kth from start = 3 (node = 3)
Kth from end = 3 (node = 3)

Same node → no swap.

Output:

1 2 3 4 5

Example 2

Input:

4 4
1 2 3 4


Swap 4th from start and 4th from end.

Output:

4 2 3 1
```

---

## 5. Approaches

### Approach 1: Collect Nodes in Array

**Idea:**
- Traverse list, store nodes in an ArrayList
- Swap the two required nodes using pointer redirection
- Not efficient but easy to implement

**Steps:**
- Store each node in a list
- Identify A = Kth from start
- Identify B = Kth from end
- Update their previous links

**💭 Intuition Behind the Approach:**
- Treat LL like indexed array.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
- 💾 Space Complexity
  * O(N)
- Not optimal.

### Approach 2: Single Pass with Pointers

**Idea:**
- Find Kth from start = node A
- Find Kth from end = node B
- Swap actual nodes by changing prev.next pointers
- Careful handling when A or B is head

**Steps:**
- Traverse to find length N
- Kth from end = (N - K + 1)th from start
- Find:
  * prevA, A
  * prevB, B
- Swap pointers:
  * prevA.next = B
  * prevB.next = A
  * A.next ↔ B.next
- If A or B is head, update head accordingly

**Java Code:**
```java
public ListNode swapKthNode(ListNode head, int n, int k) {

    if (k > n) return head;
    if (2 * k - 1 == n) return head; // same node

    // Step 1: Find Kth from start
    ListNode prevA = null, A = head;
    for (int i = 1; i < k; i++) {
        prevA = A;
        A = A.next;
    }

    // Step 2: Find Kth from end
    int posB = n - k + 1;
    ListNode prevB = null, B = head;
    for (int i = 1; i < posB; i++) {
        prevB = B;
        B = B.next;
    }

    // Step 3: Link previous nodes
    if (prevA != null) prevA.next = B;
    else head = B;

    if (prevB != null) prevB.next = A;
    else head = A;

    // Step 4: Swap next pointers
    ListNode temp = A.next;
    A.next = B.next;
    B.next = temp;

    return head;
}
```

**💭 Intuition Behind the Approach:**
- Kth from start is simple.
- Kth from end = Kth from start from the back side, so (N - K + 1)
- Once you find both nodes (A and B) and their previous nodes:
  * redirect the links
  * swap next pointers
- This swaps the nodes logically, not values.
- Swapping nodes in singly linked list = rewiring 4 pointers carefully.
- This is the exact trick used in many LL problems.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Because:
  * We traverse the list to reach A
  * Traverse again to reach B
  * Constant-time pointer swap
- 💾 Space Complexity
  * O(1)
  * No extra list or array used
  * Only pointer variables

---

## 6. Justification / Proof of Optimality

- Brute force wastes memory, not optimal
- Swapping values is disallowed
- This approach is perfect because:
  * pointer operations are constant time
  * handles all cases (head swap, adjacent nodes, etc.)
  * linear time, constant space
- This is the optimal and interview-preferred solution.
---

## 7. Variants / Follow-Ups

- Swap pairwise nodes (1↔2, 3↔4, …)
- Swap Kth nodes from beginning (only one side)
- Reverse nodes between two indices
- Swap nodes in circular linked list
- Swap two arbitrary nodes by value
---

## 8. Tips & Observations

- Always find prevA and prevB
- If swapping involves the head, update the head
- Check if A == B early
- Carefully handle the case when both nodes are adjacent
- Next pointer swap MUST happen after prev pointer swaps
---

<!-- #endregion -->
