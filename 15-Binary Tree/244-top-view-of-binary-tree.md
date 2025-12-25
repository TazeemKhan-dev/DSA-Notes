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
