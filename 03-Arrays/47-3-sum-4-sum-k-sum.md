<!-- #region 47-3 Sum, 4 Sum, K Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q47: 3 Sum, 4 Sum, K Sum</h1>

## 1. Problem Understanding

- 3-Sum: Find all unique triplets a + b + c = 0.
- 4-Sum: Find all unique quadruplets a + b + c + d = target.
- K-Sum: Generalized version: find all unique combinations of size k that sum to target.
- Order of elements inside the tuple does NOT matter (we sort each tuple).
- No duplicates allowed, meaning two identical sets like [1, -1, 0] must appear only once.
---

## 2. Constraints

- 1 ≤ n ≤ 10^5
- Numbers may be positive, negative, or zero.
- Need efficient deduplication logic.
- Must sort array for optimal solutions.
---

## 3. Edge Cases

- Array with < k elements → return empty.
- Large values → possible overflow (use long when summing).
- Many duplicates → skip duplicates carefully.
- Target may be large or negative.
---

## 4. Examples

```text
Example 1:
(3 Sum)
Input: nums = [2, -2, 0, 3, -3, 5]
Output: [[-3, 0, 3], [-2, 0, 2], [-3, -2, 5]]
Explanation: Each triplet sums to 0.
Example 2:
(4 Sum):
Input: nums = [1, -2, 3, 5, 7, 9], target = 7
Output: [[-2, 1, 3, 5]]
Explanation: -2 + 1 + 3 + 5 = 7.
Example 3:
(K Sum):
Input: nums = [1, 0, -1, 0, -2, 2], k = 4, target = 0
Output: [[-2, -1, 1, 2], [-2, 0, 0, 2], [-1, 0, 0, 1]]
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all combinations:
- 3-Sum: triple loop → i < j < k
- 4-Sum: 4 nested loops
- K-Sum: recursion generating all k-combinations
- Check if sum matches target and store unique sets in a Set.

**Java Code:**
```java
🔹 3-Sum (Brute)
public static List<List<Integer>> threeSumBrute(int[] nums) {
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                if (nums[i] + nums[j] + nums[k] == 0) {
                    List<Integer> temp = Arrays.asList(nums[i], nums[j], nums[k]);
                    Collections.sort(temp);
                    set.add(temp);
                }
            }
        }
    }
    return new ArrayList<>(set);
}

🔹 4-Sum (Brute)
public static List<List<Integer>> fourSumBrute(int[] nums, int target) {
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();

    for (int a = 0; a < n; a++) {
        for (int b = a + 1; b < n; b++) {
            for (int c = b + 1; c < n; c++) {
                for (int d = c + 1; d < n; d++) {
                    long sum = (long) nums[a] + nums[b] + nums[c] + nums[d];
                    if (sum == target) {
                        List<Integer> temp = Arrays.asList(nums[a], nums[b], nums[c], nums[d]);
                        Collections.sort(temp);
                        set.add(temp);
                    }
                }
            }
        }
    }
    return new ArrayList<>(set);
}

🔹 K-Sum (Brute via combinations)
public static List<List<Integer>> kSumBrute(int[] nums, int k, int target) {
    List<List<Integer>> ans = new ArrayList<>();
    backtrack(nums, k, target, 0, new ArrayList<>(), ans);
    return ans;
}

static void backtrack(int[] nums, int k, int target, int idx, List<Integer> temp, List<List<Integer>> ans) {
    if (temp.size() == k) {
        long sum = 0;
        for (int x : temp) sum += x;
        if (sum == target) {
            List<Integer> t = new ArrayList<>(temp);
            Collections.sort(t);
            ans.add(t);
        }
        return;
    }

    for (int i = idx; i < nums.length; i++) {
        temp.add(nums[i]);
        backtrack(nums, k, target, i + 1, temp, ans);
        temp.remove(temp.size() - 1);
    }
}
```

**💭 Intuition Behind the Approach:**
- Test all possible k-element combinations.
- Sorting each tuple & storing in a Set avoids duplicates.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * 3-Sum: O(n^3)
  * 4-Sum: O(n^4)
  * K-Sum: O(n^k)
  * Why: nested loops exploring all combinations.
- 💾 Space Complexity
  * O(#results) for storing all tuples.

### Approach 2: Sorting + Hashing (Better but not optimal)

**Idea:**
- Fix i
- For remaining array, we look for pairs nums[j] + nums[k] = -nums[i]
- Use HashMap to store seen values
- Store triplets in a Set<List<Integer>> to remove duplicates

**Java Code:**
```java
Idea:
Fix one number → reduce to 2-sum using HashMap.

Java Code
public static List<List<Integer>> threeSumHash(int[] nums) {
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();

    for (int i = 0; i < n; i++) {
        int target = -nums[i];
        HashMap<Integer,Integer> map = new HashMap<>();

        for (int j = i + 1; j < n; j++) {
            int x = target - nums[j];
            if (map.containsKey(x)) {
                List<Integer> temp = Arrays.asList(nums[i], nums[j], x);
                Collections.sort(temp);
                set.add(temp);
            }
            map.put(nums[j], j);
        }
    }
    return new ArrayList<>(set);
}

Complexity

O(n^2) average due to hash lookup

Removes duplicates using set

⭐ 4-Sum Using HashMap (Better)

Idea:
Fix two numbers → solve 2-Sum using HashMap.

Java Code
public static List<List<Integer>> fourSumHash(int[] nums, int target) {
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {

            long req = (long)target - nums[i] - nums[j];
            HashMap<Integer,Integer> map = new HashMap<>();

            for (int k = j + 1; k < n; k++) {
                int x = (int)(req - nums[k]);
                if (map.containsKey(x)) {
                    List<Integer> t = Arrays.asList(nums[i], nums[j], nums[k], x);
                    Collections.sort(t);
                    set.add(t);
                }
                map.put(nums[k], k);
            }
        }
    }
    return new ArrayList<>(set);
}

Complexity

O(n^3)
Better than O(n^4) brute

⭐ K-Sum HashMap

Not feasible because combinations explode.
Better to move directly to Optimal approach based on sorting + recursion.
```

### Approach 3: Sorting + Two-Pointer Pattern (Optimal for 3-Sum and 4-Sum)

**Idea:**
- Sort the array.
- For each fixed index, convert the remaining part into a 2-Sum (two-pointer).
- Skip duplicates aggressively.

**Java Code:**
```java
3-Sum (Optimal O(n²))
public static List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> ans = new ArrayList<>();
    int n = nums.length;

    for (int i = 0; i < n; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        int l = i + 1, r = n - 1;

        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum == 0) {
                ans.add(Arrays.asList(nums[i], nums[l], nums[r]));
                l++; r--;
                while (l < r && nums[l] == nums[l - 1]) l++;
                while (l < r && nums[r] == nums[r + 1]) r--;
            } else if (sum < 0) l++;
            else r--;
        }
    }
    return ans;
}

🔹 4-Sum (Optimal O(n³))
public static List<List<Integer>> fourSum(int[] nums, int target) {
    Arrays.sort(nums);
    List<List<Integer>> ans = new ArrayList<>();
    int n = nums.length;

    for (int i = 0; i < n; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        for (int j = i + 1; j < n; j++) {
            if (j > i + 1 && nums[j] == nums[j - 1]) continue;

            int l = j + 1, r = n - 1;

            while (l < r) {
                long sum = (long) nums[i] + nums[j] + nums[l] + nums[r];

                if (sum == target) {
                    ans.add(Arrays.asList(nums[i], nums[j], nums[l], nums[r]));
                    l++; r--;
                    while (l < r && nums[l] == nums[l - 1]) l++;
                    while (l < r && nums[r] == nums[r + 1]) r--;
                } else if (sum < target) l++;
                else r--;
            }
        }
    }
    return ans;
}

🔹 K-Sum (Optimal O(n^(k-1)))

Recursive reduction to 2-Sum.

public static List<List<Integer>> kSum(int[] nums, int k, int target) {
    Arrays.sort(nums);
    return kSumHelper(nums, 0, k, target);
}

static List<List<Integer>> kSumHelper(int[] nums, int start, int k, long target) {
    List<List<Integer>> ans = new ArrayList<>();
    int n = nums.length;

    if (k == 2) {
        int l = start, r = n - 1;
        while (l < r) {
            long sum = nums[l] + nums[r];
            if (sum == target) {
                ans.add(Arrays.asList(nums[l], nums[r]));
                l++; r--;
                while (l < r && nums[l] == nums[l - 1]) l++;
                while (l < r && nums[r] == nums[r + 1]) r--;
            } else if (sum < target) l++;
            else r--;
        }
        return ans;
    }

    for (int i = start; i < n; i++) {
        if (i > start && nums[i] == nums[i - 1]) continue;
        for (List<Integer> subset : kSumHelper(nums, i + 1, k - 1, target - nums[i])) {
            List<Integer> newList = new ArrayList<>();
            newList.add(nums[i]);
            newList.addAll(subset);
            ans.add(newList);
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Reduce K-Sum → (K-1)-Sum → … → 2-Sum.
- Sorted array ensures duplicate skipping works.
- Two-pointers solve 2-Sum in O(n).
- Recursion manages higher-order sets.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * 3-Sum: O(n²)
  * 4-Sum: O(n³)
  * K-Sum: O(n^(k-1))
  * Why: each level of recursion loops through array once.
- 💾 Space Complexity
  * Recursion depth = k
  * O(k) + output space.

---

## 6. Justification / Proof of Optimality

- ✔ Brute Force
  * Generates all combinations → guaranteed correct but too slow.
  * Good for understanding but useless for constraints > 300.
- ✔ HashMap-Based Better Approach
  * Reduces nested loops:
  * 3-Sum: O(n^2)
  * 4-Sum: O(n^3)
  * Works by converting inner loops into 2-Sum.
  * Still not optimal but much faster than brute.
- ✔ Optimal K-Sum Approach
  * Solves the entire family (3-sum, 4-sum, k-sum)
  * Sorting helps remove duplicates and enable two-pointer
  * Recursion breaks problem into smaller sums efficiently
  * Most scalable and accepted on LeetCode
---

## 7. Variants / Follow-Ups

- Find any ONE triplet / quadruplet (not unique)
- Find number of triplets not the combinations
- Find combinations where product = target
- k-sum closest
---

## 8. Tips & Observations

- Always sort before two-pointers
- Always skip duplicates
- Use long for sum to avoid overflow
- For k ≥ 4, recursion is natural and elegant
---

<!-- #endregion -->
