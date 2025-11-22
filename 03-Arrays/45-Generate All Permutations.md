<!-- #region 45-Generate All Permutations -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q45: Generate All Permutations</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** A permutation of a set of elements is an arrangement of those elements in a particular order

---

## 2. Constraints

- 1 <= nums.length <= 10 (to keep factorial manageable).
- Elements may be distinct or duplicate (if duplicates → handle uniqueness).


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[1, 2, 3]
```
Output:
```text
[1,2,3]  
[1,3,2]  
[2,1,3]  
[2,3,1]  
[3,1,2]  
[3,2,1]
```


---

## 4. Approaches

### Approach 1: Brute Force (Backtracking)

- **Idea:**
  - Try all possibilities recursively by placing each unused element.

**Java Code:**
```java
import java.util.*;

class GeneratePermutations {
    public static List<List<Integer>> permuteBacktracking(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        boolean[] used = new boolean[nums.length];
        backtrack(nums, new ArrayList<>(), used, res);
        return res;
    }

    private static void backtrack(int[] nums, List<Integer> curr, boolean[] used, List<List<Integer>> res) {
        if (curr.size() == nums.length) {
            res.add(new ArrayList<>(curr));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (!used[i]) {
                used[i] = true;
                curr.add(nums[i]);
                backtrack(nums, curr, used, res);
                curr.remove(curr.size() - 1);
                used[i] = false;
            }
        }
    }
}
```

**Complexity:**
- Time: O(n! * n)
- Space: O(n! * n)

### Approach 2: Optimal (Lexicographic Ordering via Next Permutation)

- **Idea:**
  - Start with sorted array. Use next permutation repeatedly until no more.

**Java Code:**
```java
class GeneratePermutationsLexico {
    public static List<List<Integer>> permuteLexico(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums); 
        res.add(toList(nums));
        while (nextPermutation(nums)) {
            res.add(toList(nums));
        }
        return res;
    }

    private static boolean nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while (i >= 0 && nums[i] >= nums[i+1]) i--;
        if (i < 0) return false;
        int j = nums.length - 1;
        while (nums[j] <= nums[i]) j--;
        swap(nums, i, j);
        reverse(nums, i+1, nums.length-1);
        return true;
    }

    private static void swap(int[] nums, int i, int j) {
        int tmp = nums[i]; nums[i] = nums[j]; nums[j] = tmp;
    }

    private static void reverse(int[] nums, int l, int r) {
        while (l < r) swap(nums, l++, r--);
    }

    private static List<Integer> toList(int[] nums) {
        List<Integer> list = new ArrayList<>();
        for (int num : nums) list.add(num);
        return list;
    }
}
```

**Complexity:**
- Time: O(n! * n) (same as brute, but structured)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Backtracking is more intuitive and commonly used.
- Lexicographic method is cleaner if permutations must be in sorted order.
- Both are optimal in terms of complexity (O(n! * n) is unavoidable).

---

## 6. Variants / Follow-Ups

- Permutations with duplicates (need to skip duplicates).
- String permutations.
- K-th permutation (directly find without generating all).

<!-- #endregion -->

