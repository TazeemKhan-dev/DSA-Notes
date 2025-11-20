<!-- #region 129-Pair in array with difference k -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q129: Pair in array with difference k</h1>

## 1. Problem Understanding

- You’re given an array nums and an integer k.
- A k-diff pair is defined as a pair (nums[i], nums[j]) such that:
  * i != j
  * nums[i] - nums[j] == k
  * Pairs must be unique, meaning (1,3) counted once even if elements repeat multiple times.
---

## 2. Constraints

- 1 <= n <= 10000
- 0 <= k <= 10^7
- 1 <= nums[i] <= 10^7
---

## 3. Edge Cases

- When k < 0: No pair possible because difference cannot be negative.
- When k == 0: We are looking for numbers that appear at least twice.
- Duplicate elements must not create duplicate pairs.
- Large numbers but manageable constraints.
---

## 4. Examples

```text
Example 1

Input: nums = [3, 1, 4, 1, 5], k = 2

Output: 2

Pairs: (1,3), (3,5)

Example 2

Input: nums = [1, 3, 1, 5, 4], k = 0

Output: 1

Pair: (1,1) since 1 appears twice.
```

---

## 5. Approaches

### Approach 1: Using HashSet + HashMap (Most Efficient)

**Idea:**
- For k > 0:
  * If a number x exists, check if x + k also exists.
- For k == 0:
  * Count numbers that occur at least twice.
- Use set/hashmap to ensure uniqueness.

**Steps:**
- If k < 0 → return 0.
- Build a frequency map of all numbers.
- If k == 0 → count numbers with frequency >= 2.
- If k > 0 → for each number x, check if x + k exists.
- Count such unique pairs.

**Java Code:**
```java
public static int findPairs(int[] nums, int k) {
    if(k < 0) return 0;

    HashMap<Integer, Integer> map = new HashMap<>();
    for(int x : nums) map.put(x, map.getOrDefault(x, 0) + 1);

    int count = 0;

    if(k == 0){
        for(int key : map.keySet()){
            if(map.get(key) >= 2) count++;
        }
    } else {
        for(int key : map.keySet()){
            if(map.containsKey(key + k)) count++;
        }
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- A difference k basically means:
- Find pairs where second number is exactly k more than the first.
- HashMap allows O(1) lookup to check whether the "partner element" exists.
- Using keys avoids double counting duplicates.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Because:
    * Building hashmap → O(n)
    * Iterating keys → O(n)
    * Each lookup → O(1)
- 💾 Space Complexity
  * O(n)
  * Because hashmap stores frequencies of up to n elements.

### Approach 2: Sorting + Two Pointers

**Idea:**
- Sort the array.
- Use two pointers i and j and ensure:
  * nums[j] - nums[i] == k
  * i != j
- Skip duplicates to maintain unique pairs.

**Steps:**
- Sort array.
- Keep two pointers:
  * If difference < k → move j.
  * If difference > k → move i.
  * If equal → record pair, move both and skip duplicates.

**Java Code:**
```java
public static int findPairs(int[] nums, int k) {
    Arrays.sort(nums);
    int i = 0, j = 1, count = 0;
    int n = nums.length;

    while(i < n && j < n){
        if(i == j){
            j++;
            continue;
        }

        int diff = nums[j] - nums[i];

        if(diff < k){
            j++;
        } else if(diff > k){
            i++;
        } else {
            count++;
            int a = nums[i];
            int b = nums[j];
            while(i < n && nums[i] == a) i++;
            while(j < n && nums[j] == b) j++;
        }
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Sorting groups equal elements together → easy to skip duplicates.
- The difference increases automatically as j moves right.
- Two pointers ensure linear scanning instead of nested loops.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n log n)
  * Because of sorting.
    * Two-pointer scan is O(n).
- 💾 Space Complexity
  * O(1) or O(n) depending on sorting implementation.

### Approach 3: Brute Force (For Understanding Only)

**Idea:**
- Compare each pair (i,j) using nested loops.
- Use a set to ensure unique pairs.

**Java Code:**
```java
public static int findPairs(int[] nums, int k) {
    HashSet<String> set = new HashSet<>();
    int n = nums.length;

    for(int i=0;i<n;i++){
        for(int j=i+1;j<n;j++){
            if(nums[i] - nums[j] == k){
                int a = nums[j];
                int b = nums[i];
                set.add(a + "#" + b);
            }
            if(nums[j] - nums[i] == k){
                int a = nums[i];
                int b = nums[j];
                set.add(a + "#" + b);
            }
        }
    }

    return set.size();
}
```

**💭 Intuition Behind the Approach:**
- Straightforward checking of all possible pairs.
- Using a string key avoids counting a pair twice.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n^2)
  * Because all pairs are checked.
- 💾 Space Complexity
  * O(n) due to set storing unique pairs.

---

## 6. Justification / Proof of Optimality

- Approach 1 is optimal because hash lookups allow instant pairing.
- Sorting approach is also good but slower.
- Brute force is only for understanding.
---

## 7. Variants / Follow-Ups

- Count unordered k-diff pairs → modify condition to abs(nums[i] - nums[j]) == k.
- Return the pairs themselves, not just count.
- Handle extremely large inputs (stream-based frequency counting).
---

## 8. Tips & Observations

- When k == 0, the problem becomes count duplicates.
- Avoid overcounting: sets/maps are crucial.
- Difference pairs are directional; (a,b) is same as (b,a) if difference fixed.
---

<!-- #endregion -->
