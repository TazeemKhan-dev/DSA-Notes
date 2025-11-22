<!-- #region 117-Clone a Linked List with Next and Random Pointer -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q117: Clone a Linked List with Next and Random Pointer</h1>

## 1. Problem Understanding

- We are given a linked list where each node has:
  * next pointer → points to next node
  * random pointer → points to ANY node in the list (or NULL)
- We must create a deep copy:
  * Create N new nodes
  * Each new node:
    * Has the same value
    * next and random pointers must point to new nodes only
  * No pointer of the cloned list should reference original nodes.
- This is a classic Random Pointer Linked List Cloning problem.
---

## 2. Constraints

- 1 <= N <= 100
- 1 <= M <= N
- Random pointer pairs given as:
  * a b meaning → node a has random pointer to node b
- Random pointer can be missing → treated as NULL
- Must return head of cloned list
---

## 3. Edge Cases

- Only 1 node
- No random pointers
- All random pointers NULL
- Random pointer pointing to itself
- Multiple nodes pointing to same random target
- Random pointers forming cycles
- Last node having random pointer
- Random pointers out of order (given as values not linked structure)
---

## 4. Examples

```text
Example 1
1 -> 2 -> 3 -> 4
1->random = 2
2->random = 4


Output: 1 (means clone is valid)

Example 2
1 -> 3 -> 5 -> 9
1->random = 1
3->random = 4


Output: 1
```

---

## 5. Approaches

### Approach 1: O(1) Extra Space — Interleaving Nodes (Optimal)

**Idea:**
- Clone each node and insert it next to original node:
  * 1 -> 1' -> 2 -> 2' -> 3 -> 3' ...
- Set all cloned nodes' random pointers by using:
  * clone.random = original.random.next
- Detach cloned list from original list.

**Steps:**
- Step 1 — Insert clone nodes after each original
  * For each node A, create A':
  * A -> A' -> B -> B' -> C -> C' ...
- Step 2 — Set random pointer of cloned nodes
  * If A.random = R
  * then
    * A'.random = R.next
  * Because R.next → R’s clone.
- Step 3 — Detach cloned list
  * Extract all A' nodes into a separate cloned list.

**Java Code:**
```java
static Node cloneLinkedList(Node head) {
    if (head == null) return null;

    Node curr = head;

    // Step 1: Insert cloned nodes
    while (curr != null) {
        Node next = curr.next;
        Node copy = new Node(curr.data);
        curr.next = copy;
        copy.next = next;
        curr = next;
    }

    // Step 2: Set random pointers
    curr = head;
    while (curr != null) {
        if (curr.random != null)
            curr.next.random = curr.random.next;
        curr = curr.next.next;
    }

    // Step 3: Separate cloned list
    curr = head;
    Node copyHead = head.next;
    Node copy = copyHead;

    while (curr != null) {
        curr.next = curr.next.next;
        if (copy.next != null)
            copy.next = copy.next.next;

        curr = curr.next;
        copy = copy.next;
    }

    return copyHead;
}
```

**💭 Intuition Behind the Approach:**
- We avoid extra space like HashMap by creating a twin node right next to original.
- This interleaving gives us direct access to cloned node of any node using curr.next.
- Since random pointers can point anywhere, interleaving ensures we can assign clone-randoms in O(1) time.
- Finally, we untangle both lists to restore the original and produce the clone.
- This is efficient and elegant.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
  * (Every node visited a constant number of times)
- 💾 Space Complexity
  * O(1) extra space
  * (No hashing, no extra arrays)

### Approach 2: HashMap Method (Simpler but uses O(N) space)

**Idea:**
- Create all new nodes in first pass
- Store mapping: oldNode → newNode
- Second pass: assign next & random using map

**Java Code:**
```java
HashMap<Node, Node> map = new HashMap<>();

Node curr = head;
while (curr != null) {
    map.put(curr, new Node(curr.data));
    curr = curr.next;
}

curr = head;
while (curr != null) {
    map.get(curr).next = map.get(curr.next);
    map.get(curr).random = map.get(curr.random);
    curr = curr.next;
}

return map.get(head);
```

---

## 6. Justification / Proof of Optimality

- Interleaving method:
  * Uses constant space
  * Solves random pointer linking efficiently
  * Avoids extra memory overhead
  * Is the most optimal approach taught in interviews
---

## 7. Variants / Follow-Ups

- Clone a doubly linked list with random pointers
- Clone a tree with random pointers
- Clone graph using BFS/DFS
- Create deep copy of complex structures
---

## 8. Tips & Observations

- Interleaving list ensures random pointers can be assigned without storing mappings
- Always detach the lists at the end, or you will corrupt original list
- Use .next.next carefully during traversal
- Random pointer may be NULL → handle that condition
- This question appears frequently in FAANG interviews
---

<!-- #endregion -->
