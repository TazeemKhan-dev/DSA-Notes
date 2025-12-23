<!-- #region 237-Diameter of a Binary Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q237: Diameter of a Binary Tree</h1>

## 1. Problem Statement

- Given the root of a binary tree, return the diameter of the tree.
- The diameter is defined as the length of the longest path between any two nodes in the tree.
- This path may or may not pass through the root.
  * Length here = number of nodes on the path (based on examples).
---

## 2. Problem Understanding

- The longest path can:
  * pass through root
  * lie completely in left subtree
  * lie completely in right subtree
- So checking only the root is insufficient
- At every node, the possible diameter passing through it is:
  * leftHeight + rightHeight + 1
---

## 3. Constraints

- 1 <= N <= 10^5
- Large input → must be O(N)
---

## 4. Edge Cases

- Single node → diameter = 1
- Skewed tree → diameter = height
- Tree where longest path lies entirely in one subtree
---

## 5. Examples

```text
Example 1
Input:
8 2 1 3 N N 5

Output:
5


Longest path: 3 → 2 → 8 → 1 → 5

👉 Does NOT depend only on root’s immediate left & right heights.

Example 2
Input:
1 2 N

Output:
2

❌ Why “just leftHeight + rightHeight” is WRONG

Consider this tree:

      1
     /
    2
   /
  3
 /
4


At root:

leftHeight = 3

rightHeight = 0

diameter = 4 ❌

But actual diameter is 4 → 3 → 2 → 1 = 4 nodes
(root happened to work here, but not always)

Now this:

        1
       /
      2
     / \
    3   4
   /
  5


Root-based calc misses 5 → 3 → 2 → 4

Diameter lies inside subtree, not root

👉 Hence: must check every node
```

---

## 6. Approaches

### Approach 1: Brute Force

**Idea:**
- For every node:
  * Calculate height of left subtree
  * Calculate height of right subtree
  * Diameter at that node = leftHeight + rightHeight + 1
  * Take max over all nodes

**Java Code:**
```java
int diameter(TreeNode root) {
    if (root == null) return 0;

    int lh = height(root.left);
    int rh = height(root.right);

    int self = lh + rh + 1;

    int leftDia = diameter(root.left);
    int rightDia = diameter(root.right);

    return Math.max(self, Math.max(leftDia, rightDia));
}

int height(TreeNode node) {
    if (node == null) return 0;
    return 1 + Math.max(height(node.left), height(node.right));
}
```

**💭 Intuition Behind the Approach:**
- Tries all possible nodes as the “center” of diameter
- Follows the definition directly

**Complexity (Time & Space):**
- Time: O(N^2) — height recalculated for each node
- Space: O(N) — recursion stack
- ❌ Too slow for N = 10^5

### Approach 2: Optimal DFS (Single Traversal) ✅

**Idea:**
- Compute height bottom-up
- While returning height, update global diameter
- Each node contributes:
  * diameterThroughNode = leftHeight + rightHeight + 1

**Steps:**
- DFS recursively
- If node is null → height = 0
- Get left height
- Get right height
- Update diameter using lh + rh + 1
- Return 1 + max(lh, rh)

**Java Code:**
```java
int diameter = 0;

int height(TreeNode node) {
    if (node == null) return 0;

    int left = height(node.left);
    int right = height(node.right);

    diameter = Math.max(diameter, left + right + 1);

    return 1 + Math.max(left, right);
}

int diameterOfBinaryTree(TreeNode root) {
    diameter = 0;
    height(root);
    return diameter;
}
```

**💭 Intuition Behind the Approach:**
- Diameter depends on heights
- Heights depend on children
- So calculate everything in one DFS
- Each node is treated as a potential “center” of the diameter
- This is a classic postorder DFS pattern.

**Complexity (Time & Space):**
- Time: O(N) — each node visited once
- Space: O(N) — recursion stack in worst case

---

## 7. Justification / Proof of Optimality

- Root-only logic fails when diameter lies in subtree
- Optimal DFS ensures all nodes are checked
- One traversal → efficient and correct
---

## 8. Variants / Follow-Ups

- Diameter in terms of edges → return diameter - 1
- Return Pair(height, diameter)
- Iterative postorder (rare)
---

## 9. Tips & Observations

- Any problem mixing height + global value → think postorder DFS
- Diameter ≠ height
- Root is not special unless problem says so
---

## 10. Pitfalls

- Checking diameter only at root
- Forgetting +1 (node count)
- Confusing edge-based vs node-based diameter
- Recomputing height repeatedly
---

<!-- #endregion -->
