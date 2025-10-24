<!-- #region 72-Print Stair Paths -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q72: Print Stair Paths</h1>

## 1. Problem Understanding

- You are given a number n representing the number of stairs.
- You start from the top (or equivalently, at n) and want to reach the ground (0).
- You can jump 1, 2, or 3 steps at a time.
- You must print all possible paths (as strings) showing which jumps were taken.
- Each path represents one valid sequence of jumps that reaches exactly 0.
---

## 2. Constraints

- 0 ≤ n ≤ 10
- Output can grow exponentially, but within limits for small n.
---

## 3. Edge Cases

- If n == 0: You’re already on the ground → print "" (empty path).
- If n < 0: Invalid jump → don’t print anything.
---

## 4. Examples

```text
Input
3

Output
111  
12  
21  
3

Explanation:
Paths →

1 + 1 + 1

1 + 2

2 + 1

3
```

---

## 5. Approaches

### Approach 1: Recursive Backtracking (Printing Paths Directly)

**Idea:**
- At each stair n, you can jump 1, 2, or 3 steps.
- → Recursively explore all paths by appending the jump (1, 2, or 3) to the current path string.

**Steps:**
- If n == 0 → print the path (you’ve reached the ground).
- If n < 0 → invalid move, return.
- Make recursive calls for:
  * printStairPaths(n-1, path + "1")
  * printStairPaths(n-2, path + "2")
  * printStairPaths(n-3, path + "3")

**Java Code:**
```java
public static void printStairPaths(int n, String path) {
        if (n == 0) {
            System.out.println(path);
            return;
        }
        if (n < 0) return;

        printStairPaths(n - 1, path + "1");
        printStairPaths(n - 2, path + "2");
        printStairPaths(n - 3, path + "3");
    }
```

**Complexity (Time & Space):**
- 🧮 Time Complexity:
- Each call makes up to 3 recursive calls → roughly O(3ⁿ)
- Because each stair can branch into 3 possible paths.
- 💾 Space Complexity:
- O(n) for recursion stack (maximum depth = n).

---

## 6. Justification / Proof of Optimality

- Recursion explores all combinations of jumps.
- Base case ensures only valid paths reaching exactly 0 are printed.
---

## 7. Variants / Follow-Ups

- Count total paths instead of printing.
- Store all paths in a list and return.
- Change jump range (e.g., 1 or 2 only).
---

## 8. Tips & Observations

- Pattern: whenever we explore all possible moves from a given state, recursion with multiple branches (like this) works best.
- This is similar to “staircase problem” or “dice roll to target” problems.
- If asked to count paths instead of printing, replace print with return count.
---

<!-- #endregion -->
