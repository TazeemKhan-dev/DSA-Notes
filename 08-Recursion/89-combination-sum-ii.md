<!-- #region 89-Combination Sum II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q89: Combination Sum II</h1>

## 1. Problem Understanding

- We are given an array C (possibly containing duplicates) and a target sum T.
- We need to find all unique combinations of numbers in C that sum to T.
- Each number in the array can be used at most once, and we must avoid duplicate combinations.
---

## 2. Constraints

- 1 ≤ n ≤ 100
- 1 ≤ C[i] ≤ 50
- 1 ≤ target ≤ 30
- Array may contain duplicates
- Elements in each combination must be sorted in non-decreasing order
---

## 3. Edge Cases

- No combination sums to target → print nothing.
- All elements > target → no result.
- Array contains duplicates → skip repeated combinations.
---

## 4. Examples

```text
Input:

7
10 1 2 7 6 1 5
8


Output:

1 1 6
1 2 5
1 7
2 6
```

---

## 5. Approaches

### Approach 1: Backtracking with Duplicate Handling (Optimized DFS) ✅

**Idea:**
- We use recursion to generate combinations but:
  * Sort the array first to handle duplicates easily.
  * Skip duplicate elements at the same recursion level.
  * Allow each element to be used only once (move to next index after using one).

**Steps:**
- Sort the input array.
- Use a recursive function dfs(index, target, currentList).
- If target == 0 → add the current list to result.
- For each element i from index to end:
  * If i > index and C[i] == C[i-1] → skip duplicate.
  * If C[i] > target → break (no need to continue).
  * Include C[i] and recurse for i+1 (since reuse not allowed).

**Java Code:**
```java
import java.util.*;

public class Solution {
    public static List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates); // sort to handle duplicates
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(candidates, target, 0, new ArrayList<>(), ans);
        return ans;
    }

    private static void backtrack(int[] arr, int target, int start, List<Integer> curr, List<List<Integer>> ans) {
        if (target == 0) {
            ans.add(new ArrayList<>(curr));
            return;
        }

        for (int i = start; i < arr.length; i++) {
            if (i > start && arr[i] == arr[i - 1]) continue; // skip duplicates
            if (arr[i] > target) break;

            curr.add(arr[i]);
            backtrack(arr, target - arr[i], i + 1, curr, ans); // move to next index
            curr.remove(curr.size() - 1);
        }
    }
}

🌳 Recursion Tree Example

Example Input:
C = [1, 1, 2, 5, 6, 7, 10], target = 8

dfs([], 8, 0)
│
├── include 1 → [1], target = 7
│   ├── include 1 → [1,1], target = 6
│   │   ├── include 2 → [1,1,2], target = 4
│   │   │   ├── include 5 → [1,1,2,5], target = -1 ❌
│   │   │   └── ...
│   │   ├── include 5 → [1,1,5], target = 0 ✅
│   │   └── include 6 → [1,1,6], target = 0 ✅
│   └── include 2 → [1,2], target = 5
│       ├── include 5 → [1,2,5], target = 0 ✅
│       └── include 6 → [1,2,6], target = -1 ❌
│
├── include 2 → [2], target = 6
│   ├── include 5 → [2,5], target = 1 ❌
│   ├── include 6 → [2,6], target = 0 ✅
│   └── ...
│
├── include 5 → [5], target = 3 ❌
└── include 6 → [6], target = 2 ❌


✅ Valid combinations:

[1,1,6]
[1,2,5]
[1,7]
[2,6]
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(2ⁿ) in the worst case (every element is either taken or not).
  * Pruning and duplicate skipping reduce actual time.
- 💾 Space Complexity
  * O(n) recursion stack depth (since each number used once per path).

### Approach 2: Iterative (Using Set to Avoid Duplicates)

**Idea:**
- Use a queue or list to simulate recursion iteratively, keeping a Set<List<Integer>> to prevent duplicates.
- Drawback:
  * More memory heavy.
  * Less intuitive than recursion.

**Java Code:**
```java
import java.util.*;

public class SolutionIterative {
    public static List<List<Integer>> combinationSum2(int[] arr, int target) {
        Arrays.sort(arr);
        Set<List<Integer>> set = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>(); // state: index, current sum, mask bit
        queue.offer(new int[]{0, 0, 0});

        while (!queue.isEmpty()) {
            int[] state = queue.poll();
            int index = state[0], sum = state[1];
            int mask = state[2];

            if (sum == target) {
                List<Integer> comb = new ArrayList<>();
                for (int i = 0; i < arr.length; i++)
                    if ((mask & (1 << i)) != 0) comb.add(arr[i]);
                set.add(comb);
                continue;
            }

            if (sum > target || index == arr.length) continue;

            // include current
            queue.offer(new int[]{index + 1, sum + arr[index], mask | (1 << index)});
            // exclude current
            queue.offer(new int[]{index + 1, sum, mask});
        }

        return new ArrayList<>(set);
    }
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(2ⁿ) states
  * Extra cost for duplicate checks in HashSet
- 💾 Space Complexity
  * O(2ⁿ) due to state storage

---

## 6. Justification / Proof of Optimality

- Backtracking (DFS) is most efficient and readable.
- Iterative approach works but is not preferred due to overhead.
---

## 7. Variants / Follow-Ups

- Combination Sum I → Numbers can be reused multiple times.
- Combination Sum III → Choose exactly k numbers from 1–9.
- Subset Sum II → Only check if target is achievable (True/False).
---

## 8. Tips & Observations

- Sorting is mandatory to easily skip duplicates.
- Always skip same element at same recursion depth using:
  * if (i > start && arr[i] == arr[i-1]) continue;
- For printing in sorted order, sort both the array and the result list.
---

<!-- #endregion -->
