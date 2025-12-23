<!-- #region 238-Tree Level Order Traversal -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q238: Tree Level Order Traversal</h1>

## 1. Problem Statement

- You are given n values.
- Insert them one by one into a Binary Search Tree (BST).
- After constructing the BST, print its Level Order Traversal (Breadth-First Traversal).
---

## 2. Problem Understanding

- First, you must form a BST, not a normal binary tree.
- BST rule:
  * Left child < root
  * Right child > root
- Once the BST is built:
  * Traverse the tree level by level
  * Process nodes left to right at each level
- Output is a single line of values
---

## 3. Constraints

- 1 <= n <= 500
- -1000 <= node.value <= 1000
---

## 4. Edge Cases

- n = 1 → output is the single node
- Sorted input → skewed BST
- Negative values allowed
- Duplicate values → usually ignored or handled by convention (assume unique here)
---

## 5. Examples

```text
Input

6
1 2 5 3 4 6


BST formed

     1
      \
       2
        \
         5
        /  \
       3    6
        \
         4


Output

1 2 5 3 6 4
```

---

## 6. Approaches

### Approach 1: Brute Force (Height-Based Level Traversal)

**Idea:**
- First compute height of BST
- For each level from 1 → height:
  * Print nodes at that level using rec

**Steps:**
- Insert nodes to build BST
- Compute height of tree
- For each level i:
  * Recursively print nodes at level i

**Java Code:**
```java
void levelOrderBrute(TreeNode root) {
    int h = height(root);
    for (int i = 1; i <= h; i++) {
        printLevel(root, i);
    }
}

void printLevel(TreeNode node, int level) {
    if (node == null) return;
    if (level == 1) {
        System.out.print(node.val + " ");
    } else {
        printLevel(node.left, level - 1);
        printLevel(node.right, level - 1);
    }
}

int height(TreeNode node) {
    if (node == null) return 0;
    return 1 + Math.max(height(node.left), height(node.right));
}
```

**💭 Intuition Behind the Approach:**
- Level order means printing nodes level by level
- Height tells how many levels exist
- Recursion helps reach exact levels

**Complexity (Time & Space):**
- Time: O(N^2) — each level traversal re-visits nodes
- Space: O(N) — recursion stack
- ❌ Inefficient for skewed trees

### Approach 2: Optimal (Queue / BFS)

**Idea:**
- Level Order Traversal is Breadth-First Search
- Use a Queue
- Push root → process → push children

**Steps:**
- Insert nodes to form BST
- Create a queue
- Add root to queue
- While queue is not empty:
  * Pop front node
  * Print it
  * Push left child (if exists)
  * Push right child (if exists)

**Java Code:**
```java
void levelOrder(TreeNode root) {
    if (root == null) return;

    Queue<TreeNode> q = new ArrayDeque<>();
    q.add(root);

    while (!q.isEmpty()) {
        TreeNode curr = q.poll();
        System.out.print(curr.val + " ");

        if (curr.left != null) q.add(curr.left);
        if (curr.right != null) q.add(curr.right);
    }
}
```

**💭 Intuition Behind the Approach:**
- Queue naturally follows FIFO
- Exactly matches “process level by level”
- No repeated traversal
- Clean and fast

**Complexity (Time & Space):**
- Time: O(N) — every node visited once
- Space: O(N) — queue holds at most one level

---

## 7. Justification / Proof of Optimality

- Brute force works but repeats work
- BFS directly matches level order definition
- Queue-based approach is standard and expected
---

## 8. Variants / Follow-Ups

- Level order line by line
- Reverse level order
- Zigzag traversal
- Print level-wise sums
---

## 9. Tips & Observations

- Level Order = BFS = Queue
- Height-based traversal is almost always suboptimal
- BST property is irrelevant after construction for traversal
---

## 10. Pitfalls

- Forgetting to build BST first
- Using DFS instead of BFS
- Printing newline after every level (not required here)
- Ignoring null root case
---

<!-- #endregion -->
