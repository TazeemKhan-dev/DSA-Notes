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
