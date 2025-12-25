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
