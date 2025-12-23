<!-- #region 234-Binary Tree Traversals (Preorder, Inorder, Postorder) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q234: Binary Tree Traversals (Preorder, Inorder, Postorder)</h1>

## 1. Problem Statement

- Given the root of a Binary Tree, perform the following Depth First Traversals:
  * Preorder Traversal
  * Inorder Traversal
  * Postorder Traversal
- Each traversal visits all nodes of the tree exactly once but in a different order.
---

## 2. Problem Understanding

- A binary tree node has:
  * a value
  * a left child
  * a right child
- Traversal means visiting every node exactly once following a specific order.
- The only difference between preorder, inorder, and postorder is:
  * When the root node is processed relative to its children
---

## 3. Constraints

- Tree can be empty
- Number of nodes can be large
- Recursive solution is expected
- Stack depth depends on tree height
---

## 4. Edge Cases

- Empty tree → no output
- Single node tree
- Left-skewed tree
- Right-skewed tree
---

## 5. Examples

```text
For the tree:

      1
     / \
    2   3
   / \
  4   5


Preorder → 1 2 4 5 3

Inorder → 4 2 5 1 3

Postorder → 4 5 2 3 1
```

---

## 6. Approaches

### Approach 1: DFS using Recursion (Foundation)

**Idea:**
- This is the base approach for all three traversals.

**Java Code:**
```java
🔸 Variant 1: Preorder Traversal

Order: Root → Left → Right

🧪 Java Code
void preorder(Node root) {
    if (root == null) return;

    System.out.print(root.data + " ");
    preorder(root.left);
    preorder(root.right);
}

💭 Intuition Behind the Approach

Process root before children

Useful when:

copying tree

prefix expressions

serialization

🔸 Variant 2: Inorder Traversal

Order: Left → Root → Right

🧪 Java Code
void inorder(Node root) {
    if (root == null) return;

    inorder(root.left);
    System.out.print(root.data + " ");
    inorder(root.right);
}

💭 Intuition Behind the Approach

Root processed between left and right

In BST, inorder gives sorted order

Most intuitive traversal

🔸 Variant 3: Postorder Traversal

Order: Left → Right → Root

🧪 Java Code
void postorder(Node root) {
    if (root == null) return;

    postorder(root.left);
    postorder(root.right);
    System.out.print(root.data + " ");
}

💭 Intuition Behind the Approach

Root processed after children

Ideal for:

deleting tree

computing height, diameter

bottom-up problems
```

**Complexity (Time & Space):**
- Time: O(N) — each node visited once
- Space: O(H) — recursion stack (H = height of tree)

---

## 7. Justification / Proof of Optimality

- DFS recursion naturally fits tree structure
- Same base logic reused for all traversals
- Only print position changes
- This is the foundation of all tree problems
---

## 8. Variants / Follow-Ups

- Iterative traversals using stack
- Morris Traversal (O(1) space)
- Level Order Traversal (BFS – queue based)
---

## 9. Tips & Observations

- Traversals differ only in root position
- Memorize as:
- Pre  → Root first
- In   → Root middle
- Post → Root last
- Postorder is key for advanced tree problems
---

## 10. Pitfalls

- Printing root at wrong position
- Forgetting base case
- Confusing inorder with preorder
- Assuming inorder is always sorted (only true for BST)
---

<!-- #endregion -->
