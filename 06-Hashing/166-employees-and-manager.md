<!-- #region 166-Employees and Manager -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q166: Employees and Manager</h1>

## 1. Problem Understanding

- You are given a mapping employee → manager.
- For every person, you must compute how many employees they manage, including:
  * Direct reports
  * Indirect reports (reports of their reports, recursively)
- CEO is the person whose manager is himself.
- Finally, print in lexical order of employee names:
- employee total_reports
---

## 2. Constraints

- 1 <= N <= 100
- Single manager per employee
- CEO reports to himself
- Characters used to represent employees/managers
- Indirect reports included
---

## 3. Edge Cases

- Single employee (self-reporting) → output employee 0
- Multiple disconnected chains (rare but possible in input)
- Deep hierarchy (chain-like)
- Employee with zero reports
- Output must be lexically sorted
---

## 4. Examples

```text
Input

6
A C
B C
C F
D E
E F
F F


Output

A 0
B 0
C 2
D 0
E 1
F 5
```

---

## 5. Approaches

### Approach 1: Brute Force (DFS for each employee)

**Idea:**
- For each employee X, run a DFS/BFS to count all nodes reachable under him.

**Steps:**
- Build adjacency list: manager → list of direct reports
- For each employee:
  * DFS starting from them
  * Count reachable nodes (excluding themselves)

**Java Code:**
```java
public Map<Character, Integer> countEmployees(Map<Character, Character> emp) {
    Map<Character, List<Character>> tree = new HashMap<>();

    // Build manager → direct reports list
    for (char e : emp.keySet()) {
        char m = emp.get(e);
        if (e != m) {  // ignore CEO mapping to himself
            tree.computeIfAbsent(m, k -> new ArrayList<>()).add(e);
        }
    }

    Map<Character, Integer> ans = new HashMap<>();

    for (char person : emp.keySet()) {
        ans.put(person, dfs(person, tree));
    }

    return ans;
}

private int dfs(char manager, Map<Character, List<Character>> tree) {
    int count = 0;
    if (!tree.containsKey(manager)) return 0;

    for (char child : tree.get(manager)) {
        count += 1 + dfs(child, tree);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- You repeatedly compute subtrees independently.
- Simple but inefficient since same subtrees are computed multiple times.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * DFS for each employee → N * (N) = O(N^2)
  * Space for recursion tree → O(N)
- 💾 Why this complexity?
  * Each DFS can touch all nodes in worst-case chain-like hierarchy.

### Approach 2: Single DFS with Memoization

**Idea:**
- Use a tree + memoization so each subtree-count is computed once.

**Steps:**
- Build adjacency list
- Identify CEO (employee who reports to themselves)
- DFS from CEO, computing subtree sizes
- For employees with no subtree entry, their count = 0
- Print sorted

**Java Code:**
```java
public Map<Character, Integer> countEmployees(Map<Character, Character> emp) {
    Map<Character, List<Character>> tree = new HashMap<>();
    char ceo = '#';

    // build graph
    for (char e : emp.keySet()) {
        char m = emp.get(e);
        if (e == m) ceo = e; // CEO
        else tree.computeIfAbsent(m, k -> new ArrayList<>()).add(e);
    }

    Map<Character, Integer> memo = new HashMap<>();
    dfs(ceo, tree, memo);

    // for leaf nodes not managers
    for (char e : emp.keySet()) {
        memo.putIfAbsent(e, 0);
    }

    return memo;
}

private int dfs(char manager, Map<Character, List<Character>> tree,
                Map<Character, Integer> memo) {

    if (memo.containsKey(manager)) return memo.get(manager);
    int total = 0;

    if (tree.containsKey(manager)) {
        for (char c : tree.get(manager)) {
            total += 1 + dfs(c, tree, memo);
        }
    }

    memo.put(manager, total);
    return total;
}
```

**💭 Intuition Behind the Approach:**
- Each subtree is solved once and cached.
- This converts repeated work into constant-time lookups → optimal.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Building tree → O(N)
  * DFS traversal → O(N)
  * Overall → O(N)
- 💾 Why this complexity?
    * Each employee and each edge is processed a single time.

---

## 6. Justification / Proof of Optimality

- The memoized DFS approach is optimal because each report-chain is computed only once.
- Hierarchy is a directed tree → one DFS solves all subtree sizes efficiently.
---

## 7. Variants / Follow-Ups

- Print only manager nodes (those who have at least 1 report)
- Multi-level hierarchy with depth tracking
- Count only direct reports
- Print tree structure visually
---

## 8. Tips & Observations

- Always use adjacency list for hierarchy problems
- Memoization drastically improves performance
- CEO detection = employee whose manager = themselves
- Lexically sorting ensures deterministic output
---

<!-- #endregion -->
