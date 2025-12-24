<!-- #region 239-Single Child Nodes -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q239: Single Child Nodes</h1>

## 1. Problem Statement

- Given a binary tree, traverse it using preorder traversal and print all nodes that have only one child.
  * Printing must be in preorder order
  * Each printed node should be on the same line separated by space
  * Input is already given as preorder traversal with n representing null
---

## 2. Problem Understanding

- A node is a single-child node if:
- It has exactly one child
- Either:
  * left != null && right == null
  * left == null && right != null
- We must print the child node, not the parent.
- Traversal order must be preorder:
  * Root → Left → Right
---

## 3. Constraints

- 1 <= numOfNodes <= 10^5
- -10^6 <= node.data <= 10^6
- Large input → recursion must be efficient
---

## 4. Edge Cases

- Empty tree → print nothing
- Single node tree → no output
- Skewed tree → many single-child nodes
- Only left or only right subtree
---

## 5. Examples

```text
Example 1
Input:
50 25 12 n n 37 30 n n n 75 62 n 70 n n 87 n n

Output:
30 70

Example 2
Input:
50 25 12 n n n 75 62 30 n n n 87 n n

Output:
12 30
```

---

## 6. Approaches

### Approach 1: Brute Force (Check Parent at Each Node)

**Idea:**
- At every node:
  * Check if exactly one child exists
  * If yes, print the existing child
  * Continue preorder traversal
- This is straightforward and already optimal in structure.

**Steps:**
- If node is null → return
- If node has only left child → print left child
- If node has only right child → print right child
- Recurse left
- Recurse right

**Java Code:**
```java
void printSingleChildNodes(TreeNode node) {
    if (node == null) return;

    if (node.left != null && node.right == null) {
        System.out.print(node.left.val + " ");
    } 
    else if (node.left == null && node.right != null) {
        System.out.print(node.right.val + " ");
    }

    printSingleChildNodes(node.left);
    printSingleChildNodes(node.right);
}
```

**💭 Intuition Behind the Approach:**
- The parent decides whether a node is single-child
- Preorder ensures correct output order
- We print the child, not the parent

**Complexity (Time & Space):**
- Time: O(N) — each node checked once
- Space: O(N) — recursion stack (skewed tree)

### Approach 2: Parent-Aware DFS (Conceptual Optimization)

**Idea:**
- Pass parent information to child:
  * If parent has only one child → current node qualifies
  * Print current node
  * Continue preorder traversal
- This makes the logic more intuitive, especially in interviews.

**Steps:**
- Pass parent to recursive call
- If parent exists and has only one child:
  * Print current node
- Recurse preorder

**Java Code:**
```java
void printSingleChild(TreeNode node, TreeNode parent) {
    if (node == null) return;

    if (parent != null) {
        if ((parent.left == node && parent.right == null) ||
            (parent.right == node && parent.left == null)) {
            System.out.print(node.val + " ");
        }
    }

    printSingleChild(node.left, node);
    printSingleChild(node.right, node);
}
Call using:

printSingleChild(root, null);
```

**💭 Intuition Behind the Approach:**
- A node becomes “single-child” because of its parent
- Passing parent avoids checking both children repeatedly
- Cleaner conceptual mapping

**Complexity (Time & Space):**
- Time: O(N) — one DFS
- Space: O(N) — recursion stack

---

## 7. Justification / Proof of Optimality

- Both approaches are linear
- Parent-aware DFS is more expressive
- Brute-force version is simpler and acceptable
---

## 8. Variants / Follow-Ups

- Print parent instead of child
- Count number of single-child nodes
- Store in list instead of printing
- Postorder version
---

## 9. Tips & Observations

- Always clarify: print child or parent?
- Preorder means print before recursion
- Parent context simplifies logic
---

## 10. Pitfalls

- Printing parent instead of child
- Printing in inorder/postorder accidentally
- Missing skewed-tree cases
- Forgetting null checks
---

<!-- #endregion -->
