<!-- #region 41-Two Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q41: Two Sum</h1>

## 1. Problem Understanding

- You are given:
- An integer array nums
- An integer target
- Your task:
  * Find two indices i and j such that:
  * nums[i] + nums[j] == target
- Always guaranteed that one valid pair exists.
- Return the indices in increasing order.
---

## 2. Constraints

- 1 <= n <= 10^5
- Array may contain negative and positive numbers
- Must optimize for large input
- Indices must be returned in sorted order
---

## 3. Edge Cases

- Duplicate values may exist
- Negative + positive might form the target
- If target can be formed by two identical numbers → their separate indices must be chosen
- Large value ranges, so brute force may TLE
---

## 4. Examples

```text
Example 1

nums = [2,7,5,11], target = 9
Output: 0 1

Example 2

nums = [5,2,4], target = 6
Output: 1 2
```

---

## 5. Approaches

### Approach 1: HashMap (Optimal O(n))

**Idea:**
- ✔ Idea
- For each number x, check if target - x has been seen before.
- Use a HashMap:
  * Key → number
  * Value → index

**Steps:**
- Loop over array
- For each index i, compute rem = target - nums[i]
- If rem exists in map → return indices
- Otherwise put nums[i] → i into map

**Java Code:**
```java
public static int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums.length; i++) {
        int rem = target - nums[i];

        if (map.containsKey(rem)) {
            int a = map.get(rem);
            int b = i;
            return new int[]{Math.min(a, b), Math.max(a, b)};
        }

        map.put(nums[i], i);
    }

    return new int[]{-1, -1}; // won't happen as per constraints
}
```

**💭 Intuition Behind the Approach:**
- We want two numbers that sum to target.
- HashMap allows instant lookup for the complement.
- Each element is visited once → best possible performance.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Because each element is inserted/queried once in HashMap.
- 💾 Space Complexity
  * O(n)
  * Because HashMap may store all elements.

### Approach 2: Sorting + Two Pointers (O(n log n))

**Idea:**
- Create pairs: (value, originalIndex)
- Sort by value
- Use two pointers:
  * l = 0, r = n-1
- If arr[l] + arr[r] == target → return original indices
- If sum < target → increase l
- If sum > target → decrease r

**Steps:**
- Sorting ensures ordered movement of pointers.
- Must store original indices before sorting.

**Java Code:**
```java
public static int[] twoSum(int[] nums, int target) {
    int n = nums.length;
    int[][] arr = new int[n][2];

    for (int i = 0; i < n; i++) {
        arr[i][0] = nums[i];   // value
        arr[i][1] = i;         // original index
    }

    Arrays.sort(arr, (a, b) -> a[0] - b[0]);

    int l = 0, r = n - 1;

    while (l < r) {
        int sum = arr[l][0] + arr[r][0];

        if (sum == target) {
            int a = arr[l][1];
            int b = arr[r][1];
            return new int[]{Math.min(a, b), Math.max(a, b)};
        }

        if (sum < target) l++;
        else r--;
    }

    return new int[]{-1, -1};
}
```

**💭 Intuition Behind the Approach:**
- Sorting arranges numbers so that moving left/right pointers can converge to target.
- Similar to classic two-pointer problems.
- Requires preserving original indices before sorting.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n log n) — sorting dominates.
  * Two-pointer scan is O(n).
- 💾 Space Complexity
  * O(n) for storing pairs.

### Approach 3: Brute Force (O(n²), for understanding only)

**Idea:**
- Check every pair (i, j) and compare sum to target.

**Java Code:**
```java
public static int[] twoSum(int[] nums, int target) {
    int n = nums.length;

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }

    return new int[]{-1, -1};
}
```

**💭 Intuition Behind the Approach:**
- Straightforward brute-force checking.
- Works but slow for large arrays.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n²) due to nested loops.
- 💾 Space Complexity
  * O(1).

---

## 6. Justification / Proof of Optimality

- HashMap is the most efficient method (O(n)).
- Two-pointer approach is clean and logically elegant (O(n log n)).
- Brute force is simplest but slow.
---

## 7. Variants / Follow-Ups

- Return the numbers instead of indices.
- Count number of valid pairs.
- Find unique pairs (no duplicate pairs allowed).
- Solve in a stream of numbers (online two sum).
- Two sum for sorted array → direct 2-pointer.
---

## 8. Tips & Observations

- Always keep original indices if sorting is involved.
- HashMap is the best solution unless memory constraints apply.
- Complement lookup technique appears in many problems:
  * Two Sum
  * Target Sum Triplets
  * Pair With Difference K
---

<!-- #endregion -->
