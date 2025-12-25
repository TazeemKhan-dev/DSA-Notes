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
