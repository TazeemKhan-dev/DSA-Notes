<!-- #region 0-Binary Search -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Binary Search</h1>

## 1. Problem Understanding

- Binary Search is an algorithm used to efficiently search for a value in a sorted array or in any monotonic search space.
- It works by repeatedly dividing the search interval into half.
- It applies to:
  * Searching for an element
  * Finding first/last occurrence
  * Finding boundaries
  * Solving questions where answer lies in a range
  * Questions where the condition behaves monotonically (true/false pattern)
- Binary search is NOT just for arrays — it's for searching on answers (Binary Search on Answer).
---

## 2. Constraints

- Array must be sorted (or monotonic condition must exist).
- Monotonic means the values move only in one direction (only increasing or only decreasing). Nothing zig-zag.
- Time limit often requires O(log n) or O(n log n) solutions.
- Values may be very large, but binary search handles it easily due to logarithmic complexity.

- **What is a Monotonic Array?**
    - An array is monotonic if it satisfies one of the following:
    - 1️⃣ Monotonic Increasing
      * Each element is greater than or equal to the previous.
        * Example:
          * 1 2 2 3 4 7 9
        * Here:
          * arr[i] <= arr[i+1]
    - 2️⃣ Monotonic Decreasing
      * Each element is less than or equal to the previous.
      * Example:
        * 9 7 7 4 3 1
      * Here:
        * arr[i] >= arr[i+1]
    - ✔️ Why Binary Search Needs a Monotonic Condition?
      * Binary search works only if we can discard half of the search space every time.
      * That is possible only if:
        * Values consistently increase OR
        * Values consistently decrease
        * So we know which direction contains the target.
        * If array is like:
          * 3 5 2 8 1
          * Not sorted
          * Not monotonic
          * Values go up and down → cannot apply binary search
---

## 3. Edge Cases

- These are the edge cases that break 80% of beginners' code:
  * mid = (l + r) / 2 → may overflow; use
  * mid = l + (r - l) / 2.
  * Infinite loop due to wrong update (like using l = mid instead of l = mid + 1).
  * Using wrong comparison (< vs <=).
  * Arrays with duplicate elements (must choose left/right boundary carefully).
  * l surpassing r (stop condition).
  * Off-by-one errors in returning the boundary.
  * When doing binary search on answers → ensure the predicate is monotonic.
---

## 4. Examples

```text
🧪 Examples (Simple)

Searching 7 in [1,3,4,7,9]
Sorted → can apply binary search.

Finding floor square root of 40 → answer lies in integer range [0..40].

Finding minimum capacity to ship packages → monotonic condition exists.
```

---

## 5. Approaches

### Approach 1: Classic Binary Search on Sorted Array

**Idea:**
- Search for exact element.

**Java Code:**
```java
public static int binarySearch(int[] arr, int target) {
    int l = 0, r = arr.length - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Each comparison eliminates half the array → logarithmic speed.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n) because we cut search space by half every iteration.
- 💾 Space Complexity
  * O(1) iterative.

### Approach 2: Lower Bound (first index ≥ target)

**Idea:**
- Find the first position where element is >= target.

**Java Code:**
```java
public static int lowerBound(int[] arr, int target) {
    int l = 0, r = arr.length - 1;
    int ans = arr.length;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (arr[mid] >= target) {
            ans = mid;
            r = mid - 1;
        } else {
            l = mid + 1;
        }
    }
    return ans;
}
```

### Approach 3: Upper Bound (first index > target)

**Java Code:**
```java
public static int upperBound(int[] arr, int target) {
    int l = 0, r = arr.length - 1;
    int ans = arr.length;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (arr[mid] > target) {
            ans = mid;
            r = mid - 1;
        } else {
            l = mid + 1;
        }
    }
    return ans;
}
```

### Approach 4: Search Insert Position

**Java Code:**
```java
public static int searchInsert(int[] arr, int target) {
    int l = 0, r = arr.length - 1;
    int ans = arr.length;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (arr[mid] >= target) {
            ans = mid;
            r = mid - 1;
        } else {
            l = mid + 1;
        }
    }
    return ans;
}
```

### Approach 5: Binary Search on Answer (VERY IMPORTANT)

**Idea:**
- Used in many DSA problems:
  * ✔ Allocate minimum pages
  * ✔ Aggressive cows
  * ✔ Koko eating bananas
  * ✔ Painters partition
  * ✔ Minimum capacity to ship packages
  * ✔ Find smallest divisor
  * ✔ Minimize maximum distance to gas station
  * ✔ etc.
- ✔ Idea
  * Guess an answer → check if it's valid → move left or right.
  * The key is the predicate must be monotonic:
  * If x works → all values > x might work
- Or vice versa

**Java Code:**
```java
public static int searchAnswer(int low, int high) {
    int ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (isPossible(mid)) {
            ans = mid;      // mid is valid → try left
            high = mid - 1;
        } else {
            low = mid + 1;  // mid invalid → go right
        }
    }
    return ans;
}
```

### Approach 6: Binary Search on Real Numbers

**Idea:**
- Used for:
  * Square root with precision
  * Koko eating bananas (if using double)
  * Gas station placement
- Key difference:
  * Loop until precision achieved instead of l <= r.

### Approach 7: Binary Search on Rotated Sorted Array

**Idea:**
- ✔ Search in rotated sorted array
- ✔ Find minimum in rotated sorted array
- Concept:
- Left half or right half is always sorted — determine where target lies.

### Approach 8: Binary Search with Duplicates

**Idea:**
- First occurrence
- Last occurrence
- Count occurrences using:
- last - first + 1
- Requires adjusting boundaries carefully.

---

## 6. Variants / Follow-Ups

- Floor
- Ceil
- Square root
- Cube root
- kth missing positive
- Peak element
- Allocate minimum pages
- Maximum average subarray (variant)
- Aggressive cows
- Minimum time to complete tasks
- Koko eating bananas
- Search in mountain array
---

## 7. Tips & Observations

- Always check monotonicity before applying binary search.
- For duplicates → use lower/upper bound logic.
- For rotated arrays → identify sorted half.
- For BS on answer → define the range properly.
- Watch for infinite loops by using mid = l + (r - l) / 2.
- Always test with edge cases: single element, all equal, target not found, target smaller/larger than all.

- **Complexity**
    - ⏱️ Time Complexity (Final)
      * All array-based BS → O(log n)
      * BS on answer → O(log(range))
    - 💾 Space Complexity (Final)
      * O(1) for iterative.
---

<!-- #endregion -->
