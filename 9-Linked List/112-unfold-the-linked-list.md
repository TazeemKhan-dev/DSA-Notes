<!-- #region 112-Unfold the Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q112: Unfold the Linked List</h1>

## 1. Problem Understanding

- You are given a folded linked list.
- Folding pattern:
  * Original:  L0 → L1 → L2 → L3 → L4 → L5 → ...
  * Folded:    L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...
- Your task is to unfold the list and reconstruct the original order.
---

## 2. Constraints

- 1 ≤ n ≤ 1000
- Linked list is singly linked
- Values are integers
- Folding is guaranteed valid
---

## 3. Edge Cases

- Single node → same list
- Two nodes → already unfolded
- Odd vs even number of nodes
- Repeated values
- Must preserve original order, not sorted order
---

## 4. Examples

```text
Example 1

Input:
1 6 2 5 3 4
Output:
1 2 3 4 5 6

Example 2

Input:
1 5 2 4 3
Output:
1 2 3 4 5
```

---

## 5. Approaches

### Approach 1: Using Two Lists (Optimal & Common Approach)

**Idea:**
- The folded list has two interleaved lists:
  * odd positions  → original left half
  * even positions → reversed right half
- Steps to restore original:
- Split into two lists:
  * left = L0 → L1 → L2 → ...
  * right = Ln → Ln-1 → ... (in reverse order)
- Reverse the right list
- Attach reversed right list to end of left list

**Steps:**
- Create two dummy lists: odd and even
- Traverse original list:
  * nodes at index 0,2,4… → odd
  * nodes at index 1,3,5… → even
- Reverse the even list
- Concatenate odd list + reversed even list
- Return head of odd list

**Java Code:**
```java
static Node unfold(Node head) {
    if (head == null || head.next == null)
        return head;

    Node oddHead = head;
    Node evenHead = head.next;

    Node odd = oddHead;
    Node even = evenHead;

    // Separate odd and even positioned nodes
    while (even != null && even.next != null) {
        odd.next = even.next;
        odd = odd.next;

        even.next = odd.next;
        even = even.next;
    }

    odd.next = null; // end odd list

    // Reverse even list
    Node revEven = reverse(evenHead);

    // Attach reversed even list
    odd.next = revEven;

    return oddHead;
}

// Standard reverse function
static Node reverse(Node head) {
    Node prev = null;
    Node curr = head;

    while (curr != null) {
        Node next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

**💭 Intuition Behind the Approach:**
- Folding mixes the list like:
  * L0, Ln, L1, Ln-1, L2, Ln-2 ...
- So:
  * Odd positioned elements produce the left-to-right order (partially correct)
  * Even positioned elements are in reverse order of the right half
- Thus:
  * Separate odd/even
  * Reverse even list
  * Attach
- This reconstructs the original order.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
  * Single traversal to split + single traversal to reverse + constant-time attach.
- 💾 Space Complexity
  * O(1)
  * Why?
  * Only pointers used; no extra lists except reconstructed pointer links.

### Approach 2: Using an Array (Simpler but Not Optimal)

**Idea:**
- Convert nodes into array, rearrange them into original order, rebuild list.

**Steps:**
- Put all nodes into array
- First half = original left half
- Second half = reversed right half
- Re-link nodes in order

**Java Code:**
```java
static Node unfold(Node head) {
    ArrayList<Node> arr = new ArrayList<>();
    Node curr = head;
    while (curr != null) {
        arr.add(curr);
        curr = curr.next;
    }

    int i = 0, j = arr.size() - 1;
    Node dummy = new Node(-1);
    Node tail = dummy;

    while (i <= j) {
        tail.next = arr.get(i++);
        tail = tail.next;
        if (i <= j) {
            tail.next = arr.get(i);
            i++;
        }
        if (i <= j) {
            tail.next = arr.get(j--);
            tail = tail.next;
        }
    }

    tail.next = null;
    return dummy.next;
}
```

**💭 Intuition Behind the Approach:**
- Using array, we can directly reorder nodes since we have random access.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
- O(n)
- 💾 Space Complexity
  * O(n)
  * Why?
    * Array stores all nodes.

---

## 6. Justification / Proof of Optimality

- The odd-even separation + reverse method is optimal because:
- Uses only pointer manipulation
- No additional memory
- Linear time
- Matches the exact fold/unfold logic
- This is the approach used in interviews and competitive programming.
---

## 7. Variants / Follow-Ups

- Fold a list
- Check if a list is folded
- Fold/unfold a doubly linked list
- Zig-zag rearrangement
- Rearrange list into L0 → Ln → L1 → Ln-1 …
- Merge two halves alternatively
---

## 8. Tips & Observations

- Always check for null before accessing next
- Use a dummy node for clarity in some variations
- Reversing even list is essential
- Odd-even logic ONLY works because fold is deterministic
- Avoid array unless memory is not a concern
---

<!-- #endregion -->
