<!-- #region 168-Minimum Time to Complete Trips -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q168: Minimum Time to Complete Trips</h1>

## 1. Problem Statement

- You are given an array time where time[i] is the time taken by the i-th bus to complete one trip.
  * A bus can make multiple trips back-to-back.
  * Each bus runs independently.
  * You are given totalTrips, the total combined number of trips that all buses must complete together.
- Your task is to compute:
- The minimum time required so that all buses together complete at least totalTrips trips.
---

## 2. Problem Understanding

- This problem asks:
  * After how many units of time will the combined trips made by all buses be ≥ totalTrips?
- Each bus has its own speed:
  * Bus with time[i] = 1 → 1 trip per unit time
  * Bus with time[i] = 2 → 1 trip every 2 units
  * Bus with time[i] = 3 → 1 trip every 3 units
- At time t, number of trips bus i completes is:
  * t / time[i]
- So total trips at time t is:
  * sum( t / time[i] ) for all i
- We need to find the minimum t such that:
  * sum( t / time[i] ) >= totalTrips
- This is a Binary Search On Answer (BSOA) problem because:
  * “Trips completed” is a monotonically increasing function of time.
  * More time → more trips.
  * Less time → fewer trips.
  * We want the smallest time for which trips ≥ required.
---

## 3. Constraints

- 1 <= time.length <= 100000
- 1 <= time[i] <= 10^7
- 1 <= totalTrips <= 10^7
- time[] is not sorted
- All buses are independent
---

## 4. Edge Cases

- Only one bus → answer = time[i] * totalTrips
- Large totalTrips → time must be long (needs long type)
- Very slow buses may contribute almost nothing
- The k-th trip may be completed by any bus (no fixed assignment)
---

## 5. Examples

```text
Example 1
Input: time = [1,2,3], totalTrips = 5

At t = 1 → trips = [1,0,0] = 1  
At t = 2 → trips = [2,1,0] = 3  
At t = 3 → trips = [3,1,1] = 5  ✔ condition satisfied

Output: 3

Example 2
Input: time = [2], totalTrips = 1
Bus finishes first trip at time = 2
Output: 2
```

---

## 6. Approaches

### Approach 1: Brute Force (Simulate Time)

**Idea:**
- Start from time = 1 and keep increasing
- → Count trips
- → Stop when total ≥ totalTrips
- Issues
  * Time may go up to 10^14
  * Impossible to simulate

**💭 Intuition Behind the Approach:**
- Brute force tries to increment time until enough trips accumulate.

**Complexity (Time & Space):**
- O(TotalTime) → NOT feasible.

### Approach 2: Binary Search on Time (Optimal BSOA)

**Idea:**
- If we guess a time t, we can compute all trips:
  * trips = sum(t / time[i])
- If trips >= totalTrips → t might be answer, try smaller time
- Else → t is too small, try bigger time
- Key Insight
  * Trips(t) is monotonic increasing, so binary search works.

**Steps:**
- low = 1
- high = max(time) * totalTrips
- Binary search:
  * mid = (low + high) / 2
  * If total trips at mid ≥ totalTrips → store mid, search left
  * Else → search right

**Java Code:**
```java
class Solution {
    public long minimumTime(int[] time, int totalTrips) {
        long low = 1;
        long maxVal = 0;
        
        for (int t : time)
            maxVal = Math.max(maxVal, t);

        long high = maxVal * (long) totalTrips;  // upper bound

        long ans = high;

        while (low <= high) {
            long mid = low + (high - low) / 2;

            if (isPossible(time, mid, totalTrips)) {
                ans = mid;
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }

        return ans;
    }

    private boolean isPossible(int[] time, long mid, long totalTrips) {
        long trips = 0;

        for (int t : time) {
            trips += mid / t;
            if (trips >= totalTrips) return true; // avoid overflow
        }

        return false;
    }
}
```

**💭 Intuition Behind the Approach:**
- More time means more trips, so solution lies in time-space.
- You need minimum time → shrink to left when condition satisfied.
- mid / time[i] gives how many trips each bus can complete within mid.

**Complexity (Time & Space):**
- Time:
  * O(n * log(maxTime * totalTrips))
  * Because each mid computation loops over all buses.
  * Why:
    * Binary search reduces answer range exponentially.
    * Each mid requires scanning all buses.
- Space:
  * O(1)
  * Only scalar variables.

---

## 7. Justification / Proof of Optimality

- The function f(t) = sum(t / time[i]) is monotonic.
- Binary search finds the smallest t for which f(t) becomes ≥ required.
- No faster method exists because:
  * You must evaluate all buses for each mid.
  * Time domain can be huge (10^14), so binary search is required.
---

## 8. Variants / Follow-Ups

- Minimum time to paint boards (Painter’s Partition Variant)
- Minimum days to make bouquets
- Minimum speed to arrive on time
- Koko eating bananas
- All Binary Search on Answer (BSOA) questions
---

## 9. Tips & Observations

- Upper bound must be max(time) * totalTrips
- Always use long for:
  * mid
  * trips counter
  * upper bound
- Early stopping inside isPossible() avoids integer overflow
- Remember: Slow buses contribute very little
---

## 10. Pitfalls

- Using int instead of long → overflow fails tests
- Setting incorrect upper bound (using min instead of max)
- Forgetting early stopping → overflow
- Using mid as int → incorrect calculations
---

<!-- #endregion -->
