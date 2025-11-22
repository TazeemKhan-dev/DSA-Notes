<!-- #region 119-Detect a loop in LL -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q119: Detect a loop in LL</h1>

## 1. Problem Understanding

- You are given the head of a singly linked list.
- You must return true if the linked list contains a cycle/loop, otherwise return false.
- A loop means some node’s next pointer points back to a previous node, causing infinite traversal.
---

## 2. Constraints

- Linked list size can be large → Solution must be O(n) or better.
- Node values may be anything (don’t rely on value similarity).
- Must not modify the list.
- Detecting a loop must be memory-efficient (preferably O(1) space).
---

## 3. Edge Cases

- head == null → No loop.
- Single node with next == null → No loop.
- Single node with next pointing to itself → Loop exists.
- Very long lists → Must avoid using extra space (like HashSet) in optimized solution.
---

## 4. Examples

```text
Example 1

Input: 1 → 2 → 3 → 4 → 5, with tail connecting to index 1
Output: true

Example 2

Input: 1 → 3 → 7 → 4, pos = -1
Output: false

Example 3

Input: 6 → 3 → 7, with tail connecting to index 0
Output: true
```

---

## 5. Approaches

### Approach 1: HashSet Method (Simple but uses extra space) 

**Idea:**
- Traverse the linked list while storing visited nodes in a HashSet.
- If we visit a node that already exists in the set → cycle detected.

**Steps:**
- Create an empty HashSet<Node>.
- Traverse the list node by node.
- If a node appears again → return true.
- If traversal reaches null → return false.

**Java Code:**
```java
public boolean hasCycle(ListNode head) {
    HashSet<ListNode> set = new HashSet<>();
    ListNode curr = head;
    while(curr != null){
        if(set.contains(curr)){
            return true;
        }
        set.add(curr);
        curr = curr.next;
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- We “mark” every node as visited.
- If we reach a node twice, that means next pointers are looping.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
- 💾 Space Complexity
  * O(n) (due to HashSet)

### Approach 2: Floyd’s Cycle Detection (Tortoise & Hare) — Optimal 🔥

**Idea:**
- Use two pointers:
- slow → moves 1 step
- fast → moves 2 steps
- If there is a loop, they will eventually meet inside the cycle.

**Steps:**
- Initialize slow = head, fast = head.
- Move slow by 1 and fast by 2 until:
  * They meet → loop exists.
  * fast or fast.next becomes null → no loop.

**Java Code:**
```java
public boolean hasCycle(ListNode head) {
    if(head == null || head.next == null) return false;

    ListNode slow = head;
    ListNode fast = head;

    while(fast != null && fast.next != null){
        slow = slow.next;
        fast = fast.next.next;

        if(slow == fast){
            return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Imagine two runners on a circular track:
- The fast runner will eventually lap the slow runner if a cycle exists.
- If the track is straight (no loop), the fast runner will reach the end.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
- 💾 Space Complexity
  * O(1) (best possible)

---

## 6. Justification / Proof of Optimality

- Floyd’s algorithm is optimal because:
  * It uses constant extra space (O(1)), better than HashSet (O(n)).
  * Guaranteed detection due to mathematical properties of relative speeds.
  * Works in a single pass (O(n)).
---

## 7. Variants / Follow-Ups

- Detect loop and find the starting node of the loop (Floyd’s extension).
- Detect loop and remove the loop.
- Count length of the loop.
---

## 8. Tips & Observations

- HashSet approach is easiest for beginners.
- Floyd’s algorithm is preferred in interviews.
- Always check fast != null and fast.next != null to avoid null pointer errors.
---

<!-- #endregion -->
