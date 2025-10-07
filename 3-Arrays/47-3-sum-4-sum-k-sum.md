<!-- #region 47-3 Sum, 4 Sum, K Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q47: 3 Sum, 4 Sum, K Sum</h1>

## 1. Problem Understanding

- These problems are part of the K-Sum family, where we find unique combinations of numbers in an array that sum to a target.
- 3 Sum: Find all unique triplets [nums[i], nums[j], nums[k]] such that their sum is 0.
- 4 Sum: Find all unique quadruplets [a, b, c, d] such that their sum equals a given target.
- K Sum: Generalized problem: find all unique combinations of k numbers that sum to a given target.
- Requirements:
- Use distinct indices.
- Avoid duplicates.
- Output combinations in ascending order.
---

## 2. Constraints

- Array length varies by problem:
- 3 Sum: 1 ≤ n ≤ 3000
- 4 Sum: 1 ≤ n ≤ 200
- K Sum: 1 ≤ n ≤ 200
- Element range: -10^4 ≤ nums[i] ≤ 10^4
- Target range: -10^5 ≤ target ≤ 10^5
- Only valid unique combinations should be returned.
---

## 3. Edge Cases

- Arrays smaller than k.
- Arrays with all identical numbers.
- Arrays with no valid combination.
- Arrays with negative and positive numbers.
- Duplicates causing repeated results.
- Large positive/negative targets.
---

## 4. Examples

### Example 1:

```text
(3 Sum)
Input: nums = [2, -2, 0, 3, -3, 5]
Output: [[-3, 0, 3], [-2, 0, 2], [-3, -2, 5]]
Explanation: Each triplet sums to 0.
```

### Example 2:

```text
(4 Sum):
Input: nums = [1, -2, 3, 5, 7, 9], target = 7
Output: [[-2, 1, 3, 5]]
Explanation: -2 + 1 + 3 + 5 = 7.
```

### Example 3:

```text
(K Sum):
Input: nums = [1, 0, -1, 0, -2, 2], k = 4, target = 0
Output: [[-2, -1, 1, 2], [-2, 0, 0, 2], [-1, 0, 0, 1]]
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all possible combinations of elements (3 for 3 Sum, 4 for 4 Sum, k for K Sum) and check if their sum equals the target.
- This is straightforward but computationally expensive.

**Steps:**
- Use nested loops (3 for 3 Sum, 4 for 4 Sum, recursive for K Sum).
- Check if the combination sums to the target.
- Store unique sets after sorting to avoid duplicates.

**Java Code:**
```java
void kSumBruteForce(int[] nums, int k, int target, List<Integer> temp, List<List<Integer>> res, int start) {
    if (k == 0 && target == 0) {
        res.add(new ArrayList<>(temp));
        return;
    }
    if (k == 0 || start >= nums.length) return;
    for (int i = start; i < nums.length; i++) {
        temp.add(nums[i]);
        kSumBruteForce(nums, k - 1, target - nums[i], temp, res, i + 1);
        temp.remove(temp.size() - 1);
    }
}

// Example usage:
// 3 Sum
List<List<Integer>> res3 = new ArrayList<>();
kSumBruteForce(nums, 3, 0, new ArrayList<>(), res3, 0);

// 4 Sum
List<List<Integer>> res4 = new ArrayList<>();
kSumBruteForce(nums, 4, target, new ArrayList<>(), res4, 0);

// K Sum
List<List<Integer>> resK = new ArrayList<>();
kSumBruteForce(nums, k, target, new ArrayList<>(), resK, 0);
```

**Complexity (Time & Space):**
- 3 Sum → Time: O(n³), Space: O(3) for recursion stack
- 4 Sum → Time: O(n⁴), Space: O(4) for recursion stack
- K Sum → Time: O(nᵏ), Space: O(k) for recursion stack

### Approach 2: Better / Improved (Sorting + Two Pointers)

**Idea:**
- Sort the array and use the two-pointer technique to reduce nested loops by one level.
- Fix (k−2) elements, then use two pointers to find the remaining two.

**Steps:**
- Sort input array.
- Use nested loops to fix first elements.
- Apply two-pointer technique for remaining two.
- Avoid duplicates.

**Java Code:**
```java
List<List<Integer>> kSumBetter(int[] nums, int start, int k, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (k == 2) { // Base case: 2 Sum
        int left = start, right = nums.length - 1;
        while (left < right) {
            int sum = nums[left] + nums[right];
            if (sum == target) {
                res.add(Arrays.asList(nums[left], nums[right]));
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;
                left++; right--;
            } else if (sum < target) left++;
            else right--;
        }
        return res;
    }
    for (int i = start; i < nums.length - k + 1; i++) {
        if (i > start && nums[i] == nums[i - 1]) continue;
        for (List<Integer> subset : kSumBetter(nums, i + 1, k - 1, target - nums[i])) {
            List<Integer> temp = new ArrayList<>();
            temp.add(nums[i]);
            temp.addAll(subset);
            res.add(temp);
        }
    }
    return res;
}
```

**Complexity (Time & Space):**
- 3 Sum → Time: O(n²), Space: O(1)
- 4 Sum → Time: O(n³), Space: O(1)
- K Sum → Time: O(n^(k-1)), Space: O(k) recursion

### Approach 3: Optimal / Recursive Generalized K Sum

**Idea:**
- Recursively reduce K-Sum into smaller subproblems.
- Base case → 2 Sum using two-pointer method.
- Skip duplicates at all levels.

**Steps:**
- Sort array.
- Recursively call kSum for k-1 elements.
- Merge current element with results from recursion.
- Return all valid combinations.

**Java Code:**
```java
List<List<Integer>> kSumOptimal(int[] nums, int k, int target) {
    Arrays.sort(nums);
    return kSumHelper(nums, 0, k, target);
}

List<List<Integer>> kSumHelper(int[] nums, int start, int k, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (k == 2) {
        int left = start, right = nums.length - 1;
        while (left < right) {
            int sum = nums[left] + nums[right];
            if (sum == target) {
                res.add(Arrays.asList(nums[left], nums[right]));
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;
                left++; right--;
            } else if (sum < target) left++;
            else right--;
        }
        return res;
    }
    for (int i = start; i < nums.length - k + 1; i++) {
        if (i > start && nums[i] == nums[i - 1]) continue;
        for (List<Integer> subset : kSumHelper(nums, i + 1, k - 1, target - nums[i])) {
            List<Integer> temp = new ArrayList<>();
            temp.add(nums[i]);
            temp.addAll(subset);
            res.add(temp);
        }
    }
    return res;
}
```

**Complexity (Time & Space):**
- 3 Sum → Time: O(n²), Space: O(1)
- 4 Sum → Time: O(n³), Space: O(k) recursion
- K Sum → Time: O(n^(k-1)), Space: O(k) recursion

---

## 6. Justification / Proof of Optimality

- Brute Force: Simple but exponential, impractical for large n.
- Better / Two Pointer: Reduces nested loops, efficient for 3/4 Sum.
- Optimal Recursive: Elegant, generalizes for any K, avoids code repetition, scalable.
---

## 7. Variants / Follow-Ups

- 2 Sum (sorted/unsorted)
- 3 Sum Closest / 4 Sum Closest
- Count K-Sum combinations
- Return combinations nearest to target
- Use hash maps for optimized 4 Sum II variant
---

## 8. Tips & Observations

- Always sort the array first.
- Skip duplicates at every recursion/iteration level.
- Use two-pointer approach efficiently for base 2-Sum.
- Recursion stack grows with K; prune impossible paths early.
- Copy lists when adding to results to prevent mutation.
---

<!-- #endregion -->
