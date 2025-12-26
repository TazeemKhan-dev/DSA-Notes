<!-- #region 246-Maximum Width of tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q246: Maximum Width of tree</h1>

## 1. Problem Statement

- Given the root of a binary tree, return the maximum width of the given tree.
- The maximum width of a tree is defined as the maximum width among all levels.
- The width of one level is defined as the distance between the leftmost and rightmost non-null nodes, inclusive.
- Importantly, null nodes between these end-nodes are also counted, assuming the tree is extended as a complete binary tree up to that level.
---

## 2. Problem Understanding

- We are given a binary tree.
- For each level:
  * We need to measure how wide that level is.
  * Width is not just the number of nodes present.
- Missing (null) nodes between two valid nodes must be counted.
- To handle this correctly:
  * Each node must be assigned a position index as if the tree were complete.
- Key insight:
  * Width of a level =
    * index_of_rightmost_node − index_of_leftmost_node + 1
---

## 3. Constraints

- 1 <= n <= 10^5 (based on typical input size)
- Node values can be any integer
- Tree is not necessarily complete or balanced
---

## 4. Edge Cases

- Empty tree → width 0
- Single node tree → width 1
- Skewed tree (all left or all right)
- Sparse tree with large gaps between nodes
- Large depth → index values can grow big (use long)
---

## 5. Examples

```text
Example 1

Input

7
1 2 3 4 5 6 7


Output

4


Explanation

Tree:

            1
          /   \
         2     3
        / \   / \
       4   5 6   7


Maximum width is at level 3 → {4,5,6,7} → width = 4

Example 2

Input

4
1 2 3 4


Output

2


Explanation

Tree:

            1
          /   \
         2     3
        /
       4


Second level has nodes {2,3} → width = 2
```

---

## 6. Approaches

### Approach 1: Level Order Traversal with Indexing 

**Idea:**
- Perform BFS (level-order traversal).
- Assign an index to every node:
  * Root → index 0
  * Left child → 2*i + 1
  * Right child → 2*i + 2
- For each level:
  * Store index of first and last node
  * Compute width using:
    * width = lastIndex - firstIndex + 1

**Steps:**
- Use a queue storing (node, index)
- For each level:
  * Capture index of first node
  * Capture index of last node
- Compute width for that level
- Track maximum width

**Java Code:**
```java
static int widthOfBinaryTree(TreeNode root) {
    if (root == null) return 0;

    Queue<Pair> q = new ArrayDeque<>();
    q.offer(new Pair(root, 0L));
    int maxWidth = 0;

    while (!q.isEmpty()) {
        int size = q.size();
        long first = q.peek().idx;
        long last = first;

        for (int i = 0; i < size; i++) {
            Pair p = q.poll();
            last = p.idx;

            if (p.node.left != null)
                q.offer(new Pair(p.node.left, 2 * p.idx + 1));
            if (p.node.right != null)
                q.offer(new Pair(p.node.right, 2 * p.idx + 2));
        }

        maxWidth = Math.max(maxWidth, (int)(last - first + 1));
    }
    return maxWidth;
}

static class Pair {
    TreeNode node;
    long idx;
    Pair(TreeNode node, long idx) {
        this.node = node;
        this.idx = idx;
    }
}
```

**💭 Intuition Behind the Approach:**
- Indexing simulates a complete binary tree.
- Gaps caused by missing nodes are naturally counted.
- Width becomes a simple index difference problem.

**Complexity (Time & Space):**
- Time: O(n) — each node visited once
- Space: O(n) — queue storage

### Approach 2: DFS + Index Tracking (Alternative)

**Idea:**
- Use recursion.
- Pass (node, level, index)
- Store the first index seen at each level.
- Compute width as traversal proceeds.

**Steps:**
- Use a map to store first index per level
- Traverse tree using DFS
- At each node:
  * If first time at level → store index
  * Compute width using current index
- Track maximum width

**Java Code:**
```java
static int maxWidth = 0;

static int widthOfBinaryTree(TreeNode root) {
    Map<Integer, Long> firstIndex = new HashMap<>();
    dfs(root, 0, 0L, firstIndex);
    return maxWidth;
}

static void dfs(TreeNode node, int level, long idx, Map<Integer, Long> map) {
    if (node == null) return;

    map.putIfAbsent(level, idx);
    long first = map.get(level);

    maxWidth = Math.max(maxWidth, (int)(idx - first + 1));

    dfs(node.left, level + 1, 2 * idx + 1, map);
    dfs(node.right, level + 1, 2 * idx + 2, map);
}
```

**💭 Intuition Behind the Approach:**
- First node at a level defines left boundary
- Index difference measures real width including gaps
- DFS avoids queue but uses recursion stack

**Complexity (Time & Space):**
- Time: O(n) — each node once
- Space: O(n) — recursion + map

---

## 7. Justification / Proof of Optimality

- Width depends on relative positions, not node count
- Index-based traversal correctly models complete binary tree
- BFS is simpler; DFS is an elegant alternative
---

## 8. Variants / Follow-Ups

- Width of N-ary tree
- Vertical order traversal
- Maximum nodes at any level
- Binary tree indexing problems
---

## 9. Tips & Observations

- Always use long for indices
- Normalize indices per level if overflow is a concern
- BFS is preferred in interviews
- Null gaps matter — don’t ignore them
---

## 10. Pitfalls

- Counting only number of nodes
- Ignoring null positions
- Using int index (overflow)
- Forgetting +1 in width formula
---

<!-- #endregion -->
