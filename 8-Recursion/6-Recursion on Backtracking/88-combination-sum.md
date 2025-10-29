<!-- #region 88- Combination Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q88: Combination Sum</h1>

## 1. Problem Understanding

- We are given a set of distinct positive integers nums and a target integer target.
- We need to find all unique combinations of numbers from nums that sum to target.
  * ➡️ You can use each number unlimited times.
  * ➡️ The order of numbers in a combination doesn’t matter.
  * ➡️ The output can be in any order.
---

## 2. Constraints

- 1 ≤ nums.length ≤ 30
- 2 ≤ nums[i] ≤ 40
- 1 ≤ target ≤ 40
- All nums[i] are distinct
---

## 3. Edge Cases

- If nums is empty → return empty list
- If no combination sums to target → return empty list
- If target < min(nums) → no possible combinations
---

## 4. Examples

```text
Input:

nums = [6, 2, 7, 5], target = 16


Output:

[2,2,2,2,2,2,2,2]
[2,2,2,2,2,6]
[2,2,2,5,5]
[2,2,5,7]
[2,2,6,6]
[2,7,7]
[5,5,6]
```

---

## 5. Approaches

### Approach 1: Recursive Backtracking (DFS)

**Idea:**
- Use recursion to try each number multiple times until the running sum exceeds the target.
- If the sum equals target, record the current path as a valid combination.

**Steps:**
- Sort nums (optional, helps with pruning).
- Start recursion from index 0 with an empty combination.
- At each step:
  * If sum == target → store result.
  * If sum > target → return.
  * Else → explore:
    * include current element again (i same)
    * exclude and move to next (i+1)

**Java Code:**
```java
import java.util.*;

public class Solution {
    public static List<List<Integer>> combinationSum(int[] nums, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums); // Optional: helps with pruning
        backtrack(nums, target, 0, new ArrayList<>(), ans);
        return ans;
    }

    private static void backtrack(int[] nums, int target, int idx, List<Integer> curr, List<List<Integer>> ans) {
        if (target == 0) {
            ans.add(new ArrayList<>(curr));
            return;
        }
        if (target < 0 || idx == nums.length) return;

        // Include current element
        curr.add(nums[idx]);
        backtrack(nums, target - nums[idx], idx, curr, ans);
        curr.remove(curr.size() - 1);

        // Exclude current element
        backtrack(nums, target, idx + 1, curr, ans);
    }
}


Recursion Tree Example

Example Input: nums = [2, 3, 5], target = 7

backtrack([], target=7, idx=0)
│
├── include 2 → [2], target=5
│    ├── include 2 → [2,2], target=3
│    │    ├── include 2 → [2,2,2], target=1 ❌
│    │    └── exclude 2 → idx=1
│    │         ├── include 3 → [2,2,3], target=0 ✅
│    │         └── exclude 3 → idx=2
│    │              ├── include 5 → [2,2,5], target=-2 ❌
│    │              └── exclude 5 ❌
│    └── exclude 2 → idx=1
│         ├── include 3 → [2,3], target=2
│         │    ├── include 3 → [2,3,3], target=-1 ❌
│         │    └── exclude 3 → idx=2
│         │         ├── include 5 → [2,3,5], target=-3 ❌
│         │         └── exclude 5 ❌
│         └── exclude 3 → idx=2
│              ├── include 5 → [2,5], target=0 ✅
│              └── exclude 5 ❌
│
└── exclude 2 → idx=1
     ├── include 3 → [3], target=4
     │    ├── include 3 → [3,3], target=1 ❌
     │    └── exclude 3 → idx=2
     │         ├── include 5 → [3,5], target=-1 ❌
     │         └── exclude 5 ❌
     └── exclude 3 → idx=2
          ├── include 5 → [5], target=2 ❌
          └── exclude 5 ❌


✅ Valid combinations:

[2,2,3]
[2,5]
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Worst case: O(2^(target/min(nums))) — each element can be included/excluded many times
  * On average: Exponential (backtracking)
- 💾 Space Complexity
  * O(target/min(nums)) recursion depth (stack + combination list storage)

### Approach 2: Iterative (Using DP - Tabulation)

**Idea:**
- Use dynamic programming to build up combinations for all targets ≤ target.

**Steps:**
- Create a dp list where dp[i] stores all combinations summing to i.
- For each num in nums, update combinations from num → target.
- For each i, append num to every combination in dp[i - num].

**Java Code:**
```java
import java.util.*;

public class SolutionDP {
    public static List<List<Integer>> combinationSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<List<Integer>>> dp = new ArrayList<>(target + 1);
        for (int i = 0; i <= target; i++) dp.add(new ArrayList<>());

        dp.get(0).add(new ArrayList<>()); // base case

        for (int num : nums) {
            for (int t = num; t <= target; t++) {
                for (List<Integer> prev : dp.get(t - num)) {
                    List<Integer> newComb = new ArrayList<>(prev);
                    newComb.add(num);
                    dp.get(t).add(newComb);
                }
            }
        }
        return dp.get(target);
    }
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(target × n × avg_combinations) (depends on combination count)
  * Usually faster than recursion for small targets
- 💾 Space Complexity
  * O(target × avg_combinations)

---

## 6. Justification / Proof of Optimality

- Backtracking approach explores all possible sums systematically.
- DP approach can optimize recomputation but may use more memory.
---

## 7. Variants / Follow-Ups

- Combination Sum II → Each number used once only.
- Combination Sum III → Only use numbers 1–9, select k numbers.
---

## 8. Tips & Observations

- Sort nums → helps prune unnecessary recursive calls.
- Backtrack efficiently → stop recursion as soon as target < 0.
- Store results in a set (if duplicates possible).
---

<!-- #endregion -->
