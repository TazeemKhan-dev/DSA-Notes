<!-- #region 121-Check if LL is palindrome or not -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q121: Check if LL is palindrome or not</h1>

## 1. Problem Understanding

- You are given the head of a singly linked list in which each node contains a digit.
- Your task is to determine whether the entire linked list reads the same forward and backward, i.e., whether it’s a palindrome.
---

## 2. Constraints

- 1 <= nodes <= 100000
- Node values are digits 0–9
- No leading zeros
- Must be efficient: O(N) time → suitable for fast/slow pointer techniques.
---

## 3. Edge Cases

- Single node (always palindrome)
- Two nodes (check equality)
- Even-length palindrome vs odd-length palindrome
- Long list (must avoid recursion stack overflow)
---

## 4. Examples

```text
Example 1:

Input: 3 → 7 → 5 → 7 → 3
Output: true

Example 2:

Input: 1 → 1 → 2 → 1
Output: false

Example 3:

Input: 9 → 9 → 9 → 9
Output: true
```

---

## 5. Approaches

### Approach 1: Find Middle → Reverse Second Half → Compare

**Idea:**
- Use slow & fast pointers to reach the middle.
- Reverse the second half of the list.
- Compare first half and reversed second half.
- (Optional) Restore list to original.

**Steps:**
- Find middle using slow/fast.
- Reverse from middle onwards.
- Compare node-by-node.
- Return true if all matched.

**Java Code:**
```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) return true;

    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    ListNode secondHalf = reverse(slow);
    ListNode p1 = head, p2 = secondHalf;

    while (p2 != null) {
        if (p1.val != p2.val) return false;
        p1 = p1.next;
        p2 = p2.next;
    }

    return true;
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
- A palindrome mirrors around its center.
- So if you reverse the second half, the list should match from both ends.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Why?
    * One traversal to find middle → O(N)
    * One traversal to reverse → O(N)
    * One traversal to compare → O(N)
  * All sequential → still linear.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only pointer manipulation; no extra arrays/stacks.

### Approach 2: Copy List to Array → Two-Pointer Check

**Idea:**
- Copy all node values into an array.
- Use two-pointer technique (start ↔ end) to check palindrome.

**Steps:**
- Traverse list, push values into array.
- Use i=0, j=n-1:
  * if(arr[i] != arr[j]) return false
  * i++, j--
- Return true.

**Java Code:**
```java
public boolean isPalindrome(ListNode head) {
    List<Integer> arr = new ArrayList<>();
    while (head != null) {
        arr.add(head.val);
        head = head.next;
    }

    int i = 0, j = arr.size() - 1;

    while (i < j) {
        if (!arr.get(i).equals(arr.get(j))) return false;
        i++;
        j--;
    }

    return true;
}
```

**💭 Intuition Behind the Approach:**
- Array allows random access, so checking palindrome becomes trivial.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Why?
    * Fill array → O(N)
    * Two-pointer check → O(N)
    * Total still linear.
- 💾 Space Complexity
  * O(N)
  * Why?
    * We store all values explicitly in an array.

### Approach 3: Recursion (Reverse Simulation)

**Idea:**
- Not recommended for N=100000 (stack overflow possible)
- Use recursion to reach the last node.
- Maintain a global pointer moving forward to compare with backward recursion.

**Java Code:**
```java
ListNode left;

public boolean isPalindrome(ListNode head) {
    left = head;
    return helper(head);
}

private boolean helper(ListNode right) {
    if (right == null) return true;

    if (!helper(right.next)) return false;

    if (left.val != right.val) return false;

    left = left.next;
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Recursion unwinds from the tail → mimics reverse traversal.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Why?
    * Each node is visited twice (going down and coming back).
- 💾 Space Complexity
  * O(N) recursion stack
  * Why?
    * We call recursion once per node.

---

## 6. Justification / Proof of Optimality

- Palindromes rely on symmetry around the center.
- Checking this requires comparing mirrored positions.
- Among all methods:
- Reversal method gives best time & space (O(N) & O(1)).
- Array method is simplest but uses extra memory.
- Recursion is elegant but unsafe for large constraints.
---

## 7. Variants / Follow-Ups

- Check palindrome for doubly linked list
- Check palindrome for string or array
- Check palindrome after deleting at most 1 node
- Find longest palindromic sublist
---

## 8. Tips & Observations

- For interview: use Approach 1 (fast & slow pointers + reverse).
- Restore list after reversal if interviewer asks.
- Practice reversing sublists—it appears in many hard problems.
---

<!-- #endregion -->
