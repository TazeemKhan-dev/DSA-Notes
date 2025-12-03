<!-- #region 151-Koko eating bananas -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q151: Koko eating bananas</h1>

## 1. Problem Statement

- You are given n piles of bananas, where the i-th pile contains piles[i] bananas.
- Koko can choose an integer eating speed k (bananas per hour).
- Each hour:
  * She selects one pile.
  * She eats k bananas from that pile.
  * If the pile has fewer than k bananas → she eats the whole pile and the rest of the hour is idle.
- The guards will return in h hours, and Koko wants to finish eating all the bananas before they return.
- You must return the minimum integer eating speed k such that Koko finishes all piles within h hours.
---

## 2. Problem Understanding

- This problem is asking:
  * What is the slowest speed k at which Koko can still finish all bananas within exactly h hours?
- Key details:
  * Koko eats from only one pile per hour.
  * If she eats k bananas per hour:
    * A pile of size p requires ceil(p / k) hours to finish.
  * Total hours needed is:
    * sum( ceil(piles[i] / k) ) for all i
  * As k increases:
    * Eating becomes faster.
    * The total hours needed decreases.
  * As k decreases:
    * Eating becomes slower.
    * The total hours increases.
- We need to find the minimum k such that:
  * totalHours(k) <= h
- This forms a monotonically decreasing function with respect to k,
- which makes the problem a classic Binary Search on Answer (BSOA) problem.
---

<!-- #endregion -->

## 3. Constraints

- 1 ≤ n ≤ 10^4
- 1 ≤ nums[i] ≤ 10^9
- n ≤ h ≤ 10^9
- k must be an integer
- Answer range = 1 → max(nums[])
---

## 4. Edge Cases

- If h = n → Koko must finish each pile in 1 hour → k = max(nums)
- If all piles are size 1 → answer is 1
- Very large values (up to 10^9)
- h extremely large → k can be very small
- Single pile case
---

## 5. Examples

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

## 6. Approaches

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

### Approach 2: Binary Search  (l <= h) (Classic Search Style)

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

## 7. Justification / Proof of Optimality

- This works because:
- hours(k) = sum(ceil(nums[i]/k))
- is a monotonically decreasing function of k.
- Meaning:
  * If k1 < k2  → hours(k1) >= hours(k2)
- So valid/invalid speeds form:
  * invalid invalid invalid valid valid valid
- Binary search can find the first valid speed.
---

## 8. Variants / Follow-Ups

- Minimum ship capacity
- Smallest divisor
- Split array largest sum
- Painter partition
- Minimum eating speed (LeetCode #875)
---

## 9. Tips & Observations

- Divisor must start at 1, not 0
- Upper bound is max(nums[])
- Use ceil division: (x + k - 1) / k
- Stop early when hours exceed h
- (l < h) → final candidate found via shrinking
- (l <= h) → explicitly track best candidate
---

<!-- #endregion -->
