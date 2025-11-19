<!-- #region 126-Flatten a Multilevel Doubly Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q126: Flatten a Multilevel Doubly Linked List</h1>

## 1. Problem Understanding

- You are given a multilevel doubly linked list, where each node contains:
  * next pointer
  * prev pointer
  * child pointer → may point to another doubly linked list
  * that child list may have its own child lists → forming a multilevel hierarchy
- Your task:
  * 👉 Flatten it into a single-level doubly linked list
  * 👉 Children should appear RIGHT AFTER their parent (depth-first)
  * 👉 All child pointers must be set to null
---

## 2. Constraints

- 2 ≤ nodes ≤ 100
- 1 ≤ number of child levels ≤ 10
- Values can be any integers
- Doubly linked list guarantees:
  * each node has prev and next
  * child list is also a doubly linked list
---

## 3. Edge Cases

- No child levels → output = original list
- Multiple children at different depths
- Deep nesting (up to 10 levels)
- Child list attached to middle or last node
- Only one node in a child list
- Already flattened → no change
---

## 4. Examples

```text
Example 1
1 2 3 4 5
      |
      8 9 10 11 12
Flattened:
1 2 3 8 9 10 11 12 4 5

Example 2
1 2 3 4 5
      |
      8 9 10 11 12
        |
        50 60
Flattened:
1 2 3 8 9 50 60 10 11 12 4 5
```

---

## 5. Approaches

### Approach 1: Brute Force (Repeated Splice Child Into Main List)

**Idea:**
- Whenever you find a child pointer:
  * detach child
  * insert it between curr and curr.next
  * continue scanning

**Steps:**
- At each node:
  * if no child → move right
  * if child exists → connect child, then move into child list
- ❌ Limitations
  * Harder to manage prev pointers
  * Requires constant pointer updates
  * Can get messy with deeper levels

**💭 Intuition Behind the Approach:**
- Direct insertion approach, but not the cleanest.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * (You still visit each node once)
- 💾 Space Complexity
  * O(1)

### Approach 2: DFS Flatten Using Recursion

**Idea:**
- Use recursion to flatten child lists first, then attach them:
- For each node curr:
  * Flatten the child list
  * Attach flattened child between curr and curr.next
  * Continue flattening from the rest of the list
- Flatten order = depth-first traversal.

**Steps:**
- Start at head
- For each node:
  * If child exists:
    * flatten that child (recursive call returns tail)
    * insert flattened child between current and next
    * update all pointers (prev and next)
    * continue from child's tail
  * Else: move forward
- Always return the tail of the flattened list

**Java Code:**
```java
public Node flatten(Node head) {
    if (head == null) return head;
    flattenDFS(head);
    return head;
}

private Node flattenDFS(Node curr) {
    Node last = curr;

    while (curr != null) {
        Node next = curr.next;

        // Case 1: No child → move normally
        if (curr.child == null) {
            last = curr;
            curr = next;
        } else {
            // Case 2: Child exists → flatten child first
            Node childHead = curr.child;
            Node childTail = flattenDFS(childHead);

            // Insert child between curr and next
            curr.next = childHead;
            childHead.prev = curr;

            curr.child = null; // remove child pointer

            if (next != null) {
                childTail.next = next;
                next.prev = childTail;
            }

            last = childTail;
            curr = next;
        }
    }

    return last; // tail of this flattened segment
}
```

**💭 Intuition Behind the Approach:**
- The structure is naturally a tree of linked lists
- Flattening is equivalent to:
  * Perform DFS, append child list immediately after parent, then return to parent.next
- Recursion preserves depth ordering:
- Parent → Child → Grandchild → Great-grandchild → Next sibling → ...
- Returning the tail lets us connect back to the main list easily.
- This matches how flattening must be done.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * Because:
    * Every node is visited exactly once
    * All pointer updates are constant time
    * No repeated scanning
- 💾 Space Complexity
  * O(H) where H ≤ 10 (max nesting depth)
  * Due to recursion stack
  * Effectively O(1) for practical constraints

### Approach 3: Recursion dry run

**Java Code:**
```java
🌳 RECURSION TREE (DFS Call Structure)

We start:

flattenDFS(1)

🔽 LEVEL 0 — Top-Level List
flattenDFS(1)
 ├── flattenDFS(2)
 │     ├── flattenDFS(3)
 │     │     ├── flattenDFS(8)     ← CHILD of 3
 │     │     │     ├── flattenDFS(9)
 │     │     │     │     ├── flattenDFS(50)   ← CHILD of 9
 │     │     │     │     │     ├── flattenDFS(60)
 │     │     │     │     │     └── return tail (60)
 │     │     │     │     ├── flattenDFS(10)
 │     │     │     │     ├── flattenDFS(11)
 │     │     │     │     ├── flattenDFS(12)
 │     │     │     │     └── return tail (12)
 │     │     │     └── return tail (12)
 │     │     ├── flattenDFS(4)
 │     │     ├── flattenDFS(5)
 │     │     └── return tail (5)
 │     └── return tail (5)
 └── return tail (5)

🧩 DETAILED EXPLANATION OF TREE NODES
1. flattenDFS(1)

No child → go next

2. flattenDFS(2)

No child → go next

3. flattenDFS(3)

HAS CHILD → call flattenDFS on child head (8)

🔽 LEVEL 1 — Child list of 3
flattenDFS(8)

No child → go next

flattenDFS(9)

HAS CHILD → call flattenDFS on child head (50)

🔽 LEVEL 2 — Child list of 9
flattenDFS(50)

No child → go next → flattenDFS(60)

flattenDFS(60)

No child

End of its chain → returns tail = 60

Return up:

flattenDFS(50) returns tail 60

🔼 Back to LEVEL 1
flattenDFS(9) continues:

After child flatten → continue to 10

flattenDFS(10)

No child → continue

flattenDFS(11)

No child

flattenDFS(12)

No child

End of chain → return tail = 12

Return up:

flattenDFS(9) returns tail 12

🔼 Back to LEVEL 0

After finishing child chain of 3, recursion continues:

flattenDFS(4)
flattenDFS(5)

Both have no children → return tail = 5

🌲 Final Recursion Tree (Clean Text Version)
flattenDFS(1)
  flattenDFS(2)
    flattenDFS(3)
      flattenDFS(8)
        flattenDFS(9)
          flattenDFS(50)
            flattenDFS(60)
              return 60
          flattenDFS(10)
          flattenDFS(11)
          flattenDFS(12)
          return 12
      flattenDFS(4)
      flattenDFS(5)
      return 5
return 5
```

---

## 6. Justification / Proof of Optimality

- DFS-based flattening is the cleanest
- Works for arbitrary nesting depth
- No complex pointer juggling
- Guarantees correct order
- Maintains all prev/next links properly
- Handles all edge cases elegantly
---

## 7. Variants / Follow-Ups

- Flatten a multilevel singly linked list
- Flatten a nested list (like LeetCode 341)
- Flatten a binary tree to a linked list
- Flatten directory-like structures
---

## 8. Tips & Observations

- Always clean child = null after merging
- Always update both prev and next pointers
- Return the tail of flattened list for reconnection
- DFS ensures correct depth-first order
- Handle curr.next != null carefully when reconnecting
- Tail tracking is crucial; don’t lose it
---

<!-- #endregion -->
