<!-- #region 151-Koko eating bananas -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q151: Koko eating bananas</h1>

## 1. Problem Understanding

- You’re given:
  * n piles of bananas
  * nums[i] bananas in the i-th pile
  * h hours to finish all piles
  * Each hour, Koko chooses one pile and eats k bananas
  * If the pile has fewer than k bananas → she eats the entire pile and does not continue eating more that hour
- Goal:
  * Find the minimum integer k such that Koko can finish all bananas in ≤ h hours.
  * This is a classic Binary Search on Answer problem.
---

## 2. Constraints

- 1 ≤ n ≤ 10^4
- 1 ≤ nums[i] ≤ 10^9
- n ≤ h ≤ 10^9
- k must be an integer
- Answer range = 1 → max(nums[])
---

## 3. Edge Cases

- If h = n → Koko must finish each pile in 1 hour → k = max(nums)
- If all piles are size 1 → answer is 1
- Very large values (up to 10^9)
- h extremely large → k can be very small
- Single pile case
---

## 4. Examples

```text
Example 1

Input: nums = [7, 15, 6, 3], h = 8
Output: 5

Example 2

Input: nums = [25, 12, 8, 14, 19], h = 5
Output: 25

Example 3

Input: nums = [3, 7, 6, 11], h = 8
Output: 3
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all possible speeds k from 1 to max(nums[]).
- For each speed, calculate hours needed.

**Steps:**
- Compute max element = maxK
- For k = 1 → maxK:
  * Compute hours = sum(ceil(nums[i] / k))
  * If hours ≤ h → return k

**Java Code:**
```java
public int minEatingSpeed(int[] nums, int h) {
    int max = 0;
    for (int x : nums) max = Math.max(max, x);

    for (int k = 1; k <= max; k++) {
        long hours = 0;
        for (int x : nums) {
            hours += (x + k - 1) / k;
        }
        if (hours <= h) return k;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Check every possible eating speed. Very inefficient.

**Complexity (Time & Space):**
- O(max(nums) * n) → Too slow
- Space: O(1)

### Approach 2: Binary Search (l < h) (Range-Shrinking Method)

**Idea:**
- Search over possible speeds k in range 1 → max(nums[]).
- If she eats faster → hours decrease → monotonic behavior.

**Steps:**
- low = 1
- high = max(nums)
- While low < high:
  * mid = speed
  * If mid is valid → shrink right side (high = mid)
  * Else → increase low (low = mid + 1)
- Answer is low

**Java Code:**
```java
public int minEatingSpeed(int[] nums, int h) {
    int low = 1, high = 0;
    for (int x : nums) high = Math.max(high, x);

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (canEat(nums, mid, h)) {
            high = mid;      // keep mid, try smaller
        } else {
            low = mid + 1;   // need more speed
        }
    }

    return low;
}

private boolean canEat(int[] nums, int k, int h) {
    long hours = 0;
    for (int x : nums) {
        hours += (x + k - 1) / k;
        if (hours > h) return false;
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- We shrink the speed range until only the minimum valid speed remains.

**Complexity (Time & Space):**
- O(n * log(max(nums)))
- Space: O(1)

### Approach 3: Binary Search (l <= h) (Classic Search Style)

**Idea:**
- Binary search to find the smallest k such that hours ≤ h.

**Steps:**
- low = 1
- high = max(nums)
- mid = guess speed
- If mid works → store mid, go left
- Else → go right

**Java Code:**
```java
public int minEatingSpeed(int[] nums, int h) {
    int low = 1, high = 0;
    for (int x : nums) high = Math.max(high, x);

    int ans = high;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canEat(nums, mid, h)) {
            ans = mid;       // candidate
            high = mid - 1;  // try smaller
        } else {
            low = mid + 1;   // need more speed
        }
    }

    return ans;
}

private boolean canEat(int[] nums, int k, int h) {
    long hours = 0;
    for (int x : nums) {
        hours += (x + k - 1) / k;
        if (hours > h) return false;
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Keep reducing possible speed while tracking smallest valid speed.

**Complexity (Time & Space):**
- O(n * log(max(nums)))
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- This works because:
- hours(k) = sum(ceil(nums[i]/k))
- is a monotonically decreasing function of k.
- Meaning:
  * If k1 < k2  → hours(k1) >= hours(k2)
- So valid/invalid speeds form:
  * invalid invalid invalid valid valid valid
- Binary search can find the first valid speed.
---

## 7. Variants / Follow-Ups

- Minimum ship capacity
- Smallest divisor
- Split array largest sum
- Painter partition
- Minimum eating speed (LeetCode #875)
---

## 8. Tips & Observations

- Divisor must start at 1, not 0
- Upper bound is max(nums[])
- Use ceil division: (x + k - 1) / k
- Stop early when hours exceed h
- (l < h) → final candidate found via shrinking
- (l <= h) → explicitly track best candidate
---

<!-- #endregion -->
