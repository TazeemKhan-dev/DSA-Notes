<!-- #region 118-Flattening a Linked List -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q118: Flattening a Linked List</h1>

## 1. Problem Understanding

- You are given a linked list where:
  * Each node has 2 pointers:
    * right → points to next list head
    * down → points to a sorted sub-linked-list
- Every sub-list is sorted
- We must flatten all lists into a single sorted list, using only the down pointer.
- After flattening:
  * All nodes appear in a single down chain
  * Output is printed using the down pointer only
  * The right pointer should not be used in final list
- This is similar to merge K sorted linked lists.
---

## 2. Constraints

- Number of lists n ≤ 50
- Size of each sublist k ≤ 20
- Total nodes ≤ 1000
- Values ≤ 1000
- Must maintain sorted order
---

## 3. Edge Cases

- Empty main list (n = 0)
- Only 1 list
- Each list has only 1 element
- Some lists have size 1, others long
- All values identical
- Highly skewed down chains
---

## 4. Examples

```text
Example 1:

Flatten:

5 → 10 → 19 → 28
↓    ↓     ↓     ↓
7    20    22    35
↓           ↓     ↓
8           50    40
↓                 ↓
30                45


Output:

5 7 8 10 19 20 22 28 30 35 40 45 50

Example 2:

Output:

5 7 8 10 19 22 28 30 50
```

---

## 5. Approaches

### Approach 1: Merge down lists one by one (Optimal & Simple)

**Idea:**
- Treat each right sub-list as a sorted linked list.
- We repeatedly:
  * Flatten the right sublist
  * Merge the current list with the flattened right list
  * Return merged result
- This reduces to merge of K sorted lists using recursion.

**Steps:**
- Base case → if head == null or head.right == null, return head
- Recursively flatten the list starting from head.right
- Merge head and flatten(head.right)
- Ensure result uses only down pointers
- Set right = null to fully flatten structure

**Java Code:**
```java
Merge two sorted “down” linked lists
static Node merge(Node a, Node b) {
    if (a == null) return b;
    if (b == null) return a;

    Node result;

    if (a.data < b.data) {
        result = a;
        result.down = merge(a.down, b);
    } else {
        result = b;
        result.down = merge(a, b.down);
    }

    result.right = null; // ensure right pointers removed
    return result;
}

Flatten function
static Node flatten(Node root) {
    if (root == null || root.right == null)
        return root;

    // Flatten the right side first
    root.right = flatten(root.right);

    // Merge current list with flattened right list
    root = merge(root, root.right);

    return root;
}
```

**💭 Intuition Behind the Approach:**
- Each node’s “down” is a sorted list.
- The main list is: L1 -> L2 -> L3 -> ... -> Ln
- Flattening means merging these lists:
- merge(L1, merge(L2, merge(L3, ... )))
- Merging two sorted lists is O(N)
- Using recursion ensures gradually flattening from right → left
- The right pointer is ignored in final flattened list
- This is efficient and clean.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Let total nodes = N
    * Each merge operation = O(N)
    * Number of merges = n (number of lists ≤ 50)
  * Overall:
    * Time: O(N * log n) (if optimized)
    * Simple recursive merge gives O(N * n) (still fine for constraints)
- 💾 Space Complexity
  * Recursion depth = O(n) ≤ 50
  * No extra linked lists created
  * Uses same nodes → in-place flattening

---

## 6. Justification / Proof of Optimality

- The problem requires flattening multiple sorted linked lists into one sorted list.
- The optimal way to combine sorted lists is through merge operations, just like merging in Merge Sort.
- Since each node’s down list is already sorted, merging two lists using the down pointer preserves sorted order without extra space.
- Recursively flattening the right side first ensures we gradually combine all lists from right to left, simplifying structure.
- Eliminating right pointers and relying only on down ensures the final flattened list is a single-level sorted list, exactly as required.
- This approach avoids creating new nodes, ensuring in-place, memory-efficient flattening.
- The merge-based method maintains optimal time complexity and is the standard recommended solution for this problem.
---

## 7. Variants / Follow-Ups

- Flatten a multilevel doubly linked list
- Merge K sorted linked lists (priority queue)
- Flatten a tree-like structure
---

## 8. Tips & Observations

- Always merge using down pointer only
- Merge like merging 2 sorted linked lists
- Set right = null to avoid mixing structures
- The flatten process works right → left
- Base case simplifies recursion
- Print using down pointer always
- Do not create new nodes (in-place merge required)
---

<!-- #endregion -->
