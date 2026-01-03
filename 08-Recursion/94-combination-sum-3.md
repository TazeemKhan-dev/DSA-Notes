<!-- #region 94-Combination Sum 3 -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q94: Combination Sum 3</h1>

## 1. Problem Understanding

- You are given two integers k and n.
- Your task is to find all unique combinations of k numbers (from 1–9) that add up to n.
- Rules:
  * Each number can be used at most once.
  * Only numbers 1–9 are allowed.
  * You must return all unique combinations that satisfy the sum condition.
---

## 2. Constraints

- 2 <= k <= 9
- 1 <= n <= 60
- Only digits 1–9 allowed once per combination.
---

## 3. Edge Cases

- If n < smallest possible sum of k numbers → return empty.
- If n > largest possible sum of k numbers → return empty.
- If no valid combination exists → return empty list.
- k = 1 → Only return [n] if 1 <= n <= 9.
---

## 4. Examples

```text
Input:
k = 3, n = 8

Output:

1 2 5
1 3 4


Explanation:
All 3-number combinations from 1–9 that sum to 8 are [1,2,5] and [1,3,4].
```

---

## 5. Approaches

### Approach 1: Recursive Backtracking (DFS)

**Idea:**
- We explore all numbers from 1 to 9 recursively and build combinations:
- Include a number → reduce n by that number and decrement k.
- Skip it → move to the next.
- Stop when:
  * n == 0 && k == 0 → valid combination.
  * n < 0 or k < 0 → invalid path.

**Steps:**
- Start from number 1.
- At each step, choose to:
  * Include current number in the path.
  * Recurse with next number.
- If sum becomes 0 and size = k → add to result.
- Backtrack by removing the last number.

**Java Code:**
```java
import java.util.*;

class Solution {
    public static List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        helper(1, k, n, new ArrayList<>(), res);
        return res;
    }

    static void helper(int start, int k, int n, List<Integer> temp, List<List<Integer>> res) {
        if (n == 0 && temp.size() == k) {
            res.add(new ArrayList<>(temp));
            return;
        }
        if (n < 0 || temp.size() > k) return;

        for (int i = start; i <= 9; i++) {
            temp.add(i);
            helper(i + 1, k, n - i, temp, res);
            temp.remove(temp.size() - 1); // backtrack
        }
    }
}
🌳 Recursion Tree Example

For k=3, n=7:

helper(1,3,7,[])
 ├── pick 1 → helper(2,3,6,[1])
 │     ├── pick 2 → helper(3,3,4,[1,2])
 │     │     ├── pick 3 → helper(4,3,1,[1,2,3])
 │     │     └── pick 4 → helper(5,3,0,[1,2,4]) ✅ [1,2,4]
 │     └── pick 3 → helper(4,3,3,[1,3]) ...
 └── pick 2 → helper(3,3,5,[2]) ...


➡️ Final valid combinations: [1,2,4], [1,3,3] (invalid due to repetition, skipped).
```

**Complexity (Time & Space):**
- Time Complexity: O(2⁹) → Each number from 1–9 has two choices (include/exclude).
- Space Complexity: O(k) → Recursion stack depth + temporary list.

### Approach 2: Recursive with Pruning (Optimized)

**Idea:**
- Same as Approach 1, but prune unnecessary branches early:
  * Stop recursion when n < 0 or temp.size() == k.
  * Also limit upper loop bound: i <= 9 - (k - temp.size()) + 1 (ensures enough numbers remain).

**Java Code:**
```java
import java.util.*;

class Solution {
    public static List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(1, k, n, new ArrayList<>(), ans);
        return ans;
    }

    static void backtrack(int start, int k, int n, List<Integer> curr, List<List<Integer>> ans) {
        if (n == 0 && curr.size() == k) {
            ans.add(new ArrayList<>(curr));
            return;
        }

        for (int i = start; i <= 9; i++) {
            if (n - i < 0 || curr.size() >= k) break; // pruning
            curr.add(i);
            backtrack(i + 1, k, n - i, curr, ans);
            curr.remove(curr.size() - 1);
        }
    }
}
🌳 Recursion Tree Visualization (for k=3, n=8)
(1) → (2) → (5) ✅ sum=8
(1) → (3) → (4) ✅ sum=8
(1) → (4) → (5) ❌ sum=10 (pruned)
(2) → (3) → (5) ❌ sum=10 (pruned)
```

**Complexity (Time & Space):**
- Time Complexity: O(C(9, k)) → Only valid combinations of 9 choose k are explored.
- Space Complexity: O(k) → Recursion + path storage.
- Overall: Most efficient for given constraints.

### Approach 3: Bitmask Enumeration (Iterative)

**Idea:**
- We can use bitmasking to represent subsets of {1..9}.
- Each subset corresponds to one possible combination of numbers.
- For example:
  * Bitmask 010011001 → represents numbers {2,6,7,9}.
- We iterate through all possible bitmasks (from 1 to (1<<9)-1), and for each mask:
  * Count how many bits are set → that’s the number of elements (should equal k).
  * Compute the sum of those selected numbers → should equal n.
  * If both match, record the combination.

**Steps:**
- Loop through all 512 (2⁹) possible subsets.
- For each subset:
  * Extract elements (1–9) based on bits set.
  * If count == k and sum == n → add to result.

**Java Code:**
```java
import java.util.*;

class Solution {
    public static List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ans = new ArrayList<>();

        // Iterate over all subsets of {1,2,3,4,5,6,7,8,9}
        for (int mask = 1; mask < (1 << 9); mask++) {
            List<Integer> curr = new ArrayList<>();
            int sum = 0;
            for (int i = 0; i < 9; i++) {
                if ((mask & (1 << i)) != 0) {
                    curr.add(i + 1);
                    sum += i + 1;
                }
            }
            if (curr.size() == k && sum == n) ans.add(curr);
        }

        return ans;
    }
}
Example Walkthrough (k=3, n=7)

Mask = 001011001 → represents {3,4,7}

sum = 14 → invalid

Mask = 000101101 → represents {1,3,5,7}

size = 4 → invalid

Mask = 000011011 → represents {1,2,4} ✅

size = 3, sum = 7 → valid combination
```

**Complexity (Time & Space):**
- Time Complexity: O(2⁹ × 9) → Iterate all 512 subsets × up to 9 operations each.
- Space Complexity: O(k) → Temporary subset storage.
- Overall: Constant-time feasible (small fixed set), clean iterative alternative.

---

## 6. Justification / Proof of Optimality

- All three approaches (Recursive DFS, Optimized DFS with Pruning, and Bitmask Enumeration) explore subsets of numbers from 1–9 to find combinations that sum up to n.
- The Recursive DFS approach is intuitive — it systematically explores all inclusion/exclusion paths — but may explore unnecessary branches.
- The Optimized DFS (with Pruning) improves performance by stopping recursion early when the current sum exceeds the target (n) or when too many numbers are picked (path.size() > k).
  * This drastically reduces the search space while maintaining correctness.
- The Bitmask Enumeration method, on the other hand, replaces recursion with an iterative and memory-efficient subset generation technique using bitwise operations.
  * It’s particularly clean and effective when the range of numbers is small (like 1–9), since there are only 2⁹ = 512 subsets to check.
---

## 7. Variants / Follow-Ups

- Combination Sum I – Can reuse same number (no upper bound of 9).
- Combination Sum II – Numbers may repeat in input, avoid duplicates.
- Combination Sum IV – Order matters (DP-based counting problem).
---

## 8. Tips & Observations

- For small ranges (like {1..9}), bitmasking can be just as efficient as recursion and often simpler to debug.
- Pruning is the biggest performance boost — it avoids exploring paths that can never meet the target sum.
- Always increment start in recursive calls to avoid duplicate combinations and maintain ascending order.
- Use backtracking (remove last element) after each recursive call to restore the state.
- The bitmask method is purely iterative — no recursion stack, which can be advantageous in constrained environments.
- When using recursion, always consider base cases carefully:
- If sum == target and size == k → valid combination.
- If sum > target or size > k → stop exploring.
- Keep combinations sorted lexicographically by iterating numbers in increasing order.
- Both recursive and bitmask methods are efficient enough here since the input space is constant (only 9 elements).
---

<!-- #endregion -->
