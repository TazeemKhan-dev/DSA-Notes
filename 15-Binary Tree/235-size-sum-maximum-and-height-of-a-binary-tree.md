<!-- #region 235-Size, Sum, Maximum And Height Of A Binary Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q235: Size, Sum, Maximum And Height Of A Binary Tree</h1>

## 1. Problem Statement

- Given a binary tree, compute the following properties:
  * Size → total number of nodes
  * Sum → sum of all node values
  * Maximum → maximum value present in the tree
  * Height → maximum depth of the tree
- The tree is given in level order, where N represents a null node.
---

## 2. Problem Understanding

- Each property depends on all nodes
- Binary Tree naturally suggests DFS traversal
- The answer at a node depends on answers from:
  * left subtree
  * right subtree
  * 👉 This is a postorder DFS problem:
- left → right → root (compute & return)
---

## 3. Constraints

- 1 <= number of nodes <= 10000
- Node values are positive
- Tree may be skewed or balanced
---

## 4. Edge Cases

- Empty tree → size = 0, sum = 0, max = MIN, height = 0
- Single node tree
- Left-skewed / right-skewed tree
---

## 5. Examples

```text
Tree:
        1
      /   \
     2     3
    / \
   4   5

Size   = 5
Sum    = 15
Max    = 5
Height = 3
```

---

## 6. Approaches

### Approach 1: DFS Recursion (Optimal & Standard)

**Idea:**
- Visit every node exactly once
- Compute values bottom-up
- Combine left & right subtree results

**Steps:**
- If node is null, return base value
- Recursively compute left subtree
- Recursively compute right subtree
- Combine results at current node
- Return computed value

**Java Code:**
```java
🔸 1️⃣ Size of Binary Tree
🧪 Java Code
int size(Node root) {
    if (root == null) return 0;

    int left = size(root.left);
    int right = size(root.right);

    return left + right + 1;
}

💭 Intuition Behind the Approach

Size = nodes in left + nodes in right + current node

Every node contributes exactly 1

⏱️ Complexity

Time: O(N) — every node visited once

Space: O(H) — recursion stack

🔸 2️⃣ Sum of Binary Tree
🧪 Java Code
int sum(Node root) {
    if (root == null) return 0;

    int left = sum(root.left);
    int right = sum(root.right);

    return left + right + root.data;
}

💭 Intuition Behind the Approach

Sum accumulates bottom-up

Each node contributes its value once

⏱️ Complexity

Time: O(N)

Space: O(H)

🔸 3️⃣ Maximum in Binary Tree
🧪 Java Code
int max(Node root) {
    if (root == null) return Integer.MIN_VALUE;

    int left = max(root.left);
    int right = max(root.right);

    return Math.max(root.data, Math.max(left, right));
}

💭 Intuition Behind the Approach

Maximum must be one of:

current node

left subtree max

right subtree max

⏱️ Complexity

Time: O(N)

Space: O(H)

🔸 4️⃣ Height of Binary Tree
🧪 Java Code
int height(Node root) {
    if (root == null) return 0;

    int left = height(root.left);
    int right = height(root.right);

    return Math.max(left, right) + 1;
}

💭 Intuition Behind the Approach

Height = longest path from root to leaf

Current node adds 1 level

⏱️ Complexity

Time: O(N)

Space: O(H)
```

---

## 7. Justification / Proof of Optimality

- All four problems share:
  * same traversal
  * same recursion pattern
  * Only the return formula changes
- This is the base template for:
  * diameter
  * balance check
  * subtree problems
---

## 8. Variants / Follow-Ups

- Size of subtree
- Sum of subtree
- Height in terms of edges
- Maximum path problems
---

## 9. Tips & Observations

- These are postorder DFS problems
- Learn the template, not the individual problems
- Once this is clear:
  * Balanced Tree
  * Diameter
  * become easy
---

## 10. Pitfalls

- Wrong base value (0 vs MIN_VALUE)
- Forgetting +1 in height
- Mixing preorder logic
- Using global variables unnecessarily
---

<!-- #endregion -->
