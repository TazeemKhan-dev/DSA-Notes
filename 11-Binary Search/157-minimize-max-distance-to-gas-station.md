<!-- #region 157-Minimize Max Distance to Gas Station -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q157: Minimize Max Distance to Gas Station</h1>

## 1. Problem Understanding

- We are given positions of existing gas stations along the X-axis (sorted increasing).
- We must add k new gas stations anywhere (even in non-integer positions).
- Define:
  * dist = maximum distance between any two adjacent gas stations.
- We must place the k new stations such that dist becomes as small as possible.
- This is a Minimize Maximum optimization problem → solved via Binary Search on Answer (in optimal approach).
---

## 2. Constraints

- 10 ≤ n ≤ 5000
- 0 ≤ arr[i] ≤ 1e9
- Strictly increasing array
- 0 ≤ k ≤ 1e5
- Precision needed: 1e-6
---

## 3. Edge Cases

- k = 0 → answer = max(arr[i+1] - arr[i])
- One huge gap dominates answer
- All stations equally spaced
- High precision on doubles
- Very large values → require careful floating-point operations
---

## 4. Examples

```text
arr = [1..10], k = 9 → 0.5

arr = [1..10], k = 1 → 1.0

arr = [3,6,12,19,33,44,67,72,89,95], k=2 → ≈ 13–14
```

---

## 5. Approaches

### Approach 1: Brute Force (Simulation)

**Idea:**
- For each new gas station:
  * Find the current largest segment.
  * Place 1 gas station inside it, splitting it.
- Repeat k times.
- Uses an array howMany[] to track how many divisions each segment has.

**Steps:**
- Compute initial gaps.
- For each of the k gas stations:
  * For each segment:
    * Compute effective max segment length = (arr[i+1]-arr[i]) / (howMany[i]+1)
  * Find segment with largest effective length.
  * Add a gas station to that segment.
- After all insertions, compute the largest segment length.

**Java Code:**
```java
public static double bruteForce(int[] arr, int k) {
    int n = arr.length;
    int[] howMany = new int[n - 1];

    for (int gas = 1; gas <= k; gas++) {
        double maxSection = -1;
        int maxInd = -1;

        for (int i = 0; i < n - 1; i++) {
            double diff = arr[i + 1] - arr[i];
            double sectionLen = diff / (howMany[i] + 1);
            if (sectionLen > maxSection) {
                maxSection = sectionLen;
                maxInd = i;
            }
        }
        howMany[maxInd]++;
    }

    double ans = -1;
    for (int i = 0; i < n - 1; i++) {
        double diff = arr[i + 1] - arr[i];
        double secLen = diff / (howMany[i] + 1);
        ans = Math.max(ans, secLen);
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Split the largest gap each time.
- Matching the human intuition: reduce the largest interval.

**Complexity (Time & Space):**
- Time:
  * O(k * n) ← too slow for k = 1e5
- Space:
  * O(n)

### Approach 2: Better Approach (Priority Queue / Max Heap)

**Idea:**
- Instead of scanning all segments each time, use a max-heap where each entry stores:
  * [effectiveLength, segmentIndex]
- At each step:
  * Pop the largest segment.
  * Split it.
  * Push back the new effective segment length.

**Steps:**
- Initialize PQ with initial segments.
- For k insertions:
  * Pop max segment.
  * Increase its count (howMany).
  * Compute new effective length.
  * Push back into PQ.
- Answer = PQ.peek().effectiveLength.

**Java Code:**
```java
public double betterPQ(int[] arr, int k) {
    int n = arr.length;
    int[] howMany = new int[n - 1];

    PriorityQueue<double[]> pq = new PriorityQueue<>(
        (a, b) -> Double.compare(b[0], a[0])
    );

    for (int i = 0; i < n - 1; i++) {
        double dist = arr[i + 1] - arr[i];
        pq.offer(new double[]{dist, i});
    }

    for (int gas = 1; gas <= k; gas++) {
        double[] top = pq.poll();
        int index = (int) top[1];

        howMany[index]++;

        double initialDist = arr[index + 1] - arr[index];
        double newLen = initialDist / (howMany[index] + 1);

        pq.offer(new double[]{newLen, index});
    }

    return pq.peek()[0];
}
```

**💭 Intuition Behind the Approach:**
- The brute approach scanned all segments,
- PQ keeps track of only the largest segment efficiently.

**Complexity (Time & Space):**
- omplexity
- Time:
  * O(k log n)
- Space:
  * O(n)
- Still too slow for k = 1e5 in Java.

### Approach 3: Optimal (Binary Search on Answer)

**Idea:**
- Binary search the smallest possible dist.
- For a given trial mid = dist,
- calculate how many gas stations are needed to ensure:
  * no adjacent distance > mid
- For each gap:
  * needed = floor(gap / mid)
  * if (gap % mid == 0) needed -= 1
- If:
  * totalNeeded <= k → dist is possible → try smaller
  * else → too small → increase dist

**Steps:**
- Set low = 0, high = max gap.
- While (high - low > 1e-6):
  * mid = (low + high)/2
  * calculate required stations
  * if <= k → move high = mid
  * else → move low = mid
- return high

**Java Code:**
```java
private int needed(double dist, int[] arr) {
    int cnt = 0;
    for (int i = 1; i < arr.length; i++) {
        double gap = arr[i] - arr[i - 1];
        int stations = (int)(gap / dist);
        if (Math.abs((gap / dist) - stations) < 1e-12)
            stations--;
        cnt += stations;
    }
    return cnt;
}

public double optimalBSOA(int[] arr, int k) {
    int n = arr.length;
    double low = 0, high = 0;

    for (int i = 0; i < n - 1; i++)
        high = Math.max(high, arr[i + 1] - arr[i]);

    while (high - low > 1e-6) {
        double mid = (low + high) / 2.0;
        int req = needed(mid, arr);

        if (req > k) low = mid;
        else high = mid;
    }

    return high;
}
```

**💭 Intuition Behind the Approach:**
- Larger dist → fewer stations needed → always possible
- Smaller dist → more stations needed → may exceed k
- This monotonic behavior → perfect for binary search.

**Complexity (Time & Space):**
- Time:
  * O(n log(1e6)) → ≈ O(n * 60)
- Space:
  * O(1)

---

## 6. Justification / Proof of Optimality

- Minimize Maximum = find the smallest X such that it is possible.
- Binary search on answer applies because:
  * If dist = X is possible → any dist > X is also possible
  * If dist = X is not possible → any dist < X is not possible
- This monotonic property makes BSOA valid.
---

## 7. Variants / Follow-Ups

- Aggressive Cows (maximize minimum distance)
- Split array into K subarrays minimizing maximum sum
- Minimize max packet size
- Painter’s partition problem
- Minimize max sweetness
---

## 8. Tips & Observations

- Always recognize “minimize maximum” → binary search over answer.
- For double precision, use:
  * while (high - low > 1e-6)
- PQ approach is still too slow for large k.
- Use accurate floating-point comparisons.
- No merging or simulation can beat BSOA.

- **⚠️ Pitfalls**
    - Incorrect formula for stations needed
    - Forgetting the precision threshold
    - Using integer binary search instead of double
    - Misinterpreting "minimize maximum" vs "maximize minimum"
    - Precision issues in dividing gaps
---

<!-- #endregion -->
