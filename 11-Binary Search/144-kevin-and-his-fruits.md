<!-- #region 144-Kevin And His Fruits -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q144: Kevin And His Fruits</h1>

## 1. Problem Understanding

- Kevin has N buckets, each containing some fruits.
- He chooses an integer marker.
- From each bucket, he can eat:
  * max(0, bucket[i] - marker)
- He wants to eat at least M fruits in total.
- Your task:
- 👉 Find the maximum value of marker such that total fruits eaten ≥ M.
---

## 2. Constraints

- 1 ≤ N ≤ 10^4
- 1 ≤ M ≤ 10^6
- 0 ≤ arr[i] ≤ 10^4
- Sum of all fruits ≥ M (so answer definitely exists)
- Output: maximum possible marker
---

## 3. Edge Cases

- All buckets have very small fruits → marker becomes small
- One huge bucket can alone contribute ≥ M
- Buckets with zero fruits
- Marker = 0 or Marker = max(bucket)
---

## 4. Examples

```text
Example 1

Input
4 30
10 40 30 20

Marker = 20
Eatable fruits = 0 + 20 + 10 + 0 = 30

Output: 20

Example 2

Input
4 16
5 8 20 1

Marker = 6
Eatable = 0 + 2 + 14 + 0 = 16

Output: 6
```

---

## 5. Approaches

### Approach 1: Brute Force (Check all possible marker values)

**Idea:**
- Try every marker from 0 to max(arr)
- For each marker:
  * compute total eaten fruits
  * check if ≥ M
- Pick the maximum valid marker.

**Steps:**
- Let maxVal = max(arr)
- Loop marker from 0 to maxVal
- Calculate:
  * sum += max(0, arr[i] - marker)
- If sum ≥ M → store this marker
- Return the maximum marker

**Java Code:**
```java
public int maxMarkerBrute(int[] arr, int M) {
    int maxVal = 0;
    for (int x : arr) maxVal = Math.max(maxVal, x);

    int ans = 0;

    for (int marker = 0; marker <= maxVal; marker++) {
        int sum = 0;
        for (int x : arr) {
            if (x > marker) sum += x - marker;
        }
        if (sum >= M) ans = marker;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We test each possible marker and check what happens.
- Very slow but conceptually simple.

**Complexity (Time & Space):**
- Time: O(N * max(arr))
- Each marker requires scanning all buckets
- Space: O(1)

### Approach 2: Binary Search on Answer

**Idea:**
- As marker increases:
- Eatable fruits decrease.
- This forms a monotonic decreasing function, meaning:
  * marker ↑ → total_eaten ↓
- Thus, we can binary search for the largest marker that still gives eaten ≥ M.

**Steps:**
- Set search space:
  * low = 0
  * high = max(arr)
- For a mid = marker:
  * compute total = Σ max(0, arr[i] - mid)
- If total ≥ M:
  * marker is valid → move right (try bigger marker)
- Else:
  * marker too big → move left
- Return the maximum valid marker

**Java Code:**
```java
public int maxMarker(int[] arr, int M) {
    int maxVal = 0;
    for (int x : arr) maxVal = Math.max(maxVal, x);

    int low = 0, high = maxVal, ans = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        long total = 0;
        for (int x : arr) {
            if (x > mid) total += x - mid;
        }

        if (total >= M) {
            ans = mid;        // mid is valid
            low = mid + 1;    // try for bigger marker
        } else {
            high = mid - 1;   // mid is too large
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- When the marker is small:
  * Kevin eats many fruits because most buckets have arr[i] - marker > 0.
- When the marker is medium:
  * Kevin eats a moderate number of fruits.
- When the marker is large:
  * Kevin eats very few fruits, because only buckets with high values contribute.
- As the marker increases:
  * Total eaten fruits keep decreasing.
  * This forms a monotonically decreasing pattern.
- Since the function (marker → total eaten fruits) is monotonic:
  * We can apply binary search to find the largest marker for which the total eaten fruits is still ≥ M.

**Complexity (Time & Space):**
- Time: O(N * log(max(arr)))
  * Each mid requires scanning all N buckets
  * log(max(arr)) ≤ log(10000) ≈ 14
- Space: O(1)
  * Why time is O(N log(max))?
    * Because binary search checks ~14 possible markers, each requiring an O(N) scan.

---

## 6. Justification / Proof of Optimality

- Binary search on marker is the standard pattern for problems like:
- Cutting trees
- Collecting wood
- Minimizing/maximizing a threshold
- Allocation tasks
- The function is monotonic → binary search is the optimal tool.
---

## 7. Variants / Follow-Ups

- Minimum saw height to cut M wood
- Allocate minimum pages (binary search on answer)
- Gas stations distance minimization
- Aggressive cows
- Koko eating bananas
---

## 8. Tips & Observations

- When target is “maximize marker,” we use:
  * if (valid) low = mid + 1
- Think monotonic: if mid works, everything smaller also works.
- Use long for sums to avoid overflow.
---

<!-- #endregion -->
