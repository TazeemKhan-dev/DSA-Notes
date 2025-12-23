<!-- #region 00-Binary Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q00: Binary Tree</h1>

## 1. Problem Statement

- What is a Tree?
- A Tree is a non-linear data structure
- Made of:
  * Nodes → store data
  * Edges → connect nodes
- No cycles
- Grows downward from the root
- Exactly one path between any two nodes

- **Node Terminology**
- Root
    * Starting node
    * Has no parent
  * Parent
    * Node having at least one child
  * Child
    * Node derived from a parent
  * Leaf (External Node)
    * Node with no children
  * Internal Node
    * Node with at least one child

- **Tree Properties**
- A tree with N nodes has exactly N − 1 edges
  * Removing any edge disconnects the tree
  * Adding any extra edge creates a cycle

- **N-ary Tree**
- Each node can have at most N children
  * Examples:
    * Binary Tree → N = 2
    * Ternary Tree → N = 3
---

## 2. Problem Understanding

- Binary Tree (CORE DEFINITION)
- A Binary Tree is a tree where:
  * Each node has at most 2 children
    * Left child
    * Right child
- ❌ No ordering rule
- Structure matters, not values

- **Depth vs Height (VERY IMPORTANT)**
- 🔹 Depth of a Node
    * Distance from root → that node
    * Measures how deep the node is
    * Root depth = 0
- 🔹 Height of a Node
    * Distance from that node → deepest leaf
    * Measures how tall the subtree is

- **🔹 Height of a Tree**
- Height of the root node
      * Longest path from root → deepest leaf
      * 📌 Height depends on structure, NOT number of nodes.

- **🌲 Types of Binary Trees**
- 1️⃣ Perfect Binary Tree
        * Every internal node has exactly 2 children
        * All leaf nodes at same level
        * Height = log2(N + 1)
    * 2️⃣ Full Binary Tree
        * Every node has either:
        * 0 children, or
        * 2 children
        * No node with only one child
    * 3️⃣ Complete Binary Tree
        * All levels completely filled except possibly last
        * Last level filled left to right
        * Used in Heap
    * 4️⃣ Skewed Binary Tree
        * All nodes in one direction
        * Left-skewed or Right-skewed
        * Worst-case structure
        * Height = N
    * 5️⃣ Balanced Binary Tree
        * For every node:
        * |height(left subtree) − height(right subtree)| ≤ 1
        * Keeps height small
        * Improves performance of operations

- **Height Analysis (CORRECT & SAFE)**
- Balanced / Complete / Perfect Binary Tree
      * Height ≈ log2(N)
    * Skewed Binary Tree
      * Height = N
    * General Binary Tree
      * Height depends on shape, not size
    * ❌ Never compute height using log2(size) unless tree type is guaranteed.

- **Subtree**
- Any node with all its descendants forms a subtree
  * Types:
  * Left Subtree
  * Right Subtree

- **Tree Traversals**
- Depth First Search (DFS)
    * Goes deep before wide
    * Uses:
    * Recursion (implicit stack) or
    * Explicit stack
    * Types:
    * Preorder
    * Inorder
    * Postorder
  * Breadth First Search (BFS)
    * Visits nodes level by level
    * Uses Queue
    * Also called Level Order Traversal

- **DFS Traversal Orders**
- Preorder
    * Root → Left → Right
    * Used when:
    * Creating copy of tree
    * Prefix expression
  * Inorder
    * Left → Root → Right
    * Used when:
    * Traversing tree structure
    * (Sorted output ONLY if BST — not here)
  * Postorder
    * Left → Right → Root
    * Used when:
    * Deleting tree
    * Postfix expression
  * Level Order Traversal
    * Nodes visited level by level
    * Uses Queue
    * Common for:
      * Height
      * Level problems
      * BFS questions

- **Common Binary Tree Pitfalls**
- Height ≠ log2(size)
    * Depth ≠ Height
    * Binary Tree ≠ BST
    * Traversal order does not imply sorting
    * Skewed tree causes worst-case performance

- **Interview & Exam Tips (Binary Tree)**
- Always clarify:
      * Height measured in nodes or edges
    * Recursive solutions → O(N) time
    * Space → recursion stack = O(height)
    * BFS → Queue
    * DFS → Stack / Recursion
---

<!-- #endregion -->
