<!-- #region 152-Minimum days to make M bouquets -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q152: Minimum days to make M bouquets</h1>

## 1. Problem Understanding

- We have:
  * nums[i] = the day the i-th rose blooms
  * Each bouquet needs k adjacent bloomed roses
  * We need m bouquets
- We must find the minimum number of days after which we can form at least m bouquets.
- If impossible → return -1.
---

## 2. Constraints

- 1 <= n <= 10^5
- 1 <= nums[i] <= 10^9
- 1 <= m <= 10^6
- 1 <= k <= n
---

## 3. Edge Cases

- If m * k > n → impossible → return -1
- If k = 1 → we just need m flowers
- Large bloom days (up to 10^9)
- All flowers bloom at same time
- Very large m (up to 1e6)
---

## 4. Examples

```text
Example 1

Input: nums=[7,7,7,7,13,11,12,7], m=2, k=3
Output: 12

Example 2

Input: nums=[1,10,3,10,2], m=3, k=2
Output: -1

Example 3

Input: nums=[1,10,3,10,2], m=3, k=1
Output: 3
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try every possible day from min(nums) to max(nums) and check if we can form m bouquets.

**Steps:**
- For each day d:
  * Count adjacent bloomed roses
  * Form bouquets
  * If bouquets ≥ m → return d

**Java Code:**
```java
public int minDays(int[] nums, int m, int k) {
    if ((long)m * k > nums.length) return -1;

    int low = Integer.MAX_VALUE, high = 0;
    for (int x : nums) {
        low = Math.min(low, x);
        high = Math.max(high, x);
    }

    for (int d = low; d <= high; d++) {
        if (canMake(nums, d, m, k)) return d;
    }
    return -1;
}

boolean canMake(int[] nums, int day, int m, int k) {
    int bouquets = 0, cnt = 0;
    for (int x : nums) {
        if (x <= day) {
            cnt++;
            if (cnt == k) {
                bouquets++;
                cnt = 0;
            }
        } else cnt = 0;
    }
    return bouquets >= m;
}
```

**💭 Intuition Behind the Approach:**
- Check all possible days → too slow.

**Complexity (Time & Space):**
- O((max-min) * n) → TLE
- Space: O(1)

### Approach 2: Binary Search (l < h)

**Idea:**
- Day range is from:
  * min(nums) → max(nums)
- If we can make ≥ m bouquets on day d, then all days after are also valid → monotonic.
- Thus binary search works.

**Steps:**
- low = min(nums)
- high = max(nums)
- While low < high:
  * mid = candidate day
  * If possible on mid → shrink right → high = mid
  * Else → search right → low = mid + 1
- Final answer = low

**Java Code:**
```java
public int minDays(int[] nums, int m, int k) {
    if ((long)m * k > nums.length) return -1;

    int low = Integer.MAX_VALUE, high = 0;
    for (int x : nums) {
        low = Math.min(low, x);
        high = Math.max(high, x);
    }

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (canMake(nums, mid, m, k)) {
            high = mid;   // keep mid
        } else {
            low = mid + 1;
        }
    }

    return low;
}

boolean canMake(int[] nums, int day, int m, int k) {
    int bouquets = 0, cnt = 0;

    for (int x : nums) {
        if (x <= day) {
            cnt++;
            if (cnt == k) {
                bouquets++;
                cnt = 0;
            }
        } else {
            cnt = 0;
        }
    }

    return bouquets >= m;
}
```

**💭 Intuition Behind the Approach:**
- Binary search shrinks toward the smallest day where enough roses bloom to form m bouquets.

**Complexity (Time & Space):**
- Time: O(n log(max(nums)))
- Space: O(1)

### Approach 3: Binary Search (l <= h)

**Idea:**
- Same search space as Approach 2, but we explicitly track answer.

**Steps:**
- If can make bouquets at mid:
  * save mid
  * go left
- Else:
  * go right

**Java Code:**
```java
public int minDays(int[] nums, int m, int k) {
    if ((long)m * k > nums.length) return -1;

    int low = Integer.MAX_VALUE, high = 0;
    for (int x : nums) {
        low = Math.min(low, x);
        high = Math.max(high, x);
    }

    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canMake(nums, mid, m, k)) {
            ans = mid;        // store candidate
            high = mid - 1;   // try smaller day
        } else {
            low = mid + 1;
        }
    }

    return ans;
}

boolean canMake(int[] nums, int day, int m, int k) {
    int bouquets = 0, cnt = 0;

    for (int x : nums) {
        if (x <= day) {
            cnt++;
            if (cnt == k) {
                bouquets++;
                cnt = 0;
            }
        } else {
            cnt = 0;
        }
    }

    return bouquets >= m;
}
```

**💭 Intuition Behind the Approach:**
- We track the earliest possible day mid that works, then refine by searching smaller days.

**Complexity (Time & Space):**
- Time: O(n log(max(nums)))
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Binary search is valid because:
- The function:
  * f(day) = number of bouquets that can be made on "day"
- is monotonic:
  * If we can make m bouquets on day d,
  * then for all d2 > d, we can also make m bouquets.
- Therefore pattern looks like:
  * false false false true true true
- Binary search finds the first true.
---

## 7. Variants / Follow-Ups

- Koko eating bananas
- Smallest divisor
- Split array largest sum
- Painter partition
- Make m bouquets (LeetCode 1482)
---

## 8. Tips & Observations

- MUST check feasibility: if m*k > n → -1
- "Adjacent" means contiguous segment
- Use integer division for blooming check
- For each mid, count consecutive flowers ≤ mid
- (l < h) → shrink to final day
- (l <= h) → track earliest day
- Bloom days up to 10^9 require binary search
---

<!-- #endregion -->
