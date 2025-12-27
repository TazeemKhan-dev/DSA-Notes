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
<!-- #region 241-Zig Zag Traversal of Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q241: Zig Zag Traversal of Tree</h1>

## 1. Problem Statement

- Given the root node of a binary tree, print its nodes in zig-zag (spiral) order traversal.
  * Level 1 → Left to Right
  * Level 2 → Right to Left
  * Level 3 → Left to Right
  * and so on…
- You must print the values in one line, space-separated.
---

## 2. Problem Understanding

- This is a level order traversal (BFS) problem.
- The traversal direction alternates at every level.
- Tree structure must NOT be modified.
- Only the order of output changes.
- Important clarification:
  * Zig-zag affects printing order, not child insertion order in BFS.
---

## 3. Constraints

- 1 <= n <= 10^5
- Node values < 2^32
- Efficient O(N) solution required
---

## 4. Edge Cases

- Empty tree → print nothing
- Single node → print the node
- Skewed tree → still alternate direction logically
---

## 5. Examples

```text
Input

1 2 3 4 5 6 7
Output

1 3 2 4 5 6 7
Explanation

Original tree was:
              1
           /    \
          2      3
         / \    / \
        4   5  6   7

After Zig Zag traversal, tree formed would be:

              1
           /    \
          3      2
         / \    / \
        4   5  6   7
```

---

## 6. Approaches

### Approach 1: BFS + Reverse Current Level

**Idea:**
- Do normal level-order traversal using a queue.
- Store each level in a list.
- Reverse the list for alternate levels.

**Steps:**
- Use a queue and push the root.
- Use a boolean leftToRight starting as true.
- For each level:
  * Collect all node values in a list.
  * Reverse list if leftToRight == false.
- Print the list.
- Toggle direction.

**Java Code:**
```java
public static void binaryTreeZigZagTraversal(Node root) {
    if (root == null) return;

    Queue<Node> q = new ArrayDeque<>();
    q.add(root);
    boolean leftToRight = true;

    while (!q.isEmpty()) {
        int size = q.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < size; i++) {
            Node curr = q.poll();
            level.add(curr.data);

            if (curr.left != null) q.add(curr.left);
            if (curr.right != null) q.add(curr.right);
        }

        if (!leftToRight) {
            Collections.reverse(level);
        }

        for (int val : level) {
            System.out.print(val + " ");
        }

        leftToRight = !leftToRight;
    }
}
```

**💭 Intuition Behind the Approach:**
- BFS guarantees correct level separation.
- Reversal only affects output order, not traversal.
- Total reversed elements across all levels = N, so no extra cost.

**Complexity (Time & Space):**
- Time: O(N) — each node processed once; total reverse cost sums to N
- Space: O(W) — queue + list for widest level

### Approach 2: BFS + Deque (No Reverse)

**Idea:**
- Use a Deque instead of a list.
- Insert values:
  * addLast() for left-to-right
  * addFirst() for right-to-left
- Avoid reversing entirely.

**Steps:**
- Push root into queue.
- Maintain a direction flag.
- For each level:
  * Use a deque to store values.
  * Insert at front or back based on direction.
- Print deque contents.
- Toggle direction.

**Java Code:**
```java
public static void binaryTreeZigZagTraversal(Node root) {
    if (root == null) return;

    Queue<Node> q = new ArrayDeque<>();
    q.add(root);
    boolean leftToRight = true;

    while (!q.isEmpty()) {
        int size = q.size();
        Deque<Integer> level = new ArrayDeque<>();

        for (int i = 0; i < size; i++) {
            Node curr = q.poll();

            if (leftToRight) {
                level.addLast(curr.data);
            } else {
                level.addFirst(curr.data);
            }

            if (curr.left != null) q.add(curr.left);
            if (curr.right != null) q.add(curr.right);
        }

        for (int val : level) {
            System.out.print(val + " ");
        }

        leftToRight = !leftToRight;
    }
}
```

**💭 Intuition Behind the Approach:**
- Queue controls tree traversal
- Deque controls zig-zag output
- Eliminates the need for reversing lists

**Complexity (Time & Space):**
- Time: O(N) — each node inserted once
- Space: O(W) — queue + deque for a level

---

## 7. Justification / Proof of Optimality

- Both approaches:
  * Preserve correct BFS structure
  * Handle large input sizes efficiently
  * Are safe for interviews and production code
- Deque approach is slightly cleaner, but reverse-based approach is perfectly acceptable.
---

## 8. Variants / Follow-Ups

- Return List<List<Integer>> instead of printing
- Spiral traversal using two stacks
- Zig-zag traversal for N-ary trees
---

## 9. Tips & Observations

- Never change child insertion order for zig-zag
- Zig-zag = output order change, not traversal change
- Collections.reverse() does not make it O(N²)
---

## 10. Pitfalls

- Resetting direction flag inside the loop
- Reversing the queue instead of the level list
- Modifying tree structure instead of output
---

<!-- #endregion -->
<!-- #region 242-Right View & Left View of Binary Tree, -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q242: Right View & Left View of Binary Tree,</h1>

## 1. Problem Statement

- Given the root of a binary tree, print:
  * Right View → nodes visible when the tree is viewed from the right side
  * Left View → nodes visible when the tree is viewed from the left side
- The output should be printed top to bottom.
---

## 2. Problem Understanding

- A view means:
  * Exactly one node per level
  * The node that appears first from that side
- For every level:
  * Right View → rightmost node
  * Left View → leftmost node
- Tree structure must not be modified.
---

## 3. Constraints

- 1 <= T <= 10
- 1 <= N <= 10000
- Efficient traversal required
---

## 4. Edge Cases

- Empty tree → print nothing
- Single node → same for both views
- Skewed tree → all nodes visible
---

## 5. Examples

```text
Input Tree:
        1
      /   \
     2     3
      \
       4

Right View: 1 3 4
Left View:  1 2 4
```

---

## 6. Approaches

### Approach 1: Brute Force (Height × Traversal)

**Idea:**
- Find height of tree
- For each level L, traverse whole tree and print:
  * First node from left (left view)
  * First node from right (right view)

**Java Code:**
```java
Right View — Java Code

static void rightView(Node root) {
    int h = height(root);
    for (int i = 1; i <= h; i++) {
        printRight(root, i);
    }
}

static boolean printRight(Node root, int level) {
    if (root == null) return false;
    if (level == 1) {
        System.out.print(root.data + " ");
        return true;
    }
    return printRight(root.right, level - 1) ||
           printRight(root.left, level - 1);
}


static void rightView(Node root) {
    int h = height(root);
    for (int i = 1; i <= h; i++) {
        printRight(root, i);
    }
}

static boolean printRight(Node root, int level) {
    if (root == null) return false;
    if (level == 1) {
        System.out.print(root.data + " ");
        return true;
    }
    return printRight(root.right, level - 1) ||
           printRight(root.left, level - 1);
}
```

**💭 Intuition Behind the Approach:**
- For each level, we explicitly search the visible node
- Short-circuit logic (||) ensures first visible node is printed

**Complexity (Time & Space):**
- Time: O(N * H) — full traversal for each level
- Space: O(H) — recursion stack

### Approach 2: DFS (Recursive, Level Tracking)

**Idea:**
- Do preorder traversal
- Track current level
- Print the first node encountered at each level
- Traversal order decides the view

**Java Code:**
```java
Right View — Java Code
static int maxLevel = -1;

static void rightView(Node root) {
    maxLevel = -1;
    dfsRight(root, 0);
}

static void dfsRight(Node root, int level) {
    if (root == null) return;

    if (level > maxLevel) {
        System.out.print(root.data + " ");
        maxLevel = level;
    }

    dfsRight(root.right, level + 1);
    dfsRight(root.left, level + 1);
}

Left View — Java Code
static int maxLevel = -1;

static void leftView(Node root) {
    maxLevel = -1;
    dfsLeft(root, 0);
}

static void dfsLeft(Node root, int level) {
    if (root == null) return;

    if (level > maxLevel) {
        System.out.print(root.data + " ");
        maxLevel = level;
    }

    dfsLeft(root.left, level + 1);
    dfsLeft(root.right, level + 1);
}
```

**💭 Intuition Behind the Approach:**
- First node visited at a level = visible node
- Changing child order only switches between views
- No extra data structure required

**Complexity (Time & Space):**
- Time: O(N) — each node visited once
- Space: O(H) — recursion stack

### Approach 3: BFS (Level Order Traversal) ✅ Optimal

**Idea:**
- Use queue for level-order traversal
- For each level:
  * Left View → first node
  * Right View → last node

**Java Code:**
```java
Right View — Java Code
static void rightView(Node root) {
    if (root == null) return;

    Queue<Node> q = new ArrayDeque<>();
    q.add(root);

    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            Node curr = q.poll();

            if (i == size - 1) {
                System.out.print(curr.data + " ");
            }

            if (curr.left != null) q.add(curr.left);
            if (curr.right != null) q.add(curr.right);
        }
    }
}

Left View — Java Code
static void leftView(Node root) {
    if (root == null) return;

    Queue<Node> q = new ArrayDeque<>();
    q.add(root);

    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            Node curr = q.poll();

            if (i == 0) {
                System.out.print(curr.data + " ");
            }

            if (curr.left != null) q.add(curr.left);
            if (curr.right != null) q.add(curr.right);
        }
    }
}
```

**💭 Intuition Behind the Approach:**
- BFS naturally separates levels
- Index position in level decides visibility
- Cleanest and safest approach

**Complexity (Time & Space):**
- Time: O(N) — each node processed once
- Space: O(W) — queue size equals max width

---

## 7. Justification / Proof of Optimality

- Brute force exists but inefficient
- DFS is elegant but recursion-heavy
- BFS is most reliable and interview-preferred
---

## 8. Variants / Follow-Ups

- Top View
- Bottom View
- Boundary Traversal
- Vertical Order Traversal
---

## 9. Tips & Observations

- Right vs Left view difference is only traversal order or index check
- Do NOT modify tree
- BFS avoids stack overflow
---

## 10. Pitfalls

- Forgetting to reset maxLevel in DFS
- Printing multiple nodes per level
- Confusing right view with zig-zag
---

<!-- #endregion -->
<!-- #region 243-Bottom View of Binary Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q243: Bottom View of Binary Tree</h1>

## 1. Problem Statement

- Given the root of a binary tree, print the bottom view of the tree from left to right.
- A node is included in the bottom view if it is visible when the tree is viewed from the bottom.
- Important rule
  * If multiple nodes exist at the same horizontal distance (HD) and same depth,
  * → print the later one in level order traversal.
---

## 2. Problem Understanding

- Each node lies on a vertical line identified by its horizontal distance (HD):
  * Root → HD = 0
  * Left child → HD - 1
  * Right child → HD + 1
- Bottom view requires:
  * Node with maximum depth at each HD
  * If depth ties → later node in BFS wins
- Output must be printed from minimum HD to maximum HD
---

## 3. Constraints

- 1 <= T <= 10
- 1 <= N <= 10000
- Tree built using level order traversal
- Efficient solution required
---

## 4. Edge Cases

- Empty tree → empty output
- Single node → that node
- Skewed tree → all nodes appear
- Multiple nodes at same HD & depth → later BFS node
---

## 5. Examples

```text
Input

1
20 8 22 5 3 4 25 N N 10 N N 14
Output

5 10 4 14 25.
Explanation

            20
          /    \
        8       22
      /   \     /   \
    5      3 4     25
          /    \      
        10       14
```

---

## 6. Approaches

### Approach 1: Brute Force (Vertical Lines + Height Check)

**Idea:**
- Compute min & max horizontal distance
- For each vertical line:
  * Traverse entire tree
  * Pick node with maximum depth

**Steps:**
- Find range of HD (minHD, maxHD)
- For each HD:
  * Traverse tree
  * Track deepest node for that HD
- Print results

**Java Code:**
```java
static class Pair {
    int val, depth;
    Pair(int v, int d) { val = v; depth = d; }
}

static void bottomView(Node root) {
    int[] range = new int[]{0, 0};
    findRange(root, 0, range);

    for (int hd = range[0]; hd <= range[1]; hd++) {
        Pair res = new Pair(-1, -1);
        findBottom(root, hd, 0, res);
        System.out.print(res.val + " ");
    }
}

static void findBottom(Node root, int hd, int depth, Pair res) {
    if (root == null) return;

    if (hd == 0 && depth >= res.depth) {
        res.val = root.data;
        res.depth = depth;
    }

    findBottom(root.left, hd - 1, depth + 1, res);
    findBottom(root.right, hd + 1, depth + 1, res);
}

static void findRange(Node root, int hd, int[] range) {
    if (root == null) return;
    range[0] = Math.min(range[0], hd);
    range[1] = Math.max(range[1], hd);
    findRange(root.left, hd - 1, range);
    findRange(root.right, hd + 1, range);
}
```

**💭 Intuition Behind the Approach:**
- We explicitly check every vertical line
- Depth comparison ensures bottom-most node
- Tie handled by >= depth check

**Complexity (Time & Space):**
- Time: O(N * W) — full traversal for each vertical line
- Space: O(H) — recursion stack

### Approach 2: DFS with HD + Depth Map

**Idea:**
- DFS traversal
- Store for each HD:
  * Node value
  * Depth
- Update map only if:
  * Depth is greater
  * OR same depth but visited later

**Java Code:**
```java
static class Pair {
    int val, depth;
    Pair(int v, int d) { val = v; depth = d; }
}

static void bottomView(Node root) {
    TreeMap<Integer, Pair> map = new TreeMap<>();
    dfs(root, 0, 0, map);

    for (Pair p : map.values()) {
        System.out.print(p.val + " ");
    }
}

static void dfs(Node root, int hd, int depth, TreeMap<Integer, Pair> map) {
    if (root == null) return;

    if (!map.containsKey(hd) || depth >= map.get(hd).depth) {
        map.put(hd, new Pair(root.data, depth));
    }

    dfs(root.left, hd - 1, depth + 1, map);
    dfs(root.right, hd + 1, depth + 1, map);
}
```

**💭 Intuition Behind the Approach:**
- DFS gives depth info naturally
- >= ensures later node overwrites on tie
- TreeMap keeps HD sorted

**Complexity (Time & Space):**
- Time: O(N log W) — TreeMap insert
- Space: O(N) — map + recursion stack
- ⚠️ Risk of stack overflow for deep trees

### Approach 3: BFS + Horizontal Distance Map ✅ Optimal

**Idea:**
- Level Order Traversal (BFS)
- Track HD for each node
- Overwrite map entry every time → later BFS node wins
- TreeMap ensures left-to-right output

**Java Code:**
```java
static class Pair {
    Node node;
    int hd;
    Pair(Node n, int h) { node = n; hd = h; }
}

static ArrayList<Integer> bottomView(Node root) {
    ArrayList<Integer> res = new ArrayList<>();
    if (root == null) return res;

    TreeMap<Integer, Integer> map = new TreeMap<>();
    Queue<Pair> q = new ArrayDeque<>();
    q.add(new Pair(root, 0));

    while (!q.isEmpty()) {
        Pair curr = q.poll();
        map.put(curr.hd, curr.node.data); // overwrite

        if (curr.node.left != null)
            q.add(new Pair(curr.node.left, curr.hd - 1));
        if (curr.node.right != null)
            q.add(new Pair(curr.node.right, curr.hd + 1));
    }

    for (int val : map.values()) {
        res.add(val);
    }
    return res;
}
```

**💭 Intuition Behind the Approach:**
- BFS guarantees level order
- Overwriting ensures:
- Deeper nodes win
- Same depth → later node wins (as required)
- TreeMap handles sorted output

**Complexity (Time & Space):**
- Time: O(N log W) — TreeMap operations
- Space: O(N) — queue + map

### Approach 4: Variant: Bottom View preferring LEFTMOST (Not Standard) 

**Idea:**
 * ⚠️ This is NOT the asked problem, but mentioned

**Steps:**
- Same HD & same depth
- Pick earlier node, not later

**Java Code:**
```java
static ArrayList<Integer> bottomViewLeftMost(Node root) {
    ArrayList<Integer> res = new ArrayList<>();
    if (root == null) return res;

    TreeMap<Integer, int[]> map = new TreeMap<>();
    Queue<Pair> q = new ArrayDeque<>();
    q.add(new Pair(root, 0));

    int level = 0;
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            Pair curr = q.poll();

            if (!map.containsKey(curr.hd)) {
                map.put(curr.hd, new int[]{curr.node.data, level});
            }

            if (curr.node.left != null)
                q.add(new Pair(curr.node.left, curr.hd - 1));
            if (curr.node.right != null)
                q.add(new Pair(curr.node.right, curr.hd + 1));
        }
        level++;
    }

    for (int[] v : map.values()) {
        res.add(v[0]);
    }
    return res;
}
```

---

## 7. Justification / Proof of Optimality

- Bottom View fundamentally depends on vertical distance + depth
- BFS is safest because:
- No recursion depth issues
- Naturally satisfies “later in level order” rule
- DFS requires careful depth handling
---

## 8. Variants / Follow-Ups

- Top View
- Vertical Order Traversal
- Left View / Right View
- Diagonal Traversal
---

## 9. Tips & Observations

- Bottom View ≠ Vertical Order
- Overwriting map entry is intentional
- BFS is almost always preferred for view problems
---

## 10. Pitfalls

- Using DFS without depth tracking
- Forgetting tie-breaking rule
- Confusing bottom view with rightmost node per level
---

<!-- #endregion -->
<!-- #region 244-Top View of Binary Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q244: Top View of Binary Tree</h1>

## 1. Problem Statement

- Given the root of a binary tree, print the top view of the tree from left to right.
- Top view of a binary tree is the set of nodes visible when the tree is viewed from the top.
- You need to complete the given function. Input and output handling are already done by the driver code.
---

## 2. Problem Understanding

- Each node lies on a vertical line, identified by a Horizontal Distance (HD):
  * Root → HD = 0
  * Left child → HD - 1
  * Right child → HD + 1
- For each vertical line:
  * The first node encountered from top is visible
- Output must be printed from minimum HD to maximum HD
- Key rule:
  * First node at a given HD wins (ignore all later ones)
---

## 3. Constraints

- 1 <= T <= 10
- 1 <= N <= 10000
- Tree built using level order traversal
- Efficient solution required
---

## 4. Edge Cases

- Empty tree → empty output
- Single node → that node
- Skewed tree → all nodes visible
- Multiple nodes at same HD → only the top-most node
---

## 5. Examples

```text
Input

1
1 2 3 4

      1
    /   \
   2     3
  /
4
Output

4 2 1 3
```

---

## 6. Approaches

### Approach 1: Brute Force (Vertical Line Scan)

**Idea:**
- Find range of horizontal distances
- For each HD:
  * Traverse entire tree
  * Pick node with minimum depth

**Steps:**
- Compute minHD and maxHD
- For each HD:
  * Traverse tree
  * Track smallest depth node
- Print results

**Java Code:**
```java
static class Pair {
    int val, depth;
    Pair(int v, int d) { val = v; depth = d; }
}

static void topView(Node root) {
    int[] range = new int[]{0, 0};
    findRange(root, 0, range);

    for (int hd = range[0]; hd <= range[1]; hd++) {
        Pair res = new Pair(-1, Integer.MAX_VALUE);
        findTop(root, hd, 0, res);
        System.out.print(res.val + " ");
    }
}

static void findTop(Node root, int hd, int depth, Pair res) {
    if (root == null) return;

    if (hd == 0 && depth < res.depth) {
        res.val = root.data;
        res.depth = depth;
    }

    findTop(root.left, hd - 1, depth + 1, res);
    findTop(root.right, hd + 1, depth + 1, res);
}

static void findRange(Node root, int hd, int[] range) {
    if (root == null) return;
    range[0] = Math.min(range[0], hd);
    range[1] = Math.max(range[1], hd);
    findRange(root.left, hd - 1, range);
    findRange(root.right, hd + 1, range);
}
```

**💭 Intuition Behind the Approach:**
- Explicitly check every vertical line
- Depth comparison ensures top-most node
- Very slow for large trees

**Complexity (Time & Space):**
- Time: O(N * W) — full traversal per vertical line
- Space: O(H) — recursion stack

### Approach 2: DFS (Recursive with HD + Depth)

**Idea:**
- DFS traversal
- Track:
  * Horizontal distance
  * Depth
- Store node only if:
  * HD not seen before
  * OR current depth is smaller

**Java Code:**
```java
static class Pair {
    int val, depth;
    Pair(int v, int d) { val = v; depth = d; }
}

static ArrayList<Integer> topView(Node root) {
    TreeMap<Integer, Pair> map = new TreeMap<>();
    dfs(root, 0, 0, map);

    ArrayList<Integer> res = new ArrayList<>();
    for (Pair p : map.values()) {
        res.add(p.val);
    }
    return res;
}

static void dfs(Node root, int hd, int depth, TreeMap<Integer, Pair> map) {
    if (root == null) return;

    if (!map.containsKey(hd) || depth < map.get(hd).depth) {
        map.put(hd, new Pair(root.data, depth));
    }

    dfs(root.left, hd - 1, depth + 1, map);
    dfs(root.right, hd + 1, depth + 1, map);
}
```

**💭 Intuition Behind the Approach:**
- DFS provides depth information
- Only top-most node stored per HD
- Needs careful depth comparison

**Complexity (Time & Space):**
- Time: O(N log W) — TreeMap operations
- Space: O(N) — recursion + map

### Approach 3: BFS (Level Order + HD Map) ✅ Optimal

**Idea:**
- Use BFS (level order traversal)
- Track HD for each node
- Store first node encountered at each HD
- Ignore all later nodes at same HD

**Java Code:**
```java
static class Pair {
    Node node;
    int hd;
    Pair(Node n, int h) {
        node = n;
        hd = h;
    }
}

static ArrayList<Integer> topView(Node root) {
    ArrayList<Integer> ans = new ArrayList<>();
    if (root == null) return ans;

    TreeMap<Integer, Integer> map = new TreeMap<>();
    Queue<Pair> q = new ArrayDeque<>();
    q.add(new Pair(root, 0));

    while (!q.isEmpty()) {
        Pair curr = q.poll();
        int hd = curr.hd;
        Node node = curr.node;

        if (!map.containsKey(hd)) {
            map.put(hd, node.data);
        }

        if (node.left != null)
            q.add(new Pair(node.left, hd - 1));
        if (node.right != null)
            q.add(new Pair(node.right, hd + 1));
    }

    for (int val : map.values()) {
        ans.add(val);
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- BFS guarantees top-to-bottom traversal
- First node at an HD is always top-most
- TreeMap ensures sorted output

**Complexity (Time & Space):**
- Time: O(N log W) — TreeMap insert
- Space: O(W) — queue + map

---

## 7. Justification / Proof of Optimality

- Brute force is inefficient
- DFS works but is riskier
- BFS is clean, safe, and intuitive for view problems
---

## 8. Variants / Follow-Ups

- Bottom View
- Left View
- Right View
- Vertical Order Traversal
---

## 9. Tips & Observations

- Top View = first node per HD
- Never overwrite map entry
- BFS is almost always the safest choice
---

## 10. Pitfalls

- Overwriting values in map (breaks top view)
- Confusing top view with vertical traversal
- Using DFS without depth comparison
---

<!-- #endregion -->
<!-- #region 245-Same Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q245: Same Tree</h1>

## 1. Problem Statement

- Given two root nodes of two separate binary trees a and b, check whether the trees are identical.
- Two binary trees are considered the same if:
  * They have identical structure (shape)
  * Corresponding nodes have the same value
- Print "YES" if the trees are identical, otherwise print "NO".
---

## 2. Problem Understanding

- We must compare both structure and values
- Just comparing values using traversal is not enough
- Structure matters:
  * Left child vs right child position
  * Presence or absence of nodes
- Key requirement:
- Every node in tree a must match the corresponding node in tree b.
---

## 3. Constraints

- 1 <= size(a), size(b) <= 100
- Node value < 2^32
- Multiple test cases possible
---

## 4. Edge Cases

- Both trees empty → identical
- One tree empty, other non-empty → not identical
- Same values but different structure → not identical
- Single-node trees → compare value
---

## 5. Examples

```text
Example 1
Input

1 2 3 
1 2 3
Output

YES
Explanation

1st tree
     1
    / \
   2   3

2nd tree
     1
    / \
   2   3
Example 2
Input

1 2 3
1 3 2
Output

NO
Explanation

1st tree
     1
    / \
   2   3

2nd tree
     1
    / \
   3   2
```

---

## 6. Approaches

### Approach 1: Brute Force (Traversal Without Nulls) ❌ Incorrect

**Idea:**
- Perform preorder / inorder traversal on both trees
- Compare resulting lists
- Why this fails
- Traversal does not capture structure.
- Example:
- Tree A:     Tree B:
   * 1           1
  * /             \
 * 2               2
- Preorder for both:
- 1 2
- But trees are not identical.
- 🚫 Incorrect approach

### Approach 2: Traversal WITH Null Markers (Better but Heavy)

**Idea:**
- Serialize both trees using preorder
- Add a marker (e.g. "N") for null nodes
- Compare the serialized sequences

**Java Code:**
```java
static void serialize(Node root, List<String> list) {
    if (root == null) {
        list.add("N");
        return;
    }
    list.add(String.valueOf(root.data));
    serialize(root.left, list);
    serialize(root.right, list);
}

static boolean isSameTree(Node a, Node b) {
    List<String> l1 = new ArrayList<>();
    List<String> l2 = new ArrayList<>();
    serialize(a, l1);
    serialize(b, l2);
    return l1.equals(l2);
}
```

**💭 Intuition Behind the Approach:**
- Null markers preserve structure
- Two trees serialize identically only if they are identical

**Complexity (Time & Space):**
- Time: O(N) — full traversal
- Space: O(N) — serialization storage
- ⚠️ Valid but not preferred

### Approach 3: Recursive Node-by-Node Comparison ✅ Optimal

**Idea:**
- Compare both trees simultaneously
- At every step:
  * Both nodes null → same
  * One null → not same
  * Values differ → not same
  * Recursively check left and right subtrees

**Java Code:**
```java
static boolean isSameTree(Node a, Node b) {
    if (a == null && b == null) return true;
    if (a == null || b == null) return false;
    if (a.data != b.data) return false;

    return isSameTree(a.left, b.left)
        && isSameTree(a.right, b.right);
}
```

**💭 Intuition Behind the Approach:**
- Structure and values are checked together
- No extra data structures
- Clean and direct logic

**Complexity (Time & Space):**
- Time: O(N) — each node compared once
- Space: O(H) — recursion stack (tree height)

### Approach 4: Alternative: Iterative BFS Comparison (Optional)

**Idea:**
- Compare both trees simultaneously
- At each node:
  * Both null → same
  * One null → not same
  * Values differ → not same
  * Recurse on left & right

**Java Code:**
```java
class Solution {

    public boolean isSameTree(Node a, Node b) {
        Queue<Node> q1 = new ArrayDeque<>();
        Queue<Node> q2 = new ArrayDeque<>();

        q1.add(a);
        q2.add(b);

        while (!q1.isEmpty() && !q2.isEmpty()) {
            Node n1 = q1.poll();
            Node n2 = q2.poll();

            if (n1 == null && n2 == null) continue;
            if (n1 == null || n2 == null) return false;
            if (n1.data != n2.data) return false;

            q1.add(n1.left);
            q1.add(n1.right);
            q2.add(n2.left);
            q2.add(n2.right);
        }
        return q1.isEmpty() && q2.isEmpty();
    }
}
```

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)
- ⚠️ Works, but recursive version is cleaner

---

## 7. Justification / Proof of Optimality

- Traversal without nulls ❌
- Serialization works but heavy
- Recursive comparison:
  * Clean
  * Efficient
  * Most expected by interviewers
---

## 8. Variants / Follow-Ups

- Subtree Check
  * Check whether tree B is a subtree of tree A
  * Uses Same Tree logic as a helper
  * Related problem: Subtree of Another Tree
- Symmetric Tree
  * Check whether a tree is a mirror of itself
  * Similar recursive structure comparison
  * Key change: compare left of one side with right of the other
- Check if Two Trees Are Mirror Images
  * Same as Same Tree, but:
    * Compare a.left with b.right
    * Compare a.right with b.left
- Check if Two Trees Have Same Structure (Ignore Values)
  * Only structure matters
  * Skip value comparison
  * Useful for structural validation problems
- Same Tree Using Iterative DFS (Stack-based)
  * Avoid recursion
  * Useful when recursion depth is a concern
  * Variant of the BFS approach you saw
- Same Tree with Serialization (String-based)
  * Serialize both trees with null markers
  * Compare serialized strings
  * Useful in systems / storage-based questions
---

## 9. Tips & Observations

- Same Tree = same shape + same values
- Traversal alone is misleading
- Always handle null first
---

## 10. Pitfalls

- Comparing only preorder/inorder
- Ignoring left/right positions
- Missing base null cases
---

<!-- #endregion -->
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
<!-- #region 247-Vertical Order Traversal of Binary Tree -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q247: Vertical Order Traversal of Binary Tree</h1>

## 1. Problem Statement

- You are given the root of a binary tree. Your task is to return the vertical order traversal of the tree.
- The vertical order traversal is defined as follows:
- Each node is assigned a position (row, col)
  * Root is at (0, 0)
  * Left child → (row + 1, col - 1)
  * Right child → (row + 1, col + 1)
- Nodes are grouped by their column index (col), from leftmost column to rightmost column.
- Within the same column:
  * Nodes are ordered from top to bottom (increasing row).
  * If multiple nodes share the same row and same column, they must be sorted by value.
- Return the result as a 2D list, where each inner list represents one vertical column.
---

## 2. Problem Understanding

- This is not a simple vertical grouping.
- Order matters in three levels:
  * Column (col) → left to right
  * Row (row) → top to bottom
  * Value (val) → ascending (only when row & col are same)
- We must track coordinates for each node.
- Output structure is:
  * [
    * column_1_nodes,
    * column_2_nodes,
    * ...
  * ]
- Key insight:
- 👉 This is a coordinate-based traversal problem, not just BFS or DFS alone.
---

## 3. Constraints

- 1 <= number of testcases <= 10
- 1 <= number of nodes <= 1000
- 1 <= node.val <= 1000
---

## 4. Edge Cases

- Single node tree
- Multiple nodes in same (row, col)
- Left-skewed or right-skewed trees
- Sparse trees
- Nodes having same column but different depths
---

## 5. Examples

```text
Input

1
1 2 3


Output

2
1
3


Explanation

  1
 / \
2   3


Vertical columns:

col -1 → {2}

col 0 → {1}

col +1 → {3}

Example 2

Input

1
3 9 20 N N 15 7


Output

9
3 15
20
7


Explanation

    3
   / \
  9  20
    /  \
   15   7


Columns:

col -1 → {9}

col 0 → {3, 15}

col +1 → {20}

col +2 → {7}
```

---

## 6. Approaches

### Approach 1: Brute Force using DFS + Sorting All Nodes

**Idea:**
- Traverse the tree.
- Store every node as (col, row, value) in a list.
- Sort the list based on:
  * col
  * row
  * value
- Group nodes by column.

**Steps:**
- DFS traversal storing (col, row, value)
- Sort list by (col, row, value)
- Group values by column

**Java Code:**
```java
static List<List<Integer>> verticalTraversal(TreeNode root) {
    List<int[]> nodes = new ArrayList<>();
    dfs(root, 0, 0, nodes);

    nodes.sort((a, b) -> {
        if (a[0] != b[0]) return a[0] - b[0];
        if (a[1] != b[1]) return a[1] - b[1];
        return a[2] - b[2];
    });

    List<List<Integer>> res = new ArrayList<>();
    int prevCol = Integer.MIN_VALUE;

    for (int[] n : nodes) {
        if (n[0] != prevCol) {
            res.add(new ArrayList<>());
            prevCol = n[0];
        }
        res.get(res.size() - 1).add(n[2]);
    }
    return res;
}

static void dfs(TreeNode node, int row, int col, List<int[]> nodes) {
    if (node == null) return;
    nodes.add(new int[]{col, row, node.val});
    dfs(node.left, row + 1, col - 1, nodes);
    dfs(node.right, row + 1, col + 1, nodes);
}
```

**💭 Intuition Behind the Approach:**
- Collect everything first, then impose the required ordering explicitly via sorting.

**Complexity (Time & Space):**
- Time: O(n log n) — sorting all nodes
- Space: O(n) — storing all nodes

### Approach 2: BFS + Nested Maps + PriorityQueue (Optimal)

**Idea:**
- Use BFS to naturally handle row-wise order.
- Use a nested structure:
  * col → row → min-heap(values)
- This ensures:
  * Columns sorted automatically
  * Rows sorted automatically
  * Same (row, col) values sorted by value

**Steps:**
- BFS with queue storing (node, row, col)
- Insert node value into:
  * map[col][row].add(value)
- Traverse columns from left to right
- For each column, traverse rows top to bottom
- Extract values from heap

**Java Code:**
```java
static List<List<Integer>> verticalTraversal(TreeNode root) {
    TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> map = new TreeMap<>();
    Queue<Tuple> q = new ArrayDeque<>();

    q.offer(new Tuple(root, 0, 0));

    while (!q.isEmpty()) {
        Tuple t = q.poll();
        map.putIfAbsent(t.col, new TreeMap<>());
        map.get(t.col).putIfAbsent(t.row, new PriorityQueue<>());
        map.get(t.col).get(t.row).offer(t.node.val);

        if (t.node.left != null)
            q.offer(new Tuple(t.node.left, t.row + 1, t.col - 1));
        if (t.node.right != null)
            q.offer(new Tuple(t.node.right, t.row + 1, t.col + 1));
    }

    List<List<Integer>> res = new ArrayList<>();

    for (TreeMap<Integer, PriorityQueue<Integer>> rows : map.values()) {
        List<Integer> colList = new ArrayList<>();
        for (PriorityQueue<Integer> pq : rows.values()) {
            while (!pq.isEmpty()) colList.add(pq.poll());
        }
        res.add(colList);
    }
    return res;
}

static class Tuple {
    TreeNode node;
    int row, col;
    Tuple(TreeNode node, int row, int col) {
        this.node = node;
        this.row = row;
        this.col = col;
    }
}


Vertical Order Traversal (Refactored with computeIfAbsent)
static List<List<Integer>> verticalTraversal(TreeNode root) {
    // col -> row -> min-heap of values
    TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> map = new TreeMap<>();
    Queue<Tuple> q = new ArrayDeque<>();

    q.offer(new Tuple(root, 0, 0));

    while (!q.isEmpty()) {
        Tuple t = q.poll();

        map
            .computeIfAbsent(t.col, k -> new TreeMap<>())
            .computeIfAbsent(t.row, k -> new PriorityQueue<>())
            .offer(t.node.val);

        if (t.node.left != null) {
            q.offer(new Tuple(t.node.left, t.row + 1, t.col - 1));
        }
        if (t.node.right != null) {
            q.offer(new Tuple(t.node.right, t.row + 1, t.col + 1));
        }
    }

    List<List<Integer>> res = new ArrayList<>();

    for (TreeMap<Integer, PriorityQueue<Integer>> rows : map.values()) {
        List<Integer> colList = new ArrayList<>();
        for (PriorityQueue<Integer> pq : rows.values()) {
            while (!pq.isEmpty()) {
                colList.add(pq.poll());
            }
        }
        res.add(colList);
    }

    return res;
}

static class Tuple {
    TreeNode node;
    int row, col;

    Tuple(TreeNode node, int row, int col) {
        this.node = node;
        this.row = row;
        this.col = col;
    }
}
Why this version is better

✅ No repeated get() calls

✅ No manual putIfAbsent clutter

✅ Atomic creation + retrieval

✅ Cleaner nested-map logic

✅ Preferred in interviews & production code

One-line interview explanation

“I used computeIfAbsent to lazily initialize nested maps and heaps, avoiding redundant lookups and keeping the code concise.”
```

**💭 Intuition Behind the Approach:**
- BFS preserves top-to-bottom order
- TreeMap maintains left-to-right columns
- PriorityQueue resolves same row & column conflicts

**Complexity (Time & Space):**
- Time: O(n log n) — due to TreeMap & heaps
- Space: O(n) — maps + queue

---

## 7. Justification / Proof of Optimality

- Problem requires strict ordering rules
- Single traversal is insufficient
- Coordinate + ordering-based solution is mandatory
---

## 8. Variants / Follow-Ups

- Bottom View of Binary Tree
- Top View of Binary Tree
- Diagonal Traversal
- Vertical Sum of Binary Tree
---

## 9. Tips & Observations

- Always track (row, col)
- TreeMap → sorted columns
- PriorityQueue → resolve ties
- BFS preferred for row ordering
---

## 10. Pitfalls

- Ignoring same (row, col) sorting rule
- Using HashMap instead of TreeMap
- Forgetting row ordering
- Using DFS without careful sorting
---

<!-- #endregion -->
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
