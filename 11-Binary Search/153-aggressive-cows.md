<!-- #region 153-Aggressive Cows -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q153: Aggressive Cows</h1>

## 1. Problem Understanding

- You have:
  * nums[]: positions of stalls
  * k: number of cows
- You must place k cows in these stalls such that:
- 👉 The minimum distance between any two cows is maximized.
---

## 2. Constraints

- 2 ≤ n ≤ 10^5
- 2 ≤ k ≤ n
- Stall positions up to 10^9
- Order is NOT guaranteed → must sort
---

## 3. Edge Cases

- All stalls at the same position → answer = 0
- If k = 2 → answer = max distance between farthest stalls
- Large values → must use long safe math
- Positions not sorted → sorting is required
---

## 4. Examples

```text
Example 1
n = 6, k = 4  
nums = [0, 3, 4, 7, 10, 9]
Output: 3

Example 2
n = 5, k = 2  
nums = [4, 2, 1, 3, 6]
Output: 5

Example 3
n = 5, k = 3  
nums = [10, 1, 2, 7, 5]
Output: 3
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try every possible minimum distance d from 1 to max-min,
- and check if we can place k cows with at least d spacing.

**Steps:**
- Sort stalls
- Range of answer = 1 → max(nums)-min(nums)
- For each distance d:
  * Try placing cows greedily
  * If ≥ k cows placed, answer could be d

**Java Code:**
```java
public int aggressiveCows(int[] nums, int k) {
    Arrays.sort(nums);
    int maxDist = nums[nums.length-1] - nums[0];

    for (int d = 1; d <= maxDist; d++) {
        if (canPlace(nums, d, k)) return d;
    }
    return -1;
}

private boolean canPlace(int[] nums, int d, int k) {
    int count = 1;  
    int lastPos = nums[0];

    for (int pos : nums) {
        if (pos - lastPos >= d) {
            count++;
            lastPos = pos;
            if (count == k) return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Try all distances → slow.

**Complexity (Time & Space):**
- ⏱ Time Complexity:
  * O(n * maxDistance)
  * maxDistance = max(nums) – min(nums)
  * For each possible distance d, we scan the full array to try placing cows.
- 💾 Space Complexity:
  * O(1)
  * No extra data structures used.

### Approach 2: Binary Search (l < h)

**Idea:**
- Binary search on possible distances d.
- We want maximum possible minimum distance.
- Search range:
  * low = 1
  * high = max(nums) - min(nums)

**Steps:**
- Sort the stalls
- While low < high:
  * mid = candidate distance
  * If cows can be placed with ≥ mid spacing → move right
    * → low = mid + 1
  * Else → move left
    * → high = mid
- Return low - 1 (because loop overshoots)

**Java Code:**
```java
public int aggressiveCows(int[] nums, int k) {
    Arrays.sort(nums);

    int low = 1;  
    int high = nums[nums.length-1] - nums[0];

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (canPlace(nums, mid, k)) {
            low = mid + 1;    // try for larger distance
        } else {
            high = mid;       // shrink right side
        }
    }

    return low - 1;           // overshoot correction
}

private boolean canPlace(int[] nums, int d, int k) {
    int count = 1;  
    int lastPos = nums[0];

    for (int pos : nums) {
        if (pos - lastPos >= d) {
            count++;
            lastPos = pos;
            if (count == k) return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- If we can place cows with mid spacing,
- we try to increase spacing (maximize).
- If not, shrink the spacing.
- This converges to the best spacing.

**Complexity (Time & Space):**
- ⏱ Time Complexity:
  * O(n * log(maxDistance))
  * log(maxDistance) → because we binary-search over possible distances
  * For every mid, we do O(n) greedy placement check.
- 💾 Space Complexity:
  * O(1)
  * Only variables, no extra arrays or memory.

### Approach 3: Binary Search (l <= h)

**Idea:**
- Check mid as candidate distance:
  * If valid → store mid, search right
  * If not valid → search left

**Steps:**
- low = 1
- high = maxDist
- While low ≤ high:
  * mid = (low+high)/2
  * If canPlace(mid) → ans = mid, low = mid + 1
  * Else → high = mid - 1
- Answer = ans

**Java Code:**
```java
public int aggressiveCows(int[] nums, int k) {
    Arrays.sort(nums);

    int low = 1;  
    int high = nums[nums.length-1] - nums[0];
    int ans = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canPlace(nums, mid, k)) {
            ans = mid;
            low = mid + 1;     // try larger distance
        } else {
            high = mid - 1;
        }
    }

    return ans;
}

private boolean canPlace(int[] nums, int d, int k) {
    int count = 1;    
    int lastPos = nums[0];

    for (int pos : nums) {
        if (pos - lastPos >= d) {
            count++;
            lastPos = pos;
            if (count == k) return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- We try to maximize the minimum distance.
- Binary search selects distances and checks validity.

**Complexity (Time & Space):**
- ⏱ Time Complexity:
  * O(n * log(maxDistance))
  * Same reasoning as above:
  * Binary search over distances
  * For each mid → linear greedy check.
- 💾 Space Complexity:
  * O(1)
  * Again just constant variables, no additional memory.

---

## 6. Justification / Proof of Optimality

- Binary search is applicable because:
- If we can place cows with distance d,
- we can surely place them with any smaller distance.
- This creates a monotonic pattern:
  * false false true true true true
- Binary search finds the first true going from right side.
- Thus valid.
---

## 7. Variants / Follow-Ups

- Magnetic Force Between Balls
- Placing k students in dorms
- Maximum minimum distance problems
- Parking lots problem
- Allocate routers with max-min signal gaps
---

## 8. Tips & Observations

- Always SORT the array
- Max distance = max−min
- Searching for maximum of a minimum means:
- mid valid → move RIGHT
- mid invalid → move LEFT
- Use greedy placement for cow assignment
- Stop early when cows placed = k
---

<!-- #endregion -->
