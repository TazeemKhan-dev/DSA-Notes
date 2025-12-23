<!-- #region 0-Tree & Binary Search Tree (BST) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Tree & Binary Search Tree (BST)</h1>

## 1. Problem Understanding

- 🧠 Tree — Basic Understanding
- What is a Tree?
- A Tree is a non-linear data structure
- Made of:
  * Nodes
  * Edges
- Node
  * Contains:
    * Data
    * Links (references) to other nodes
- Edge
  * A connection between two nodes

- **Root, Parent, Child, Leaf**
- Root Node
  * Starting point of the tree
  * Does not have a parent
- Parent
  * Node having at least one child
- Child
  * Node derived from a parent
- Leaf Node
  * Node with no   children

- **Tree Properties**
- A tree with N nodes has exactly N − 1 edges
- Tree grows downward from the root
- No cycles allowed

- **N-Array (N-ary) Tree**
- A tree in which each node can have up to N children
- Example:
  * Binary Tree → N = 2
  * Ternary Tree → N = 3

- **Binary Tree**
- A tree in which each node can have at most 2 children
  * Left child
  * Right child

- **Height & Distance**
- Height of a Node
  * Defined as the distance from the root to that node
  * Measured in number of edges
- Height of a Tree
  * Distance from root to the deepest leaf node
  * Measured in edges

- **Types of Binary Trees**
- 1️⃣ Perfect Binary Tree
  * Every node has exactly 2 children
  * All leaf nodes are at the same level
- 2️⃣ Full Binary Tree
  * Every node has either:
  * 0 children or
  * 2 children
- 3️⃣ Complete Binary Tree
  * All levels are completely filled except possibly the last
  * Last level nodes are filled from left to right
  * Used in Heap
  * ⚠️ Note:
  * Nodes do not need exactly 2 children on last level
- 4️⃣ Skewed Binary Tree
  * All nodes are in one direction
    * Left skewed
    * Right skewed
    * Worst case height
- 5️⃣ Balanced Binary Tree
  * For every node:
      * | height(left subtree) − height(right subtree) | ≤ 1

- **Height Analysis**
- Average height of binary tree with N nodes → log2(N)
- Worst case (skewed) → N

- **Subtree**
- A subtree is a part of a tree
- Types:
  * Left Subtree → Tree formed by left child
  * Right Subtree → Tree formed by right child

- **Tree Traversals**
- Depth First Search (DFS)
  * Uses recursion
  * No explicit data structure
  * Types:
    * Preorder
    * Inorder
    * Postorder
- Breadth First Search (BFS)
  * Iterative
  * Uses Queue
  * Also called Level Order Traversal

- **DFS Traversal Orders**
- 1️⃣ Preorder (Root → Left → Right)
  * Root
  * Left Subtree
  * Right Subtree
- 2️⃣ Inorder (Left → Root → Right)
  * Left Subtree
  * Root
  * Right Subtree
- 3️⃣ Postorder (Left → Right → Root)
  * Left Subtree
  * Right Subtree
  * Root
- 🌐 Level Order Traversal
  * Visits nodes level by level
  * Uses Queue
- Example:
  * Level 0 → Root
  * Level 1 → Children
  * Level 2 → Grandchildren

- **Binary Search Tree (BST)**
- A Binary Search Tree is a Binary Tree where:
- Left Subtree  → values < root
- Right Subtree → values > root
- This property holds for every node
- 💡 Intuition Behind BST
  * Inorder traversal of BST gives sorted order
  * Faster searching compared to normal binary tree
  * Structure depends on insertion order

```text
class Node {
    int data;
    Node left;
    Node right;

    Node(int data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

```

- **BST Core Operations (Concept)**
- Insert
  * Compare value with root
  * Go left or right recursively
  * Insert at correct position
- Search
  * If value < root → go left
  * If value > root → go right
  * If equal → found
- Delete
  * Case 1: Leaf node
  * Case 2: One child
  * Case 3: Two children
  * → Replace with inorder successor

- **Time Complexity (BST)**
- Average Case (Balanced)
  * Search → O(log N)
  * Insert → O(log N)
  * Delete → O(log N)
- Worst Case (Skewed)
  * All operations → O(N)
- 💾 Space Complexity
  * Recursive calls → O(height)
  * Balanced → O(log N)
  * Skewed → O(N)

- **Important Pitfalls**
- BST can become skewed
- Not self-balancing by default
- Inorder traversal only sorted if BST property holds
- Confusing Binary Tree with BST

- **Interview Tips**
- BST inorder → sorted output
- Balanced BST improves performance
- AVL / Red-Black Tree solve skewing issue
- BFS → Queue
- DFS → Recursion
---

<!-- #endregion -->
