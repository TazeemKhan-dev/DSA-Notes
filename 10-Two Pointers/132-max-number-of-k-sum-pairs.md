<!-- #region 132-Max Number of K-Sum Pairs -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q132: Max Number of K-Sum Pairs</h1>

## 1. Problem Understanding

- We are given an array nums and an integer k.
- You can perform operations where you remove two numbers whose sum is exactly k.
- Return the maximum number of operations you can perform.
---

## 2. Constraints

- 1 <= nums.length <= 100000
- 1 <= nums[i] <= 10^9
- 0 <= k <= 10^9
- Large input → must aim for O(n) or O(n log n) solutions.
---

## 3. Edge Cases

- All numbers greater than k → no pairs.
- k = 0 edge half-case (pairs must be identical numbers).
- Frequent duplicates.
- Large values → only integer arithmetic needed.
- Only one valid pair existing.
---

## 4. Examples

```text
Example 1
nums = [1,2,3,4], k = 5 → output = 2
(1+4), (2+3)

Example 2
nums = [3,1,3,4,3], k = 6 → output = 1
Only possible pair: (3,3)
```

---

## 5. Approaches

### Approach 1: Brute Force (O(n²))

**Idea:**
- For each element, search for its complement k - nums[i].
- If found and not used, remove both.
- Continue until no more pairs.

**Java Code:**
```java
public static int maxOperations(int[] nums, int k) {
    int n = nums.length;
    boolean[] used = new boolean[n];
    int ops = 0;

    for (int i = 0; i < n; i++) {
        if (used[i]) continue;
        for (int j = i + 1; j < n; j++) {
            if (!used[j] && nums[i] + nums[j] == k) {
                used[i] = used[j] = true;
                ops++;
                break;
            }
        }
    }
    return ops;
}
```

**💭 Intuition Behind the Approach:**
- Directly check all pairs.
- Works but extremely slow for large inputs.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n²) because every pair of indices is checked.
- 💾 Space Complexity
  * O(n) for the used[] array.

### Approach 2: Sorting + Two Pointers (O(n log n))

**Idea:**
- Sort the array.
- Use two pointers (l, r):
- If nums[l] + nums[r] == k → count operation, move both.
- If sum < k → move left.
- If sum > k → move right.

**Java Code:**
```java
public static int maxOperations(int[] nums, int k) {
    Arrays.sort(nums);
    int l = 0, r = nums.length - 1, ops = 0;

    while (l < r) {
        int sum = nums[l] + nums[r];
        if (sum == k) {
            ops++;
            l++;
            r--;
        } else if (sum < k) {
            l++;
        } else {
            r--;
        }
    }
    return ops;
}
```

**💭 Intuition Behind the Approach:**
- Sorting helps control the sum direction.
- Two pointers efficiently find matching pairs.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n log n) due to sorting.
  * Two-pointer scan is O(n).
- 💾 Space Complexity
  * O(1) ignoring sorting space.

### Approach 3: HashMap Counting (Optimal O(n))

**Idea:**
- Use a frequency map:
  * For each number x, its required partner is k - x.
  * If partner exists in map:
    * Use one from map → count operation.
  * Else:
    * Store current number in map.
- Very efficient since each element is processed once.

**Java Code:**
```java
public static int maxOperations(int[] nums, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int ops = 0;

    for (int x : nums) {
        int need = k - x;

        if (map.getOrDefault(need, 0) > 0) {
            ops++;
            map.put(need, map.get(need) - 1);
        } else {
            map.put(x, map.getOrDefault(x, 0) + 1);
        }
    }
    return ops;
}
```

**💭 Intuition Behind the Approach:**
- For each number, try to find its partner immediately.
- If partner previously seen → form pair.
- Else store number.
- Map ensures constant-time lookup → very fast.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Each element inserted or matched exactly once.
- 💾 Space Complexity
  * O(n) in worst case (no pairs).

---

## 6. Justification / Proof of Optimality

- Brute force is too slow for n = 10^5.
- Sorting+two-pointers is good, clean, commonly used.
- HashMap is optimal, used in almost all top solutions.
- HashMap approach avoids sorting and matches pairs instantly.
---

## 7. Variants / Follow-Ups

- Count unique pairs instead of removing.
- Print actual pairs instead of count.
- Generalize to k-sum using hashing.
- Use multiset/map instead of hashmaps in languages like C++.
---

## 8. Tips & Observations

- Any problem where you match “two numbers summing to k” strongly suggests:
  * HashMap
  * Frequency count
  * Two pointers after sorting
- Removing elements = choosing pairs; HashMap excels at this.
---

<!-- #endregion -->
