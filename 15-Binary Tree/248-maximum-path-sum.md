<!-- #region 248-Maximum Path Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q248: Maximum Path Sum</h1>

## 1. Problem Statement

- You are given the root of a binary tree.
- Your task is to find the maximum path sum in the tree.
- Definition of a Path
  * A path can start and end at any node.
  * The path must follow parent–child connections.
  * A node can be included only once in a path.
  * The path does not need to pass through the root.
- Return an integer representing the maximum possible sum of any such path.
---

## 2. Problem Understanding

- Each node has an integer value (can be negative).
- A path can be:
  * A single node
  * Left → node
  * Right → node
  * Left → node → right (important case)
- At every node, we must decide:
  * Should we include the left path?
  * Should we include the right path?
  * Or should we ignore negative contributions?
- Key observation:
  * The answer path may pass through a node and use both children.
  * But when returning to parent, we can only extend one direction (left or right).
---

## 3. Constraints

- 1 <= number of nodes <= 1000
- -1000 <= node.val <= 1000
---

## 4. Edge Cases

- All node values are negative
- Single node tree
- Tree with negative subtrees
- Maximum path does not include the root
- Skewed trees (left or right)
---

## 5. Examples

```text
Example 1

Input

1
1 2 3 N N N N


Output

6


Explanation

    1
   / \
  2   3


Maximum path: 2 → 1 → 3
Sum = 6

Example 2

Input

1
5 4 3 -5 N N N N N


Output

12


Explanation

      5
    /   \
   4     3
  /
 -5


Maximum path: 4 → 5 → 3
Sum = 12
```

---

## 6. Approaches

### Approach 1: DFS with Global Maximum (Optimal)

**Idea:**
- At every node:
  * Compute the maximum downward path sum.
  * Update a global maximum considering:
    * left + node + right
  * While returning to parent:
    * Only one side can be chosen.
- Negative sums are ignored using max(0, ...).

**Steps:**
- Use DFS recursion
- For each node:
  * Compute left contribution
  * Compute right contribution
- Update global answer:
  * maxSum = max(maxSum, left + node.val + right)
- Return:
  * node.val + max(left, right)

**Java Code:**
```java
static int maxSum;

static int maxPathSum(TreeNode root) {
    maxSum = Integer.MIN_VALUE;
    dfs(root);
    return maxSum;
}

static int dfs(TreeNode node) {
    if (node == null) return 0;

    int left = Math.max(0, dfs(node.left));
    int right = Math.max(0, dfs(node.right));

    // Path passing through this node
    maxSum = Math.max(maxSum, left + right + node.val);

    // Return max path extending upwards
    return node.val + Math.max(left, right);
}
```

**💭 Intuition Behind the Approach:**
- A negative path reduces total sum → discard it.
- A node can be a turning point (left + node + right).
- Parent can only extend one direction, not both.

**Complexity (Time & Space):**
- Time: O(n) — each node visited once
- Space: O(n) — recursion stack

### Approach 2: Postorder DP Explanation (Same Logic, Different View)

**Idea:**
- Think in DP terms:
  * dp(node) = max path sum starting at node and going down.
  * Global answer tracks best V-shaped path.

**Steps:**
- Postorder traversal
- Compute dp values
- Update global maximum

**Java Code:**
```java
Code will be the same as approac 2

Then why did I list them as two approaches?

Because interviewers care about explanation, not just code.

🔹 Approach 2: DFS with Global Maximum

Explanation style:

“I do a DFS. At each node, I calculate left and right contributions and update a global maximum.”

This is a procedural explanation.

🔹 Approach 3: Postorder DP

Explanation style:

“For every node, I define a DP value representing the maximum downward path starting at that node.
While computing DP, I update the global maximum considering both children.”

This is a DP formulation.

Think of it like this (important)
View	What changes
Code	❌ Nothing
Logic	❌ Nothing
Explanation	✅ Yes
Interview clarity	✅ Big improvement

Same algorithm, two mental models.

Why interviewers like the DP explanation

Because you clearly answer:

What is the state?
dp(node) = max path sum starting at node

What is the transition?
node.val + max(left, right)

Why global max?
Because best path may split at a node

That sounds much more solid than “just DFS”.

Final takeaway (remember this line)

Maximum Path Sum is a postorder DP problem disguised as a DFS problem.
```

**💭 Intuition Behind the Approach:**
- Postorder ensures children are solved before parent.

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

---

## 7. Justification / Proof of Optimality

- Each node is evaluated as a potential highest point of a path.
- Ignoring negative contributions guarantees optimality.
- DFS ensures linear traversal.
---

## 8. Variants / Follow-Ups

- Maximum Path Sum from Root to Leaf
- Maximum Sum BST in Binary Tree
- Diameter of Binary Tree (similar structure)
- Maximum Subtree Sum
---

## 9. Tips & Observations

- Always initialize global max to Integer.MIN_VALUE
- Use Math.max(0, childSum) to drop negative paths
- Path ≠ root to leaf
- Very common FAANG + interview favorite
---

## 10. Pitfalls

- Returning left + right + node.val to parent ❌
- Not handling all-negative trees
- Forgetting global variable
- Confusing with root-to-leaf sum
---

<!-- #endregion -->
