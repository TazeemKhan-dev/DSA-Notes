<!-- #region 92-Subsets 2 -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q92: Subsets 2</h1>

## 1. Problem Understanding

- Given an array nums that may contain duplicates, print all unique subsets (non-empty), sorted in lexicographical order, and each subset itself in sorted order.
- Subsets are combinations where order doesn’t matter, but duplicates must be handled carefully.
---

## 2. Constraints

- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
- Duplicates may exist
- Subsets must be unique and ignore the empty set
---

## 3. Edge Cases

- Array has all duplicates → [1,1,1]
- Array has negative numbers → [-1,0,1]
- Only one element → [5]
---

## 4. Examples

```text
Input
3
2 1 2

Output
1
1 2
1 2 2
2
2 2

Explanation

All unique subsets of sorted [1, 2, 2] are:
[1], [1,2], [1,2,2], [2], [2,2]
```

---

## 5. Approaches

### Approach 1: Recursion with List<List<Integer>>

**Idea:**
- Use backtracking to explore two choices at every step:
  * Include current element
  * Exclude current element
  * But to avoid duplicates:
  * Sort the input array
  * Skip identical elements at the same recursion depth (if (i > idx && nums[i] == nums[i - 1]) continue)

**Steps:**
- Sort the array
- Create a recursive function helper(idx, current, ans)
- Add current subset to answer list
- For each element from idx → n:
- Skip duplicates
- Include the element and recurse
- Backtrack (remove element)
- Remove the empty subset before returning

**Java Code:**
```java
import java.util.*;

public class Subsets2 {
    public static List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums); // Step 1: sort
        List<List<Integer>> ans = new ArrayList<>();
        helper(nums, 0, new ArrayList<>(), ans);
        // Removes all empty subsets ([]) from the result list.
        // Equivalent to: ans.removeIf(list -> list.isEmpty());
        ans.removeIf(List::isEmpty); // remove empty subset
        return ans;
    }

    static void helper(int[] nums, int idx, List<Integer> current, List<List<Integer>> ans) {
        ans.add(new ArrayList<>(current));
        for (int i = idx; i < nums.length; i++) {
            if (i > idx && nums[i] == nums[i - 1]) continue; // skip duplicates
            current.add(nums[i]);
            helper(nums, i + 1, current, ans);
            current.remove(current.size() - 1); // backtrack
        }
    }

    public static void main(String[] args) {
        int[] nums = {2, 1, 2};
        List<List<Integer>> result = subsetsWithDup(nums);
        for (List<Integer> l : result) System.out.println(l);
    }
}


🌳 Recursion Tree (Example: nums = [1,2,2])
Start → []
|
|-- Include 1 → [1]
|     |
|     |-- Include 2 → [1,2]
|     |     |
|     |     |-- Include 2 → [1,2,2]
|     |     └-- Exclude → [1,2]
|     └-- Exclude → [1]
|
|-- Exclude 1 → []
      |
      |-- Include 2 → [2]
      |     |
      |     |-- Include 2 → [2,2]
      |     └-- Exclude → [2]


Collected subsets (excluding empty):
[1], [1,2], [1,2,2], [2], [2,2]
```

**Complexity (Time & Space):**
- Time Complexity:
  * Generating all subsets → O(2ⁿ × n)
    * There are 2^n subsets.
    * Copying each subset (up to size n) costs O(n).
  * ➡️ Overall: O(2ⁿ × n)
- Space Complexity:
  * Recursion stack → O(n) (depth of recursion tree).
  * Result list storage → O(2ⁿ × n) (to hold all subsets).
  * Temporary list (current) → O(n) at max depth.
  * ➡️ Overall: O(2ⁿ × n)

### Approach 2: Convert List<List<Integer>> → int[][] (for judge)

**Idea:**
- Judges often require 2D arrays instead of List<List<>>.
- After generating all subsets, convert the result manually.

**Java Code:**
```java
import java.io.*;
import java.util.*;

class Solution {
    static int[][] solve(int n, int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        helper(nums, 0, new ArrayList<>(), ans);

        ans.removeIf(List::isEmpty);

        int[][] result = new int[ans.size()][];
        for (int i = 0; i < ans.size(); i++) {
            List<Integer> subset = ans.get(i);
            result[i] = new int[subset.size()];
            for (int j = 0; j < subset.size(); j++) {
                result[i][j] = subset.get(j);
            }
        }
        return result;
    }

    static void helper(int[] nums, int idx, List<Integer> current, List<List<Integer>> ans) {
        ans.add(new ArrayList<>(current));
        for (int i = idx; i < nums.length; i++) {
            if (i > idx && nums[i] == nums[i - 1]) continue;
            current.add(nums[i]);
            helper(nums, i + 1, current, ans);
            current.remove(current.size() - 1);
        }
    }
}
```

**Complexity (Time & Space):**
- Time Complexity:
  * Subset generation → O(2ⁿ × n) (same as above).
  * Conversion (List<List<Integer>> → int[][]) → O(2ⁿ × n) (copying all elements).
  * ➡️ Overall: O(2ⁿ × n)
- Space Complexity:
  * Recursion stack → O(n)
  * Result (2D array) → O(2ⁿ × n)
  * Temporary subset array (during conversion) → O(1) (built directly into result).
  * ➡️ Overall: O(2ⁿ × n)

---

## 6. Justification / Proof of Optimality

- Sorting ensures subsets are lexicographically ordered.
- The skip condition prevents duplicate subsets.
- Manual conversion to int[][] gives flexibility for judge-based problems.
---

## 7. Variants / Follow-Ups

- Subsets I: no duplicates → simpler version.
- Combination Sum II: similar duplicate handling but with sum constraints.
- Permutations II: same duplicate handling logic but for permutation ordering.
---

## 8. Tips & Observations

- While converting List<List<Integer>> → int[][]
  * Never initialize with a fixed [n][n] matrix.
  * Use dynamic sizing based on subset list lengths.
    * int[][] result = new int[ans.size()][];
  * Each inner list can have different lengths, so create it dynamically:
    * result[i] = new int[l.size()];
- Use nested loops to fill values safely.
- Don’t forget to sort input array before recursion.
- Always remove the empty subset if question asks to ignore it.
---

<!-- #endregion -->
