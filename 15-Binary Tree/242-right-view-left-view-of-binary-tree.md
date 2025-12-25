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
