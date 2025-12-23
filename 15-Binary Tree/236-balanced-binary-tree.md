<!-- #region 236-Balanced Binary Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q236: Balanced Binary Tree</h1>

## 1. Problem Statement

- Given a binary tree, determine whether it is height-balanced.
- A binary tree is height-balanced if for every node, the absolute difference between the heights of its left and right subtrees is less than or equal to 1.
---

## 2. Problem Understanding

- You must check every node, not just the root.
- Height of a node = number of edges on the longest path from that node to a leaf.
- A tree is balanced only if all subtrees are balanced.
- Even one violation makes the whole tree unbalanced.
---

## 3. Constraints

- 0 <= n <= 5000
- -10^4 <= node.val <= 10^4
---

## 4. Edge Cases

- Empty tree → balanced
- Single node → balanced
- Skewed tree → not balanced
- Balanced at root but unbalanced inside → not balanced
---

## 5. Examples

```text
Example 1
Input

5
3 9 20 15 7
Output

false
Explanation

Left and right subtree difference for atleast one node is > 1

Example 2
Input

7
1 2 2 3 3 4 4
Output

false
Explanation

Left and right subtree difference for node 1 itself is 2 (> 1)
```

---

## 6. Approaches

### Approach 1: Brute Force 

**Idea:**
- For every node:
  * Compute height of left subtree
  * Compute height of right subtree
  * Check abs(leftHeight - rightHeight) <= 1
  * Recursively check left and right children

**Steps:**
- If node is null, return true
- Find height of left subtree
- Find height of right subtree
- If difference > 1 → return false
- Recursively check left and right subtrees

**Java Code:**
```java
boolean isBalanced(TreeNode root) {
    if (root == null) return true;

    int lh = height(root.left);
    int rh = height(root.right);

    if (Math.abs(lh - rh) > 1) return false;

    return isBalanced(root.left) && isBalanced(root.right);
}

int height(TreeNode node) {
    if (node == null) return 0;
    return 1 + Math.max(height(node.left), height(node.right));
}
```

**💭 Intuition Behind the Approach:**
- You directly follow the definition of a balanced tree:
  * Measure heights
  * Compare them
  * Repeat for all nodes
- Simple, readable, but inefficient.

**Complexity (Time & Space):**
- Time: O(N^2) — height is recalculated for every node
- Space: O(N) — recursion stack in worst case (skewed tree)

### Approach 2: Optimal DFS 

**Idea:**
- While computing height:
  * If any subtree is unbalanced, stop immediately
  * Use a special return value (-1) to signal imbalance

**Steps:**
- If node is null, return height 0
- Get left subtree height
- If -1, propagate -1
- Get right subtree height
- If -1, propagate -1
- If abs(left - right) > 1, return -1
- Else return 1 + max(left, right)
- Final check:
  * If returned value is -1 → not balanced
  * Else → balanced

**Java Code:**
```java
boolean isBalanced(TreeNode root) {
    return dfsHeight(root) != -1;
}

int dfsHeight(TreeNode node) {
    if (node == null) return 0;

    int left = dfsHeight(node.left);
    if (left == -1) return -1;

    int right = dfsHeight(node.right);
    if (right == -1) return -1;

    if (Math.abs(left - right) > 1) return -1;

    return 1 + Math.max(left, right);
}
```

**💭 Intuition Behind the Approach:**
- Balance checking depends on height
- So combine both in one recursion
- Early termination avoids unnecessary work
- Bottom-up ensures correctness
- This is how DFS tree problems should be solved.

**Complexity (Time & Space):**
- Time: O(N) — each node visited once
- Space: O(N) — recursion stack

---

## 7. Justification / Proof of Optimality

- Brute force works but recalculates heights repeatedly
- Optimal approach avoids recomputation
- Interviewers expect the second approach
---

## 8. Variants / Follow-Ups

- Return a Pair(isBalanced, height)
- Iterative postorder using stack (rarely asked)
- Check balance while building tree
---

## 9. Tips & Observations

- Any tree problem involving height → think bottom-up
- Use sentinel values like -1 to propagate failure
- Never compute height separately in balance problems (unless asked)
---

## 10. Pitfalls

- Checking balance only at root
- Recomputing height again and again
- Forgetting early exit
- Confusing node height vs tree height
---

<!-- #endregion -->
