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
