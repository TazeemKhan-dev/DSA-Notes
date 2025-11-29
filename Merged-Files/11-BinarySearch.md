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
<!-- #region 1-core conceptual difference between the 2 binary-search loop -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q1: core conceptual difference between the 2 binary-search loop</h1>

## 1. Problem Understanding

- You want to understand the core conceptual difference between the 2 binary-search loop styles:
  * while (low < high)
  * while (low <= high)
- The real question you had:
- “When there’s only 1 element left, aren’t ‘element’ and ‘candidate’ the same?
- Why do we sometimes STOP, and sometimes CHECK that single element?”
- This is exactly where your confusion was.
---

## 2. Constraints

- These rules apply to all binary-search problems, including:
  * searching for a target
  * finding a minimum/maximum
  * finding pivot/peak
  * BS on answer problems (aggressive cows, painters partition etc.)
  * rotated sorted array problems
- You must choose the correct loop condition depending on the nature of the problem.
---

## 3. Edge Cases

- You must understand these cases:
  * When the last remaining index is always the answer
  * When the last remaining index might OR might not be the answer
  * When you need to examine the last element
  * When you only need to narrow down to the last element
  * These differences define which loop condition is correct.
---

## 4. Examples

```text
I will use two simple examples later in the notes to illustrate the difference:

Find minimum in rotated array

Search for a target in sorted or rotated array

These will prove why (low < high) vs (low <= high) matter.
```

---

## 5. Approaches

### Approach 1: (low < high): Range Shrinking (Binary Search on Answer)

**Idea:**
- You are NOT looking for a specific element/value at mid.
- You are just trying to shrink the search space until only ONE possible index remains.
- Once only one index remains, that index must be the answer.
- In simple language
  * "The answer will reveal itself if I keep reducing the range."

**Steps:**
- You STOP when:
  * low == high
- Because at that moment:
- You have EXACTLY ONE candidate
- That candidate is guaranteed to be the correct answer
- You DON’T need to check it manually

**Java Code:**
```java
while (low < high) {
    int mid = (low + high) / 2;

    if (condition mid belongs right half)
        low = mid + 1;
    else
        high = mid;
}

return nums[low];
```

**💭 Intuition Behind the Approach:**
- You aren’t searching for equality
- You don’t need to inspect the final element
- The loop narrows to THE answer
- The logic itself ensures the final index is correct
- Where used
  * Find minimum in rotated array
  * Find peak
  * BS on answer (aggressive cows, allocate books)
  * Find first bad version (monotonic boolean)

**Complexity (Time & Space):**
- Time → O(log N)
- Space → O(1)

### Approach 2: (low <= high): Classic Search (Search for a Target or Condition)

**Idea:**
- Here you ARE looking for a specific value or satisfying a condition.
- Meaning:
  * The last element might be your answer
  * You MUST check the final element
  * The loop stops only when every index has been considered
- In simple language
  * "Even if ONE index remains, I must CHECK it, because it might or might not be my answer."
- You STOP when:
  * low > high

**Java Code:**
```java
while (low <= high) {
    int mid = (low + high) / 2;

    if (nums[mid] == target) return mid;

    if (nums[mid] < target)
        low = mid + 1;
    else
        high = mid - 1;
}

return -1;
```

**💭 Intuition Behind the Approach:**
- You might find the answer at ANY position
- Even the last remaining element must be tested
- You cannot stop early
- You need to use equality checks (==)
- Where used
  * Binary search to find a number
  * Search in rotated array
  * Lower bound / upper bound
  * First/last occurrence
  * Segment search problems

**Complexity (Time & Space):**
- Time → O(log N)
- Space → O(1)

---

## 6. Justification / Proof of Optimality

- Why both loops seem similar (they both test mid)
- But they are not solving the same TYPE of problem.
- The two questions clarify this:
- 🟢 Key Question 1: “Do I want to STOP when only 1 element is left?”
  * Use (low < high)
  * Because that 1 element is guaranteed to be the answer.
  * You stop without checking it.
- 🔵 Key Question 2: “Even when ONE element is left, MUST I CHECK it?”
  * Use (low <= high)
  * Because that last element might or might not be the answer.
  * You must verify it.
---

## 7. Variants / Follow-Ups

- These loop conditions appear in:
  * search for min/max
  * rotated array min
  * rotated array search
  * peak element search
  * function optimization
  * BS on answer problems
  * target index search
---

## 8. Tips & Observations

- Both loops (low < high and low <= high) DO check the mid.
- But they are used for COMPLETELY DIFFERENT GOALS.
- So the question is NOT:
- “Am I traversing mid or not?”
- You ALWAYS traverse mid.
- The REAL question is:
- “Do I want to STOP when only 1 element is left, or do I want to CHECK that 1 element too?”
- This changes EVERYTHING.
- ✔ Case 1: (low < high)
  * → "I want the loop to end when only ONE candidate is left."
  * Purpose: REDUCE the range.
  * You are NOT searching for an exact match.
  * Example
    * find minimum
    * find peak
    * find first bad version (binary reduce)
    * maximize or minimize value using BS (BS on answer)
    * Here the algorithm guarantees:
    * There is exactly one answer
    * When we narrow to that one index → DONE
  * That's why we stop when:
  * low == high
- We DO NOT need to check mid for equality.
- ✔ Case 2: (low <= high)
  * → "Even when ONE element is left, I MUST CHECK it."
  * Purpose: SEARCH for a target
  * or search for a condition.
  * Examples:
    * search element in array
    * search pivot index
    * search lower bound
    * search upper bound
    * search element in rotated array
    * Here the answer may be anywhere, and even the LAST remaining element must be checked.
    * So we stop only when:
    * low > high
  * Because the last remaining element might be the answer.

- **"When 1 element left, element and candidate are same, so why difference?"**
    - ✔ “One candidate left” means
      * → The algorithm has mathematically proven that this is the answer.
    - ✔ “One element left” means
      * → It is just the last element in the range.
      * → It may or may not be the answer.
      * → Equality must be checked.
    - They only LOOK the same, but the meaning is completely different.

- **When you think “candidate = element”, think this:**
    - In (low < high) →
      * The algorithm GUARANTEES the last index is correct.
    - In (low <= high) →
      * The last index is simply the only one left untested.

- **Stopping and checking are DIFFERENT operations.**
    - (low < high) → you stop and declare the answer
    - (low <= high) → you inspect the last element

- **FINAL SUMMARY YOU MUST MEMORIZE**
    - 🔥 Use low < high ONLY IF:
      * The algorithm doesn't check mid for equality
      * You are shrinking the space
      * The last index is automatically the answer
    - 🔥 Use low <= high ONLY IF:
      * You might find your answer ANYWHERE
      * You need to check the last element
      * You rely on ==, >=, <= conditions
---

<!-- #endregion -->
<!-- #region 2-Binary Search on Answer Fundamentals  -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q2: Binary Search on Answer Fundamentals</h1>

## 1. Problem Understanding

- Binary Search on Answer (BSOA) is a pattern used when:
  * You are not searching for an element inside an array
  * You are searching for the best (minimum or maximum) valid answer
  * You can convert the problem into a YES/NO function
  * That YES/NO result behaves monotonically
- Monotonic means:
  * All false then all true → 0 0 0 1 1 1
  * OR
  * All true then all false → 1 1 1 0 0 0
- Examples:
  * Minimum speed to finish bananas
  * Maximum minimum distance (aggressive cows)
  * Minimum capacity to ship packages
  * Minimum days to make bouquets
  * Minimum pages per student
  * Minimum time to paint boards
---

## 2. Constraints

- These fundamentals apply when:
  * The answer lies in a numeric range
  * That numeric range is bounded, e.g.
    * 1 → max(arr)
    * max(weights) → sum(weights)
  * You can design an isPossible(mid) function
  * isPossible(mid) runs in O(N) or better
  * Answer validity is monotonic
---

## 3. Edge Cases

- Search space must always be valid
- Lower bound must never be illogical (e.g., 0 speed is invalid)
- If problem asks to minimize, the first true is the answer
- If problem asks to maximize, the last true is the answer
- Always validate:
  * largest value in array
  * sum of elements (for capacity problems)
  * smallest possible answer = 1 (for speed/distance problems)
---

## 4. Examples

```text
Example 1 – Koko Eats Bananas

Search space = speed
isPossible(speed) → can Koko finish in H hours?

Monotonic:

0 0 0 1 1 1

Example 2 – Aggressive Cows

Search space = minimum distance
isPossible(d) → can cows be placed with ≥ d spacing?

Monotonic:

0 0 1 1 1

Example 3 – Minimum Capacity to Ship Packages

Search space = capacity
isPossible(cap) → can ship in ≤ D days?

Monotonic:

1 1 1 0 0
```

---

## 5. Approaches

### Approach 1: Binary Search to Minimize the Answer (Most Common)

**Idea:**
- Find the smallest value of x such that isPossible(x) == true.
- Pattern:
- 0 0 0 1 1 1 1
      * ↑
   * answer

**Steps:**
- Define low, high search space
- While low <= high:
  * Compute mid
  * If possible(mid) → store mid, move left
  * Else → move right

**Java Code:**
```java
int low = start, high = end;
int ans = -1;

while (low <= high) {
    int mid = low + (high - low) / 2;

    if (isPossible(mid)) {
        ans = mid;
        high = mid - 1;  // try to minimize
    } else {
        low = mid + 1;
    }
}

return ans;
```

**💭 Intuition Behind the Approach:**
- You continuously shrink toward the smallest valid number.
- Once you find a valid candidate, you attempt a better (smaller) one.

### Approach 2: Binary Search to Maximize the Answer

**Idea:**
- Find the largest value of x such that isPossible(x) == true.

**Steps:**
- If possible(mid) → keep this value, try for bigger
- Else → reduce range

**Java Code:**
```java
int low = start, high = end;
int ans = -1;

while (low <= high) {
    int mid = low + (high - low) / 2;

    if (isPossible(mid)) {
        ans = mid;
        low = mid + 1;  // try to maximize
    } else {
        high = mid - 1;
    }
}

return ans;
```

**💭 Intuition Behind the Approach:**
- You try to stretch the answer to its maximum limit while staying valid.

---

## 6. Justification / Proof of Optimality

- Binary Search on Answer is correct because:
  * The answer lies in a monotonic numeric space
  * isPossible(x) cleanly divides that space into two halves:
    * Valid answers
    * Invalid answers
  * This behavior allows binary search to converge to the boundary
  * Time complexity becomes:
    * Checking mid → O(N)
    * Binary search iterations → O(log M)
    * Combined → O(N log M)
---

## 7. Variants / Follow-Ups

- Minimize max distance
- Maximize min distance
- Minimize max pages
- Minimum speed problems
- Minimum number of days
- Minimum capacity
- Minimum time to finish tasks
- Feasibility check (YES/NO) driving the logic
---

## 8. Tips & Observations

- ✔ 1. The array has NOTHING to do with the binary search
  * Binary Search is on answer, not array values.
- ✔ 2. Always ensure monotonicity
  * isPossible() must give results like:
    * 0 0 0 1 1 1
    * or
    * 1 1 1 0 0 0
- ✔ 3. Use (low <= high) ALWAYS in BSOA
  * You must evaluate the final candidate.
- ✔ 4. Boundaries are EVERYTHING
  * Correct low and high = 50% of the problem solved.
- ✔ 5. Minimize → move left
  * Maximize → move right
- ✔ 6. isPossible() must be efficient
  * Typically O(N).
- ✔ 7. Never forget to store answer
  * Every time mid is valid:
  * ans = mid;
- ✔ 8. Practice problems strengthen monotonic intuition
  * Aggressive Cows and Allocate Books are the best starters.

- **Complexity**
    - ⏱ Time Complexity
      * Each check: O(N)
      * Binary search: O(log M)
      * Final: O(N log M)
      * M = range of possible answers
    - 💾 Space Complexity
      * O(1) (no extra structures)
---

<!-- #endregion -->
<!-- #region 3-The answer (target) lies between 0 to ∞ -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q3: The answer (target) lies between 0 to ∞</h1>

## 1. Problem Understanding

- Some binary search problems have unbounded search space, meaning:
  * You do NOT know the upper limit (high)
  * The answer lies somewhere in [0, ∞) or [1, ∞)
- Example:
  * Find first value greater than target in an infinite sorted array
  * Find square root (answer grows unbounded w.r.t input)
  * Find minimum speed to eat bananas
  * Find minimum time to paint boards
  * Find smallest x that satisfies a monotonic condition
- We solve this using Exponential Search + Binary Search.
---

## 2. Constraints

- Search space is unbounded
- Answer is guaranteed to exist
- isPossible(x) must be monotonic
  * (true → always true OR false → always false afterward)
- Must work in O(log answer) or O(log range) time
---

## 3. Edge Cases

- Answer is very small (0 or 1)
- Extremely large target (like 10^18)
- Condition can overflow → use long
- Monotonicity must hold
- Infinite loop possible if bounds not chosen properly
---

## 4. Examples

```text
“Find first value ≥ target in infinite sorted array”

“Find minimum x so that f(x) ≥ k”

“Find max/min using binary search without known upper bound”

“Root finding”

“Minimum speed to finish tasks”
```

---

## 5. Approaches

### Approach 1: Exponential Expansion + Binary Search (Universal Template)

**Idea:**
- ✔ Works when the answer ∈ [0, ∞)
- ✔ No upper bound known
- ✔ Perfect for BSOA (Binary Search on Answer)

**Steps:**
- Start with a small range
- Double the high boundary until you exceed the condition
- Then apply binary search in the bounded region
- STEP 1 — Find upper bound using exponential expansion
  * low = 0
  * high = 1
  * while (isPossible(high) == false)
      * low = high
      * high = high * 2
  * This ensures:
    * low is invalid
    * high is valid
    * This forms the typical monotonic window for binary search.
- STEP 2 — Perform classic binary search
  * Now we know answer lies in [low, high]
    * while (low <= high):
        * mid = (low + high)/2
        * if (isPossible(mid)):
            * ans = mid
            * high = mid - 1
        * else:
            * low = mid + 1
    * Return ans.

**Java Code:**
```java
public long findAnswer() {

    long low = 0;
    long high = 1;

    // 1. Expand until we find a valid high bound
    while (!isPossible(high)) {
        low = high;
        high = high * 2;  // Exponential growth
    }

    long ans = high;

    // 2. Now binary search in [low, high]
    while (low <= high) {
        long mid = low + (high - low) / 2;

        if (isPossible(mid)) {
            ans = mid;      // mid is a valid answer
            high = mid - 1; // try to minimize it
        } else {
            low = mid + 1;
        }
    }

    return ans;
}

// Replace with problem-specific logic
boolean isPossible(long x) {
    // condition check
    return true;
}
```

**💭 Intuition Behind the Approach:**
- When you don't know the upper bound, start small
- Keep doubling until you cross the point where the condition becomes true
- This creates a valid search range
- Then binary search works perfectly
- Used in 100+ real problems (range search)

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Exponential expansion: O(log answer)
  * Binary search: O(log answer)
  * Overall:
    * O(log answer)
- 💾 Space Complexity
  * O(1)

---

## 6. Variants / Follow-Ups

- This technique can be used for:
- Find ceil/floor in infinite array
- Minimum time problems
- Maximum distance problems
- Load balancing
- Machine scheduling problems
- Square root (but sqrt usually uses bounded search)
---

## 7. Tips & Observations

- ALWAYS check monotonicity
- Use long for mid * mid type operations
- Exponential expansion ensures O(log answer) complexity
- Works even for answer up to 10^18
- For minimizing → move high
- For maximizing → move low
- Always update answer when condition becomes true
---

<!-- #endregion -->
<!-- #region 04- Binary Search on Answer Fundamentals-2 -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q04: Binary Search on Answer Fundamentals-2</h1>

## 1. Problem Understanding

- Binary Search on Answer (BSOA) is used when:
- You must minimize or maximize an integer answer
- The input isn't sorted, but the answer space is monotonic
- You can build a function isPossible(x) that answers YES/NO
- This pattern is the backbone of many "optimization + constraint" problems.
---

## 2. Constraints

- Answer must lie in a range: lower_bound → upper_bound
- isPossible(mid) must be monotonic (if true at mid, true for all higher or lower mid)
- Condition must be checkable in polynomial time (often O(n))
---

## 3. Edge Cases

- Wrong bounds (lower bound not starting at 1)
- Overflow in multiplication or power
- Wrong monotonic direction (maximize vs minimize)
- Wrong feasibility function
- Answer=0 edge case (impossible for divisors, coco, etc.)
- m*k > n edge case (bouquets)
---

## 4. Examples

```text
Minimize k such that sum(ceil(arr[i]/k)) <= limit
→ Smallest Divisor

Minimize days such that we can form m bouquets
→ Minimum Days to Make Bouquets

Find maximum minimum distance such that cows can be placed
→ Aggressive Cows

Minimize eating speed k such that Koko finishes in h hours
→ Koko Eating Bananas

Minimize maximum pages student reads
→ Allocate Books
```

---

## 5. Approaches

### Approach 1: Brute Force (For Generic BSOA)

**Idea:**
- Try all possible answers (k or day or speed) and check if they satisfy condition.

**Steps:**
- Loop from L → R
- For each mid, run feasibility check
- Return first that works (minimize) or last that works (maximize)

**Java Code:**
```java
for (int x = low; x <= high; x++) {
    if (isPossible(x)) return x;
}
```

**💭 Intuition Behind the Approach:**
- Works but extremely slow.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O((range of answer) * cost_of_isPossible)
  * Often → TLE
- 💾 Space Complexity
  * O(1)

### Approach 2: Binary Search on Answer (l <= h)

**Idea:**
- Use **binary search on the answer space**, not on the array itself.
- Let `mid` be the **candidate answer**.
- Use a helper function to check if this `mid` value is **valid/possible**.
- If `mid` is valid:
  - store it as the **current best answer** (`ans = mid`)
  - move **left** to try finding a smaller valid answer.
- If `mid` is not valid:
  - move **right** to search for a larger possible answer.

**Steps:**
- low = L
- high = R
- while low <= high:
  * mid = ...
  * if possible(mid):
    * ans = mid
    * high = mid – 1
  * else low = mid + 1
- return ans

**Java Code:**
```java
int ans = -1;
while (low <= high) {
    int mid = low + (high - low) / 2;
    if (isPossible(mid)) {
        ans = mid;
        high = mid - 1;
    } else {
        low = mid + 1;
    }
}
return ans;
```

**💭 Intuition Behind the Approach:**
- Explicitly stores best possible mid.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log(range) * cost_of_isPossible)
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Binary Search on Answer works because:
- The answer space is sorted by validity
- Feasibility forms a monotonic curve
- For minimization:
  * F F F F T T T T
  * → find first T
- For maximization:
  * T T T T F F F F
  * → find last T
- Binary search excels in finding this boundary.
---

## 7. Variants / Follow-Ups

- ✔ Classic BSOA Problems
  * Smallest Divisor
  * Koko Eating Bananas
  * Minimum Days to Make M Bouquets
  * Aggressive Cows
  * Allocate Minimum Number of Pages
  * Split Array Largest Sum
  * Painter Partition Problem
  * Binary Search on continuous values (sqrt, cbrt)
- ✔ Variants Where Monotonic Behavior Is Tricky
  * Nth Root
  * Minimize maximum gas station distance
  * Minimize time to repair cars
---

## 8. Tips & Observations

- Focus on search space, NOT input array
- Build a solid isPossible(mid)
- Always ask: Should mid move left or right?
- Identify if you're doing minimization or maximization
- low = 1 is often the right boundary, NOT 0
- Be careful with overflow in feasibility checks
- For minimizing something, use (l < h)
- For storing best solution, use (l <= h)

- **🕳️ Pitfalls**
    - Misidentifying search space
    - Using wrong monotonic direction
    - Binary searching the array instead of the answer
    - Overflow in (mid * something)
    - Starting low = 0 where answer must be ≥1
    - Forgetting to sort (Aggressive Cows, Allocate Books)
    - Incorrect feasibility logic
    - Returning wrong boundary (low vs low-1)

- **🧩 LEVEL 1 — Beginner**
    - (Recognize simple monotonic functions)
      * Square Root (Binary Search)
      * Nth Root
      * Smallest Divisor
      * Koko Eating Bananas

- **🧠 LEVEL 2 — Intermediate**
    - (Monotonic with arrays)
      * Minimum Days to Make Bouquets
      * Aggressive Cows
      * Magnetic Force Between Balls

- **🚀 LEVEL 3 — Advanced**
    - (Partition + minimization/maximization)
      * Allocate Books
      * Painter Partition
      * Split Array Largest Sum

- **🧨 LEVEL 4 — Master**
    - (Complex feasibility checks)
      * Minimize maximum distance to gas stations
      * Min time to repair cars
      * Min speed to arrive on time
---

<!-- #endregion -->
<!-- #region 135-Find Minimum in Rotated Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q135: Find Minimum in Rotated Sorted Array</h1>

## 1. Problem Understanding

- You are given a sorted array that has been rotated between 1 and n times.
- Examples:
- Original: [1,2,3,4,5]
- Rotated: [3,4,5,1,2]
- Your task:
- Find the minimum element in the rotated array.
- Must run in O(log n) → Binary Search.
---

## 2. Constraints

- 1 <= n <= 5000
- Numbers can be negative.
- All elements are unique
- Must use binary search → no linear scan.
---

## 3. Edge Cases

- Array not rotated → minimum at index 0.
- Example: [1,2,3,4]
- Rotation by n times also gives original array.
- Minimum might be at the boundaries.
- Example: [2,3,4,5,1]
- Very small arrays: size 1, size 2.
- Already sorted means return nums[0].
---

## 4. Examples

```text
Example 1

Input:

[3,4,5,1,2]


Output:

1

Example 2

Input:

[4,5,6,7,0,1,2]


Output:

0
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Scan)

**Idea:**
- The minimum of any array is simply the smallest element.
- Directly scan the array.

**💭 Intuition Behind the Approach:**
- A sorted rotated array still contains a minimum somewhere.
- You inspect all elements to find it.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Because scanning all elements.
- 💾 Space Complexity
  * O(1)
  * Only one variable used.

### Approach 2: Slightly Better (Check Adjacent Break Point)

**Idea:**
- In a rotated array, the minimum occurs at the break point where:
- nums[i] < nums[i-1]
- Scan and check only such break.

**Java Code:**
```java
public static int findMin(int[] nums) {
    int n = nums.length;
    for (int i = 1; i < n; i++) {
        if (nums[i] < nums[i - 1]) return nums[i];
    }
    return nums[0];
}
```

**💭 Intuition Behind the Approach:**
- The array is sorted except one pivot point.
- Minimum sits right after the point where order breaks.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) worst case
  * If array not rotated.
- 💾 Space Complexity
  * O(1)

### Approach 3: Binary Search — O(log n)

**Idea:**
- Use binary search to find the pivot where rotation happened.
- Observations:
  * If nums[mid] > nums[right], the minimum is to the right.
  * Else → minimum is to the left or at mid.

**Steps:**
- Set l = 0, r = n-1.
- While l < r:
  * Compute mid.
  * If nums[mid] > nums[r] → search right half.
  * Else → search left half (including mid).
- At the end l = index of minimum.

**Java Code:**
```java
public static int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;

    while (l < r) {
        int mid = l + (r - l) / 2;

        if (nums[mid] > nums[r]) {
            l = mid + 1;     // min is to the right
        } else {
            r = mid;         // min is at mid or left side
        }
    }

    return nums[l]; // l is the index of minimum
}
```

**💭 Intuition Behind the Approach:**
- When nums[mid] > nums[r], right half is unsorted → min lies there.
- When nums[mid] <= nums[r], right half is sorted → min cannot be in sorted region except possibly mid.
- Binary search narrows search space towards the pivot point.
- This is a classic Binary Search on answer space, not on presence.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Because each iteration halves the search space.
- 💾 Space Complexity
  * O(1)
  * Only two pointers.

---

## 6. Justification / Proof of Optimality

- The array is partially sorted.
- Binary search is the perfect fit when:
  * There is a monotonic pattern.
  * There is a pivot.
  * We can eliminate half of the array on each decision.
- The condition nums[mid] > nums[r] is the key that tells us which side the pivot lies on.
---

## 7. Variants / Follow-Ups

- Find index of minimum instead of value.
- Find number of times rotated → index of min is the rotation count.
- Rotated array with duplicates → trickier binary search.
- Search element in rotated sorted array.
---

## 8. Tips & Observations

- Always compare with the rightmost element for easier logic.
- If whole array is already sorted → return nums[0].
- If duplicates exist:
  * Need to shrink boundaries carefully.
- Rotated sorted array problems ALWAYS revolve around:
  * Detecting pivot
  * Searching on monotonic segments
---

<!-- #endregion -->
<!-- #region 136-Square root of a number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q136: Square root of a number</h1>

## 1. Problem Understanding

- You are given a number x.
- You must return:
- If x is a perfect square → its exact square root
- Otherwise → floor(√x)
- You must not use the built-in sqrt() function.
- Time complexity must be O(log N), which hints binary search.
---

## 2. Constraints

- 1 ≤ x ≤ 10^7
- Input is a single integer
- Output must be an integer (floor value)
---

## 3. Edge Cases

- x = 1 → answer = 1
- Very small values: x = 2, 3 → answer = 1
- Large inputs up to 10^7
- Perfect squares: 4, 9, 16, 25 → return exact root
- Overflow of mid * mid → use long to prevent overflow
---

## 4. Examples

```text
Example 1
Input: 5
Output: 2

Example 2
Input: 36
Output: 6

Example 3
Input: 2
Output: 1
```

---

## 5. Approaches

### Approach 1: Brute Force Approach (Linear Search)

**Idea:**
- Try every number from 1 to x and check the last i such that i*i ≤ x.

**Steps:**
- Loop from i=1 to i*i ≤ x.
- Track the last valid number.
- Return it.

**Java Code:**
```java
public static int mySqrt(int x) {
    int ans = 0;
    for (long i = 1; i * i <= x; i++) {
        ans = (int) i;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Directly simulate what square root means.
- Simple but slow.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(√N)
  * Because loop runs until i*i ≤ x.
- 💾 Space Complexity
  * O(1)
  * Only counters used.

### Approach 2: Using Math Properties (Decreasing Search Range)

**Idea:**
- Instead of checking all numbers from 1 to x, we can optimize by stopping early once i*i crosses x.
- (Slightly better than brute but still worst-case √N)

**Steps:**
- Same as brute but break early.

**Java Code:**
```java
public static int mySqrt(int x) {
    long i = 1;
    while (i * i <= x) i++;
    return (int)(i - 1);
}
```

**💭 Intuition Behind the Approach:**
- Stop as soon as you've gone past the root.
- Still linear in √N but less work than brute.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(√N)
- 💾 Space Complexity
  * O(1)

### Approach 3: Binary Search (Required O(log N))

**Idea:**
- The real square root lies between 1 and x.
- Binary search this space for the greatest mid such that:
- mid * mid ≤ x

**Steps:**
- Let low = 1, high = x
- Compute mid
- If mid*mid ≤ x, store it and move right (search for bigger)
- Else move left
- Return last stored answer

**Java Code:**
```java
public static int mySqrt(int x) {
    if (x == 0) return 0;

    long s = 1;
    long e = x;
    long ans = 1;

    while (s <= e) {
        long mid = (s + e) / 2;

        if (mid * mid <= x) {
            ans = mid;      // mid is valid
            s = mid + 1;    // try for bigger
        } else {
            e = mid - 1;    // mid too big
        }
    }
    return (int) ans;
}
```

**💭 Intuition Behind the Approach:**
- Square root grows slowly, so binary search reduces the search space by half every step.
- We want the largest number whose square is ≤ x → therefore move right on valid mid.
- This ensures correctness for both perfect and non-perfect squares.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log N)
  * Because binary search repeatedly halves the range.
- 💾 Space Complexity
  * O(1)
  * Only variables used.

---

## 6. Justification / Proof of Optimality

- Binary search is optimal because the search range is monotonic (i*i increases as i increases).
- Unlike brute force (√N steps), binary search does it extremely fast in log N steps.
- Works well for values up to 10^7 easily within constraints.
---

## 7. Variants / Follow-Ups

- Calculate ceil of √x
- Return true/false if x is a perfect square
- Return √x without using multiplication (Newton’s Method)
- √x for long or big integers
---

## 8. Tips & Observations

- Always use long for mid * mid to avoid integer overflow.
- When searching for largest valid, use:
  * if (mid*mid <= x) → move right
- For a perfect square, binary search will naturally find it.
- Classic question in binary search category — must be mastered.
---

<!-- #endregion -->
<!-- #region 137-Search A 2D Matrix -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q137: Search A 2D Matrix</h1>

## 1. Problem Understanding

- You are given a matrix where:
  * Each row is sorted from left → right.
  * The first element of each row is greater than the last element of previous row.
  * This means the entire matrix behaves like one sorted array.
- Goal:
  * Search if x exists.
  * Return true / false.
---

## 2. Constraints

- 1 <= m, n <= 1000
- m * n ≤ 10^6 → binary search is perfect.
- Values range from -10^4 to 10^4.
---

## 3. Edge Cases

- x smaller than smallest element → false
- x larger than largest element → false
- 1×1 matrix
- Single row or single column
- Large matrix → must use O(log(m*n))
---

## 4. Examples

```text
Example 1
Matrix:
1  3  5  7
10 11 16 20
23 30 34 60
x = 10 → true

Example 2
x = 12 → false
```

---

## 5. Approaches

### Approach 1: Brute Force (Check Every Element)

**Idea:**
- Scan all rows and all columns until element is found.

**Java Code:**
```java
public static boolean searchMatrix(int[][] mat, int x) {
    int m = mat.length, n = mat[0].length;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (mat[i][j] == x) return true;
        }
    }
    return false;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(m*n) — checks every cell
  * Why: No skipping possible.
- 💾 Space Complexity
  * O(1) — no extra space

### Approach 2: Row Selection + Binary Search (Better)

**Idea:**
- Identify which row x could be in:
  * Check first/last elements of each row.
- Binary search that row.

**Java Code:**
```java
public static boolean searchMatrix(int[][] mat, int x) {
    int m = mat.length, n = mat[0].length;

    for (int i = 0; i < m; i++) {
        if (x >= mat[i][0] && x <= mat[i][n-1]) {
            int l = 0, r = n-1;
            while (l <= r) {
                int mid = l + (r - l) / 2;
                if (mat[i][mid] == x) return true;
                else if (mat[i][mid] < x) l = mid + 1;
                else r = mid - 1;
            }
            return false;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Each row is sorted, so we can binary search inside the correct row.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Worst-case: O(m + log n)
  * Why: Scan rows → then binary search inside one row.
- 💾 Space Complexity
  * O(1)

### Approach 3: Treat as Sorted 1D Array (Optimal — O(log(m*n)))

**Idea:**
- Because:
  * Last element of row i < first element of row i+1
  * Matrix behaves like a single sorted array of length m*n.
- Map 1D index to matrix index:
  * row = mid / n
  * col = mid % n
- Binary search on 1D indexed search space.

**Java Code:**
```java
public static boolean searchMatrix(int[][] mat, int x) {
    int m = mat.length, n = mat[0].length;

    int l = 0, r = m * n - 1;

    while (l <= r) {
        int mid = l + (r - l) / 2;

        int row = mid / n;
        int col = mid % n;

        if (mat[row][col] == x) return true;
        else if (mat[row][col] < x) l = mid + 1;
        else r = mid - 1;
    }

    return false;
}
```

**💭 Intuition Behind the Approach:**
- You’re performing binary search on a flattened version of the matrix.
- Because matrix is globally sorted, this works flawlessly.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log(m*n))
  * Why: Classic binary search on total number of elements.
- 💾 Space Complexity
  * O(1) — constant space.

---

## 6. Justification / Proof of Optimality

- Brute force is too slow for large m*n.
- Row-wise selection reduces search but still touches many rows.
- Flattened binary search guarantees minimal comparisons and utilizes full sorted property — best approach.
---

## 7. Variants / Follow-Ups

- Search in a matrix where each row AND column is sorted (different problem, different approach).
- Find the number of occurrences of x.
- Return position instead of boolean.
---

## 8. Tips & Observations

- Whenever a matrix has this row-start > previous-row-end pattern → treat like 1D sorted list.
- Binary search remains the strongest pattern if global monotonicity exists.
---

<!-- #endregion -->
<!-- #region 138-Find First and Last Position of Element in Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q138: Find First and Last Position of Element in Sorted Array</h1>

## 1. Problem Understanding

- Given a sorted (non-decreasing) array nums, find the first and last index of a given target.
- If target doesn’t appear, return [-1, -1].
- We must do it in O(log n), so binary search is required.
---

## 2. Constraints

- Array length: 0 to 1e5
- Values range: -1e9 to 1e9
- Array is sorted
- Must achieve O(log n)
---

## 3. Edge Cases

- n = 0 → return [-1, -1]
- Target is smaller than first element
- Target is greater than last element
- All elements are the same as target → output [0, n-1]
- Target appears once
- Target appears multiple times
---

## 4. Examples

```text
Example 1

Input

6 8
5 7 7 8 8 10


Output

3 4

Example 2

Input

6 6
5 7 7 8 8 10


Output

-1 -1
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Scan)

**Idea:**
- Iterate whole array once to find first occurrence and last occurrence.

**Steps:**
- Loop from left to right → find first match
- Loop from right to left → find last match

**Java Code:**
```java
public int[] searchRange(int[] nums, int target) {
    int first = -1, last = -1;

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == target) {
            first = i;
            break;
        }
    }

    for (int i = nums.length - 1; i >= 0; i--) {
        if (nums[i] == target) {
            last = i;
            break;
        }
    }

    return new int[]{first, last};
}
```

**💭 Intuition Behind the Approach:**
- We check the entire array because it’s unspecific where target may occur.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) because we scan the array twice.
- 💾 Space Complexity
  * O(1) because no extra space.

### Approach 2: Two Binary Searches (Optimal)

**Idea:**
- Use binary search twice:
  * once to find the first occurrence
  * once to find the last occurrence
- Because array is sorted, binary search ensures O(log n).

**Steps:**
- Steps
- Find first position
  * Normal binary search
  * When you find nums[mid] == target, move left to see if earlier occurrence exists
  * So set high = mid - 1
- Find last position
  * When nums[mid] == target, move right
  * So set low = mid + 1
- Return [first, last]

**Java Code:**
```java
public int[] searchRange(int[] nums, int target) {
    int first = findFirst(nums, target);
    int last = findLast(nums, target);
    return new int[]{first, last};
}

private int findFirst(int[] nums, int target) {
    int low = 0, high = nums.length - 1, ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (nums[mid] == target) {
            ans = mid;
            high = mid - 1; // go left
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}

private int findLast(int[] nums, int target) {
    int low = 0, high = nums.length - 1, ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (nums[mid] == target) {
            ans = mid;
            low = mid + 1; // go right
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Binary search normally stops when the target is found, but we modify it:
  * For first occurrence, we force the search to continue left until no earlier index contains the target.
  * For last occurrence, we force search right.
- This preserves the O(log n) speed but still finds boundaries.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Two binary searches → O(log n)
  * Why?
    * Each binary search halves the array repeatedly, so two searches still log n + log n = log n.
- 💾 Space Complexity
  * O(1)
  * Why?
    * Only variables used; no extra data structures.

---

## 6. Justification / Proof of Optimality

- Binary search ensures fastest possible runtime (O(log n)), and by slightly modifying conditions, we can locate the exact boundaries of the target.
---

## 7. Variants / Follow-Ups

- Find first occurrence only
- Find last occurrence only
- Count occurrences → last - first + 1
- Works for rotated sorted arrays with modifications
---

## 8. Tips & Observations

- Always prefer binary search if array is sorted and you need positions.
- Use the mid calculation trick:
- mid = low + (high - low) / 2 to avoid overflow.
- Searching for first → move left
- Searching for last → move right
---

<!-- #endregion -->
<!-- #region 139-Count 1 in sorted binary array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q139: Count 1 in sorted binary array</h1>

## 1. Problem Understanding

- You are given a binary, sorted in non-increasing order array:
  * 1 1 1 ... 1 0 0 0
- You must find the count of 1s.
- Because all 1s come first and all 0s later, the problem becomes finding the boundary between 1 → 0.
- Binary search helps find this boundary in O(log n).
---

## 2. Constraints

- 1 <= N <= 10^6
- Array contains only 0 and 1
- Array is sorted in non-increasing order
- Must be efficient (binary search recommended)
---

## 3. Edge Cases

- All 1s → count = N
- All 0s → count = 0
- Only one element
- Transition happens at index 0
- Transition happens at index N-1
---

## 4. Examples

```text
Example 1

Input

8
1 1 1 1 1 0 0 0


Output

5

Example 2

Input

4
1 1 1 1


Output

4
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Traverse from the left and count until you see the first 0.

**Steps:**
- Initialize count = 0
- Loop from left to right
- If element is 1, increment
- If element is 0, break immediately
- Return count

**Java Code:**
```java
public int countOnes(int[] arr) {
    int count = 0;
    for (int x : arr) {
        if (x == 1) count++;
        else break;
    }
    return count;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(k) where k = number of initial 1s
  * Worst case → O(n)
  * Why?
    * You may traverse the entire array if all values are 1.
- 💾 Space Complexity
  * O(1)
  * Why?
    * No extra memory used.

### Approach 2: Binary Search for First 0 (Better)

**Idea:**
- Count of 1s = index of the first 0.
- Example:
- [1 1 1 0 0] → first 0 at index 3 → count = 3.

**Steps:**
- Binary search to find the first index where arr[mid] == 0.
- If no 0 exists → return N.
- Else return that index.

**Java Code:**
```java
public int countOnes(int[] arr) {
    int low = 0, high = arr.length - 1;
    int firstZero = arr.length;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == 0) {
            firstZero = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }

    return firstZero;
}
```

**💭 Intuition Behind the Approach:**
- The first 0 divides the array into:
- [1s] [0s]
- Finding that boundary gives us the count of 1s.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Because each step halves the search space.
- 💾 Space Complexity
  * O(1)
  * Only a few variables used.

### Approach 3: Binary Search for Last 1 (Optimal)

**Idea:**
- Find last index where arr[mid] == 1, then count = mid + 1

**Steps:**
- Steps
- Binary search over array.
- If arr[mid] == 1, record index and move right to find last 1.
- If arr[mid] == 0, move left.
- At end →
  * if last1 = -1, return 0
  * else return last1 + 1

**Java Code:**
```java
public int countOnes(int[] arr) {
    int low = 0, high = arr.length - 1;
    int lastOne = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == 1) {
            lastOne = mid;
            low = mid + 1; // search right
        } else {
            high = mid - 1;
        }
    }

    return lastOne + 1;
}
```

**💭 Intuition Behind the Approach:**
- The array looks like this:
  * 1 1 1 … 1 | 0 0 0
- Binary search targets the rightmost 1 efficiently.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Boundary search using binary search.
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Binary search works perfectly because the array has a monotonic pattern: all 1s first, then all 0s.
- Finding the transition boundary is the fastest way to determine count.
---

## 7. Variants / Follow-Ups

- Find first 1
- Find first 0 in increasing binary array
- Count zeros
- First and last occurrence of any target
- Works in monotonic boolean functions
- Find first element greater than X
---

## 8. Tips & Observations

- Always look for monotonicity to apply binary search.
- For non-increasing binary arrays:
  * First 0 → count of 1s
  * Last 1 → count of 1s
- Binary search boundary problems always need careful condition handling.
---

<!-- #endregion -->
<!-- #region 140-Floor in a Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q140: Floor in a Sorted Array</h1>

## 1. Problem Understanding

- You are given a sorted array without duplicates and a value x.
- You must find the floor of x:
  * Floor of x = largest element ≤ x.
- Return its index (0-based).
- If it doesn’t exist → return -1.
---

## 2. Constraints

- 1 ≤ N ≤ 1e5
- Array sorted strictly increasing
- 0 ≤ x ≤ arr[N-1]
---

## 3. Edge Cases

- x < smallest element → return -1
- x > largest element → return index of last element
- Exact match exists → return index
- Single-element array
- Missing element but floor exists (example: x=5, array=[1,2,8…])
---

## 4. Examples

```text
Example 1

Input

7 0
1 2 8 10 11 12 19


Output

-1

Example 2

Input

7 5
1 2 8 10 11 12 19


Output

1

Example 3

Input

7 10
1 2 8 10 11 12 19


Output

3
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Scan from left until elements exceed x.

**Steps:**
- Loop through array
- Track last index with arr[i] <= x
- Return index

**Java Code:**
```java
public int floorIndex(int[] arr, int x) {
    int ans = -1;
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] <= x) ans = i;
        else break;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Since array is sorted, once you exceed x, you won’t find a valid floor later.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Might scan whole array.
- 💾 Space Complexity
  * O(1)

### Approach 2: Binary Search (Optimal)

**Idea:**
- Use binary search to find last element ≤ x.

**Steps:**
- Initialize low=0, high=n-1, ans=-1
- While low ≤ high:
  * Compute mid
  * If arr[mid] ≤ x:
    * Record ans = mid
    * Go right → low = mid + 1
  * Else (arr[mid] > x):
    * Go left → high = mid - 1
- Return ans

**Java Code:**
```java
public int floorIndex(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] <= x) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- This is a boundary search:
- We want the rightmost value that is still ≤ x.
- Binary search lets us jump over large sections to find this efficiently.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Binary search halves the space each step.
- 💾 Space Complexity
- O(1)
- Constant space.

---

## 6. Justification / Proof of Optimality

- Binary search is ideal because the array is sorted and we need to find a boundary position.
- This ensures the best efficiency (O(log n)).
---

## 7. Variants / Follow-Ups

- Find ceil of x (smallest ≥ x)
- Find first element greater than x
- Lower bound / upper bound problems
- Find last occurrence of a number
---

## 8. Tips & Observations

- Floor → go right when arr[mid] <= x
- Ceil → go left when arr[mid] >= x
- Boundary problems always use modified binary search
- Always store ans before moving
---

<!-- #endregion -->
<!-- #region 141-Rotated Sorted Array Search -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q141: Rotated Sorted Array Search</h1>

## 1. Problem Understanding

- You are given an array that was originally sorted in non-decreasing order but then rotated at some pivot. Example:
  * Original: 0 1 2 4 5 6 7
  * Rotated : 4 5 6 7 0 1 2
- Your task:
- Find the index of target B in this rotated sorted array.
- If not present → return -1.
- No duplicates exist.
- Goal: O(log n) → must use binary search.
---

## 2. Constraints

- 1 ≤ N ≤ 1e6
- 1 ≤ A[i] ≤ 1e9
- Array was sorted then rotated
- All elements are unique
- Must achieve O(log n)
---

## 3. Edge Cases

- Target is at pivot
- Array is not rotated at all (normal sorted)
- Target smaller than all elements
- Target larger than all elements
- Single-element array
- Pivot at index 0 or last index
- Target at boundaries
---

## 4. Examples

```text
Example 1

Input

8
4 5 6 7 0 1 2 3
4


Output

0

Example 2

Input

4
5 17 100 3
6


Output

-1
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Loop from 0 to N-1
- If element equals target → return index
- Else return -1

### Approach 2: Find Pivot then Binary Search Both Halves (Better)

**Idea:**
- In a rotated sorted array, the smallest element is the pivot.
- Example:
  * 4 5 6 7 0 1 2 → pivot is at index 4.
- Steps:
  * Find pivot using binary search
  * Decide whether target is in left side or right side
  * Apply binary search normally in that half

**Steps:**
- Binary search to find smallest element index (pivot).
- If target lies in interval [arr[pivot], arr[n-1]] → search in right half.
- Else search in left half.

**Java Code:**
```java
public int search(int[] arr, int target) {
    int n = arr.length;

    // Step 1: find pivot (index of smallest element)
    int low = 0, high = n - 1;
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] > arr[high]) low = mid + 1;
        else high = mid;
    }
    int pivot = low;

    // Step 2: decide which half to search
    if (target >= arr[pivot] && target <= arr[n - 1]) {
        return binarySearch(arr, pivot, n - 1, target);
    } else {
        return binarySearch(arr, 0, pivot - 1, target);
    }
}

private int binarySearch(int[] arr, int low, int high, int target) {
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- A rotated sorted array is two sorted halves.
- Find pivot → pick correct half → normal binary search.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Pivot search: O(log n)
  * Binary search in correct half: O(log n)
  * Total: O(log n)
- 💾 Space Complexity
  * O(1)

### Approach 3: Single-Pass Binary Search (Optimal)

**Idea:**
- We modify binary search logic itself.
- At any mid, one side is always sorted:
- Either:
  * Left half is sorted
  * OR right half is sorted
- We check which half is sorted and decide where to move.

**Steps:**
- Standard binary search loop
- If arr[mid] == target → return mid
- Check which half is sorted
  * If left half sorted (arr[low] ≤ arr[mid]):
    * Check if target lies in left sorted range
    * → move right or left accordingly
  * Else right half sorted:
    * Check if target lies in right sorted range
    * → move accordingly

**Java Code:**
```java
public int search(int[] arr, int target) {
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;

        // Left half sorted
        if (arr[low] <= arr[mid]) {
            if (target >= arr[low] && target < arr[mid])
                high = mid - 1;
            else
                low = mid + 1;
        }
        // Right half sorted
        else {
            if (target > arr[mid] && target <= arr[high])
                low = mid + 1;
            else
                high = mid - 1;
        }
    }

    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Every mid splits the array into two halves, and one half is guaranteed to be sorted.
- By checking:
  * whether target lies inside that sorted half
  * or in the unsorted half
- we discard half of the array each time → classic binary search.
- This avoids finding pivot separately.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * We halve the search space every iteration.
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Binary search works perfectly here because even after rotation, one part of the array remains sorted at every division.
- This property ensures we can always make the correct decision about which half to search.
---

## 7. Variants / Follow-Ups

- Search element in rotated array with duplicates
- Search min element in rotated array
- Search max element in rotated array
- Count rotations
- Find pivot index
- Find peak element
---

## 8. Tips & Observations

- At every mid, one half is sorted → the key insight
- If arr[low] <= arr[mid] → left sorted
- Else → right sorted
- Use ranges carefully when comparing target
- Avoid infinite loops by updating low and high properly
---

<!-- #endregion -->
<!-- #region 142-Peak Index in a Mountain Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q142: Peak Index in a Mountain Array</h1>

## 1. Problem Understanding

- You are given a mountain array, meaning:
- It strictly increases up to a peak
- Then strictly decreases
- Example:
  * 0 2 5 7 6 3 1
          * ↑
         * peak
- You must find the index of the peak element.
- Mountain array guarantees:
- Exactly one peak
- Peak is not at index 0 or index n-1
- Must solve in O(log n)
---

## 2. Constraints

- 3 ≤ n ≤ 1e5
- Values: 0 ≤ arr[i] ≤ 1e6
- Array is guaranteed to be a perfect mountain
- Must solve in O(log n)
---

## 3. Edge Cases

- (not many because mountain is guaranteed)
  * Peak at index 1 (smallest valid peak)
  * Peak at index n-2 (largest valid peak)
  * Large values
  * Minimum length (3 elements)
---

## 4. Examples

```text
Example 1

Input

3
0 1 0


Output

1

Example 2

Input

4
0 2 1 0


Output

1

Example 3

Input

4
0 10 5 2


Output

1
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Scan until the numbers stop increasing.
- The first time you see a drop, the previous index is peak.

**Steps:**
- Loop from i = 1 to n-1
- If arr[i] < arr[i-1]
  * → return i-1
- Because guaranteed mountain → no other checks needed.

**Java Code:**
```java
public int peakIndex(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] < arr[i - 1]) return i - 1;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Peak is exactly where sequence changes from increasing → decreasing.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
- 💾 Space Complexity
  * O(1)

### Approach 2: Binary Search (Optimal)

**Idea:**
- Binary search for the point where slope changes:
  * If arr[mid] < arr[mid + 1] → we are on the increasing slope
  * → peak is on the right
  * → move low = mid + 1
  * If arr[mid] > arr[mid + 1] → we are on the decreasing slope
  * → mid could be the peak
  * → move high = mid
- Eventually low == high → peak index.

**Steps:**
- Initialize low = 0, high = n-1
- While low < high:
  * Compute mid
  * If increasing part → move right
  * If decreasing part → move left
- Return low (same as high)

**Java Code:**
```java
public int peakIndex(int[] arr) {
    int low = 0, high = arr.length - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] < arr[mid + 1]) {
            // Increasing part: peak is on the right
            low = mid + 1;
        } else {
            // Decreasing part: peak is mid or left side
            high = mid;
        }
    }

    return low; // or high, both same
}

In class solution
public int findPeak(int arr[], int n) {

    // Handle corner peaks
    if (n == 1) return 0;   // Only one element
    if (arr[0] >= arr[1]) return 0;
    if (arr[n-1] >= arr[n-2]) return n-1;

    int low = 1, high = n - 2;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        // Correct peak condition
        if (arr[mid] >= arr[mid-1] && arr[mid] >= arr[mid+1]) {
            return mid;
        }

        // If increasing → peak lies to the right
        if (arr[mid] < arr[mid + 1]) {
            low = mid + 1;
        }
        else {
            // If decreasing → peak lies to the left
            high = mid - 1;
        }
    }

    return -1;   // theoretically unreachable
}
  
```

**💭 Intuition Behind the Approach:**
- The mountain array has a monotonic behavior:
  * Increasing → Peak → Decreasing
- At any mid:
  * If arr[mid] < arr[mid+1], slope is increasing → move right
  * If arr[mid] > arr[mid+1], slope is decreasing → peak is left or mid
- We narrow down the peak using binary search.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * We cut the search space in half each step.
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Binary search works because the array is monotonic on each side of the peak:
  * left half strictly increasing
  * right half strictly decreasing
- This allows you to determine the direction of the peak at every step, ensuring O(log n) time.
---

## 7. Variants / Follow-Ups

- Find peak in mountain array (LeetCode 852)
- Find peak element in unsorted array (LeetCode 162)
- Find max in bitonic array
- Find first element greater than previous
- Find turning point in unimodal array
---

## 8. Tips & Observations

- Use mid < mid+1 to detect slope
- Always check arr[mid] < arr[mid+1] FIRST
- low will always converge to the peak
- Never check neighbors outside bounds because mountain is guaranteed
---

<!-- #endregion -->
<!-- #region 143-Search in a Bitonic Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q143: Search in a Bitonic Array</h1>

## 1. Problem Understanding

- You’re given a bitonic array — strictly increasing then strictly decreasing.
- You need to find the index of a target element.
- If not present → return -1.
---

## 2. Constraints

- 1 ≤ N ≤ 10^5
- Array elements: -10^6 ≤ arr[i] ≤ 10^6
- Array is bitonic, not rotated.
- Must run better than O(N) → aim for O(log N).
---

## 3. Edge Cases

- Array of size 1
- Target is at:
  * Peak
  * Beginning
  * End
- All elements increasing (no decreasing part)
- All elements decreasing (no increasing part)
- Target not present
---

## 4. Examples

```text
Example 1

Input:
[-3, 9, 18, 20, 17, 5, 1], target = 20
Output: 3

Example 2

Input:
[3, 4, 1], target = 5
Output: -1
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Search)

**Idea:**
- Scan every element and check if it equals target.

**Steps:**
- Loop from 0 to N-1
- If arr[i] == target → return i
- Else return -1

**Java Code:**
```java
public int findInBitonic(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

**Complexity (Time & Space):**
- ⏱️ Complexity
- Time: O(N)
  * Because we may scan the entire array.
- Space: O(1)
  * No extra memory used.

### Approach 2: Find Peak + Two Binary Searches

**Idea:**
- First, find the peak of the bitonic array using binary search.
- Perform ascending binary search on left side.
- Perform descending binary search on right side.

**Steps:**
- Step 1 — Find Peak
  * Binary search to find index peak where:
    * arr[mid] > arr[mid-1] && arr[mid] > arr[mid+1]
- Step 2
  * Binary search (ascending) from 0 to peak.
- Step 3
  * Binary search (descending) from peak to N-1.

**Java Code:**
```java
public int findInBitonic(int[] arr, int target) {
    int peak = findPeak(arr);

    int left = ascBinary(arr, 0, peak, target);
    if (left != -1) return left;

    return descBinary(arr, peak + 1, arr.length - 1, target);
}

private int findPeak(int[] arr) {
    int low = 0, high = arr.length - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] < arr[mid + 1]) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return low;
}

private int ascBinary(int[] arr, int low, int high, int target) {
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}

private int descBinary(int[] arr, int low, int high, int target) {
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;
        else if (arr[mid] > target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- A bitonic array has only one peak. You can find it using binary search in O(log N).
- Once you know the peak:
  * Left part is sorted ascending.
  * Right part is sorted descending.
  * Binary search works on both separately — guaranteeing logarithmic time.

**Complexity (Time & Space):**
- Time:
  * Peak search → O(log N)
  * Two binary searches → O(log N)
  * Total → O(log N)
  * Why?
    * All operations are binary searches that divide the search space in half each iteration.
- Space:
  * O(1)
  * Why?
    * No extra data structures used.

### Approach 3: Single Binary Search Without Explicit Peak Search

**Idea:**
- Perform binary search directly using bitonic property:
- At each mid:
- If arr[mid] == target → return mid
- Determine whether you're in increasing or decreasing part:
  * If arr[mid] < arr[mid+1] → you're in increasing part
  * Else → in decreasing part
- Based on direction + value relation with target, move left or right.

**Steps:**
- Standard binary search bounds low = 0, high = N-1
- Use slope (increasing/decreasing) to decide direction

**Java Code:**
```java
public int findInBitonicOptimal(int[] arr, int target) {
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;

        boolean inc = mid + 1 < arr.length && arr[mid] < arr[mid + 1];

        if (inc) {
            // Increasing part
            if (target > arr[mid]) low = mid + 1;
            else high = mid - 1;
        } else {
            // Decreasing part
            if (target > arr[mid]) high = mid - 1;
            else low = mid + 1;
        }
    }

    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Instead of first finding the peak, you infer the slope at mid:
  * If slope ↑ (increasing), you can decide which direction the target lies.
  * If slope ↓ (decreasing), same logic but reversed.
- This avoids three separate binary searches → simplifies logic.

**Complexity (Time & Space):**
- Time: O(log N)
- Because each step halves the search range.
- Space: O(1)
- No recursion or extra memory.

---

## 6. Justification / Proof of Optimality

- Binary search leverages the monotonic nature of subarrays.
- Bitonic arrays split naturally into two monotonic halves.
- Using either:
  * peak → 2 binary searches
  * or
  * slope → 1 unified binary search
  * ensures O(log N) performance.
---

## 7. Variants / Follow-Ups

- Search in rotated array
- Find peak element
- Mountain array search (LeetCode)
- Find minimum in rotated array
- Check if array is bitonic
---

## 8. Tips & Observations

- Peak in bitonic array is always found via arr[mid] < arr[mid+1]
- Use < and <= carefully to avoid infinite loops
- Avoid recursion for bitonic search — iterative is cleaner
---

<!-- #endregion -->
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
<!-- #region 145-Floor and Ceil in Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q145: Floor and Ceil in Sorted Array</h1>

## 1. Problem Understanding

- Given a sorted array nums and an integer x, find:
  * Floor(x) = largest element <= x
  * Ceil(x) = smallest element >= x
- If no valid floor/ceil exists → return -1.
- This is a classic binary search variation.
---

## 2. Constraints

- 1 <= nums.length <= 100000
- 0 < nums[i], x < 100000
- Array is sorted in ascending order.
---

## 3. Edge Cases

- x is smaller than all elements → floor = -1
- x is greater than all elements → ceil = -1
- x exactly matches an element → floor = ceil = x
- Duplicate values present
- Array of size 1
---

## 4. Examples

```text
nums = [3,4,4,7,8,10], x=5
👉 floor = 4, ceil = 7

nums = [3,4,4,7,8,10], x=8
👉 floor = 8, ceil = 8

nums = [2,4,6,8,10,12,14], x=1
👉 floor = -1, ceil = 2
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Search)

**Idea:**
- Scan the whole array and track:
- largest value <= x
- smallest value >= x

**Steps:**
- Traverse the array once.
- Compare each element with x.
- Update floor or ceil accordingly.

**Java Code:**
```java
public static int[] floorCeilBrute(int[] nums, int x) {
    int floor = -1, ceil = -1;

    for (int num : nums) {
        if (num <= x) floor = num;
        if (num >= x && ceil == -1) ceil = num;
    }

    return new int[]{floor, ceil};
}
```

**💭 Intuition Behind the Approach:**
- We simply check every element.
- If it’s ≤ x, it can be a floor.
- If it’s ≥ x and no ceil yet, it's the smallest so far.

**Complexity (Time & Space):**
- Time: O(N) → we traverse all elements
  * Reason: Every element is checked once.
- Space: O(1) → no extra storage

### Approach 2: Binary Search (Optimal)

**Idea:**
- Use binary search to:
  * Find floor = rightmost position where nums[i] <= x
  * Find ceil = leftmost position where nums[i] >= x

**Steps:**
- Use two modified binary searches:
  * One for floor
  * One for ceil

**Java Code:**
```java
class Solution {

    public int floor(int[] nums, int x) {
        int s = 0, e = nums.length - 1;
        int ans = -1;

        while (s <= e) {
            int mid = s + (e - s) / 2;

            if (nums[mid] <= x) {
                ans = nums[mid];
                s = mid + 1;
            } else {
                e = mid - 1;
            }
        }
        return ans;
    }

    public int ceil(int[] nums, int x) {
        int s = 0, e = nums.length - 1;
        int ans = -1;

        while (s <= e) {
            int mid = s + (e - s) / 2;

            if (nums[mid] >= x) {
                ans = nums[mid];
                e = mid - 1;
            } else {
                s = mid + 1;
            }
        }
        return ans;
    }

    public int[] getFloorAndCeil(int[] nums, int x) {
        return new int[]{floor(nums, x), ceil(nums, x)};
    }
}
```

**💭 Intuition Behind the Approach:**
- Binary search helps reduce search space by half each step.
  * For floor, we move right whenever nums[mid] <= x because a better (bigger) floor may exist.
  * For ceil, we move left whenever nums[mid] >= x because a smaller (closer) ceil may exist.
- We keep storing the candidate answer while narrowing down.

**Complexity (Time & Space):**
- Time: O(log N)
  * Reason: Each search halves the array.
- Space: O(1)
  * Reason: Only pointers and variables used.

---

## 6. Justification / Proof of Optimality

- The optimal binary-search approach is correct because:
  * The array is sorted → allows monotonic decisions.
  * Floor/ceil conditions (<= x, >= x) allow pruning half of the search space.
  * By storing candidate answers, we guarantee closest values.
---

## 7. Variants / Follow-Ups

- Find just floor
- Find just ceil
- Lower Bound (>= x)
- Upper Bound (> x)
- Insert Position (LeetCode 35)
- Count occurrences of a number using LB + UB
---

## 8. Tips & Observations

- Floor ⇢ look for ≤ x
- Ceil ⇢ look for ≥ x
- Floor always moves right on match
- Ceil always moves left on match
- If no candidate found → answer = -1
- <= and >= are the key comparisons for these problems.
---

<!-- #endregion -->
<!-- #region 146-Search in rotated sorted array-II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q146: Search in rotated sorted array-II</h1>

## 1. Problem Understanding

- You are given a rotated sorted array (in ascending order) that may contain duplicates.
- Example of rotated sorted array:
  * [7,8,1,2,3,3,3,4,5,6]
- You need to check if target k exists in the array.
- Duplicates make this problem trickier because the usual binary-search logic may fail when nums[mid] == nums[low] == nums[high].
- Return:
  * True → if target exists
  * False → if target does not exist
---

## 2. Constraints

- 1 <= nums.length <= 10000
- Elements may have duplicates
- Array is rotated at some pivot
- -10^4 <= nums[i], k <= 10^4
- Time target is better than O(N) but worst-case O(N) may occur due to duplicates
---

## 3. Edge Cases

- Entire array consists of duplicates → [2,2,2,2,2]
- Target equals pivot element
- Target is in left sorted part
- Target is in right sorted part
- Single element array
- Rotation at index 0 (no actual rotation)
---

## 4. Examples

```text
nums = [7,8,1,2,3,3,3,4,5,6], k=3
👉 Output: True

nums = [7,8,1,2,3,3,3,4,5,6], k=10
👉 Output: False

nums = [7,8,1,2,3,3,3,4,5,6], k=7
👉 Output: True
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Steps:**
- Iterate from start to end.
- If num == k → return true.

**Java Code:**
```java
public boolean searchBrute(int[] nums, int k) {
    for (int n : nums) {
        if (n == k) return true;
    }
    return false;
}
```

**Complexity (Time & Space):**
- Time: O(N) → scans all elements
- Space: O(1) → no extra data structures

### Approach 2: Modified Binary Search for Rotated Array with Duplicates

**Idea:**
- Use binary search but handle duplicates by shrinking boundaries.

**Steps:**
- While l <= r:
  * Compute mid.
  * If nums[mid] == k → return true.
  * Problem: duplicates like 2,2,2,2,2
  * solution → move l++ and r--
- Otherwise determine which part is sorted:
  * If left part sorted:
    * If nums[l] <= k < nums[mid] → search left
    * else → search right
  * Else right part sorted:
    * If nums[mid] < k <= nums[r] → search right
    * else → search left

**Java Code:**
```java
class Solution {
    public boolean search(int[] nums, int k) {
        int l = 0, r = nums.length - 1;

        while (l <= r) {
            int mid = l + (r - l) / 2;

            if (nums[mid] == k) return true;

            // Handle duplicates
            if (nums[l] == nums[mid] && nums[mid] == nums[r]) {
                l++;
                r--;
                continue;
            }

            // Left side sorted
            if (nums[l] <= nums[mid]) {
                if (nums[l] <= k && k < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            }
            // Right side sorted
            else {
                if (nums[mid] < k && k <= nums[r]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }

        return false;
    }
}
```

**💭 Intuition Behind the Approach:**
- Rotated array has one sorted half.
- We identify the sorted half.
- Check if the target falls inside that sorted range.
- Duplicates break the sorted structure, so we shrink from both ends (l++, r--).
- This keeps the search moving and eventually resolves ambiguity.

**Complexity (Time & Space):**
- Time:
  * Best / average: O(log N)
  * Reason: Binary search halves the array.
  * Worst-case: O(N)
  * Reason: When duplicates equalize boundaries (l == mid == r), we shrink one step at a time.
- Space: O(1)
  * Reason: Only pointers used.

---

## 6. Justification / Proof of Optimality

- Binary search is valid because:
  * Rotated sorted arrays always have one sorted half.
  * Even with duplicates, eliminating ambiguous boundary duplicates leads to eventually clear sorted zones.
  * Each step reduces the search space.
---

## 7. Variants / Follow-Ups

- Search in Rotated Array without duplicates (LC 33)
- Find minimum in rotated sorted array
- Find rotation pivot index
- Count rotations in an array
---

## 8. Tips & Observations

- If nums[mid] == nums[l] == nums[r] → shrink both ends
- Always check which half is sorted
- Target range check decides which direction to move
- Worst case O(N) only when many duplicates exist
---

<!-- #endregion -->
<!-- #region 147-Single element in sorted array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q147: Single element in sorted array</h1>

## 1. Problem Understanding

- You're given a sorted array where:
  * Every element appears exactly twice
  * Except one element, which appears only once
  * The array is sorted in non-decreasing order
  * You must return that single element
- Example:
  * [1,1,2,2,3,3,4,5,5] → answer is 4
- The challenge: do it in O(log N) time and O(1) space.
---

## 2. Constraints

- 1 <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- Array is sorted
- Exactly one element is single
- Others appear twice
---

## 3. Edge Cases

- Single-element array → answer is nums[0]
- Single element at beginning
- Single element at end
- Single element in the middle
- Array always has odd length
- Patterns like:
  * [unique, a, a, b, b]
  * [a, a, b, b, unique]
  * [a, a, unique, b, b]
---

## 4. Examples

```text
Example 1

Input:
[1,1,2,2,3,3,4,5,5,6,6]
Output:
4

Example 2

Input:
[1,1,3,5,5]
Output:
3

Example 3

Input:
[1,1,2,2,3,3,4,4,5,5,6,6,7]
Output:
7
```

---

## 5. Approaches

### Approach 1: Brute Force (Linear Scan)

**Idea:**
- Check pairs sequentially. When a pair breaks, that's the single element.

**Steps:**
- Loop through array in steps of 2
- If nums[i] != nums[i+1] → return nums[i]
- If no mismatch found → last element is the answer

**Java Code:**
```java
public int singleNonDuplicate(int[] nums) {
    for (int i = 0; i < nums.length - 1; i += 2) {
        if (nums[i] != nums[i + 1]) return nums[i];
    }
    return nums[nums.length - 1];
}
```

**💭 Intuition Behind the Approach:**
- Pairs must always start at even indices until the single element appears.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) because you scan sequentially
  * Why → checking pairs one-by-one
- 💾 Space Complexity
  * O(1) → no extra memory

### Approach 2: Optimal Binary Search (Even-Index Trick)

**Idea:**
- Before the single element:
- Pairs are:
  * (even, odd)
  * (even, odd)
- After the single element:
  * The pairing pattern shifts.
- We use this property to locate the break.

**Steps:**
- Use binary search on the index
- Ensure mid is even
- Compare nums[mid] with nums[mid + 1]
- If they match → move right
- If not → move left (single is at mid or before)

**Java Code:**
```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int low = 0, high = nums.length - 1;

        while (low < high) {
            int mid = low + (high - low) / 2;

            if (mid % 2 == 1) mid--; // convert mid to even

            if (nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            } else {
                high = mid;
            }
        }

        return nums[low];
    }
}
```

**💭 Intuition Behind the Approach:**
- Pairs MUST be aligned as (even, odd) until they are broken by the single element.
- The first index where this alignment breaks is the answer.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log N)
  * Why → range halves every iteration
- 💾 Space Complexity
  * O(1)

### Approach 3: Optimal (Mid-Neighbor Check Pattern)

**Idea:**
- First check edge-case positions
- Then use binary search in the range [1, n-2]
- The single element must be ≠ both neighbors
- Otherwise, use parity logic to decide direction

**Java Code:**
```java
import java.util.*;

class Solution {
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length;

        // Edge cases
        if (n == 1) return nums[0];
        if (nums[0] != nums[1]) return nums[0];
        if (nums[n - 1] != nums[n - 2]) return nums[n - 1];

        int low = 1, high = n - 2;

        while (low <= high) {
            int mid = (low + high) / 2;

            // Single element found
            if (nums[mid] != nums[mid + 1] && nums[mid] != nums[mid - 1]) {
                return nums[mid];
            }

            // Left part is paired correctly
            if ((mid % 2 == 1 && nums[mid] == nums[mid - 1]) ||
               (mid % 2 == 0 && nums[mid] == nums[mid + 1])) {

                low = mid + 1;  // go right
            } 
            else {
                high = mid - 1; // go left
            }
        }

        return -1; // should never reach
    }
}
```

**💭 Intuition Behind the Approach:**
- A valid pair always consumes two elements
- If the pairing around mid matches the parity pattern, the single element is on the right
- If the pairing is broken early, the single element is on the left

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log N)
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Both binary search solutions are correct because:
- The array structure is predictable
- The single element disrupts the pairing pattern
- Binary search efficiently locates this disruption
- No extra space required
- Sorted array → monotonic behavior exists in pair alignment
---

## 7. Variants / Follow-Ups

- Find the single element when all others appear three times
- Find the single element in unsorted array
- Bit manipulation version (O(N) but constant-space)
---

## 8. Tips & Observations

- Pairs ALWAYS start at even index until the unique element
- After the unique element, pairing shifts
- If you’re confused, print index parities during dry run
- The even-index trick is the cleanest and most common solution
- Your neighbor-check solution is also valid but more verbose
---

<!-- #endregion -->
<!-- #region 148-Find square root of a number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q148: Find square root of a number</h1>

## 1. Problem Understanding

- You are given a non-negative integer n.
- You must return its square root, but if the number is not a perfect square, return the floor value of sqrt(n).
- Example:
  * 36 → 6
  * 28 → floor(5.29…) = 5
  * This is a classic Binary Search on Answer problem.
---

## 2. Constraints

- 0 <= n <= 2^31 - 1
- Large input → cannot use floating operations directly
- Answer fits in int (floor of sqrt always ≤ 46340 for 32-bit)
---

## 3. Edge Cases

- n = 0 → output 0
- n = 1 → output 1
- Very large value like 2^31 - 1
- Perfect squares like 49, 121
- Non-perfect squares like 50, 90
---

## 4. Examples

```text
🧪 Examples
Example 1

Input: 36
Output: 6

Example 2

Input: 28
Output: 5

Example 3

Input: 50
Output: 7
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all x from 1 upward until x*x > n.
- The previous x is the floor sqrt.

**Steps:**
- Loop from 1 to n
- Stop when i*i > n

**Java Code:**
```java
public int mySqrt(int n) {
    int i = 1;
    while (i * 1L * i <= n) {
        i++;
    }
    return i - 1;
}
```

**💭 Intuition Behind the Approach:**
- You keep guessing increasing numbers until you cross n.

**Complexity (Time & Space):**
- Time: O(√n) (because loop runs √n times)
- Space: O(1)

### Approach 2: Binary Search Binary Search (l <= h Classic Search Style)

**Idea:**
- This version explicitly checks if mid*mid == n.
- If not, it finds the floor where mid*mid < n.

**Steps:**
- Use l <= h
- If mid*mid == n → return mid
- If mid*mid < n → answer may be mid, go right
- Else → go left

**Java Code:**
```java
public int mySqrt(int n) {
    if (n == 0 || n == 1) return n;

    int low = 1, high = n;
    int ans = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        long sq = (long) mid * mid;

        if (sq == n) return mid;

        if (sq < n) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We do a classic target search:
  * If mid² equals n → perfect square
  * Else keep the last valid mid as candidate (floor sqrt)

**Complexity (Time & Space):**
- Time: O(log n)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Binary search is valid because the function:
  * f(x) = x*x
- is monotonically increasing.
- So we can search for the boundary where:
  * x*x <= n is true
  * x*x > n is false
- This monotonic structure allows binary search.
---

## 7. Variants / Follow-Ups

- Find cube root (floor)
- Find sqrt using Newton's Method
- Integer sqrt for 64-bit values
- Check if n is a perfect square
- Floating point sqrt with precision
---

## 8. Tips & Observations

- ALWAYS cast to long when checking mid*mid to avoid overflow
- l < h = shrink range → stop at first mid*mid > n
- l <= h = classic search → check equality
- Floor sqrt is always ≤ 46340 for 32-bit integers
- In competitive coding, (l < h) version is preferred
- In interviews, (l <= h) is easier to explain
---

<!-- #endregion -->
<!-- #region 149-Find Nth root of a number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q149: Find Nth root of a number</h1>

## 1. Problem Understanding

- You're given two integers:
  * N → the root to find
  * M → the number
  * You must find an integer X such that:
  * X^N == M
- If such integer X exists → return it.
- If no such integer exists → return -1.
- Example:
  * 3rd root of 27 = 3
  * 4th root of 69 ≠ integer → return -1
  * 4th root of 81 = 3
- This is a classic Binary Search on Answer problem.
---

## 2. Constraints

- 1 <= N <= 30
- 1 <= M <= 10^9
- Answer always lies in [1, M]
- Power X^N can overflow → must use long or safe multiplication
---

## 3. Edge Cases

- N = 1 → answer is always M
- M = 1 → answer is always 1
- Huge values like M = 10^9 and N = 30
- Non-perfect roots (must return -1)
- Overflow while doing mid^N
---

## 4. Examples

```text
Example 1

Input: N=3, M=27
Output: 3

Example 2

Input: N=4, M=69
Output: -1

Example 3

Input: N=4, M=81
Output: 3
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all numbers from 1 to M and compute i^N.
- If match → return i.

**Steps:**
- Loop i from 1 to M
- Compute i^N until it exceeds M

**Java Code:**
```java
public int nthRoot(int N, int M) {
    for (long i = 1; i <= M; i++) {
        long val = 1;
        for (int j = 0; j < N; j++) val *= i;

        if (val == M) return (int)i;
        if (val > M) break;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Sequential check → guaranteed correct but slow.

**Complexity (Time & Space):**
- O(M * N) → too slow
- Space: O(1)

### Approach 2: Binary Search (l <= h)

**Idea:**
- Use classic binary search to check if mid^N == M.

**Steps:**
- Use l <= h
- If mid^N == M → return mid
- If mid^N < M → go right
- Else → go left
- If no equality found → return -1

**Java Code:**
```java
public int nthRoot(int N, int M) {
    int low = 1, high = M;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        long val = power(mid, N, M);

        if (val == M) return mid;

        if (val < M) low = mid + 1;
        else high = mid - 1;
    }

    return -1;
}

private long power(long x, int n, int limit) {
    long ans = 1;
    for (int i = 0; i < n; i++) {
        ans *= x;
        if (ans > limit) return ans;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- We perform a direct equality search for mid^N = M.
- Binary Search ensures we check the correct mid values only.

**Complexity (Time & Space):**
- O(N * log M)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Binary search is valid because:
- The function f(x) = x^N is strictly increasing
- This means:
  * If x1 < x2 → x1^N < x2^N
- So the search space is monotonic, allowing binary search.
---

## 7. Variants / Follow-Ups

- Find cube root
- Find square root
- Check if number is a perfect power
- Find nth root with precision
- Floating-point nth root using Newton’s Method
---

## 8. Tips & Observations

- Use safe multiplication to avoid overflow
- mid^N grows VERY fast for N = 30
- Prefer (l <= h) approach in interviews
- Prefer (l < h) approach in competitive programming
- Always cap multiplication when > M
- Search space is always [1, M]
---

<!-- #endregion -->
<!-- #region 150-Find the smallest divisor -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q150: Find the smallest divisor</h1>

## 1. Problem Understanding

- You are given:
- An array nums[]
- An integer limit
- You must find the smallest positive integer divisor d such that:
  * ceil(nums[0] / d)
  * + ceil(nums[1] / d)
  * + ...
  * + ceil(nums[n-1] / d)
  * <= limit
- You must minimize the divisor.
- This is Binary Search on Answer.
---

## 2. Constraints

- 1 ≤ nums.length ≤ 5 * 10^4
- 1 ≤ nums[i] ≤ 10^6
- nums.length ≤ limit ≤ 10^6
- Divisor is a positive integer
- Divisor range: 1 → max(nums[])
---

## 3. Edge Cases

- limit very large → smallest divisor = 1
- limit smaller than n → impossible unless divisor very large
- nums containing large values (up to 10^6)
- Single-element array
- All elements equal
---

## 4. Examples

```text
Example 1

Input: nums = [1,2,3,4,5], limit=8
Output: 3

Example 2

Input: nums = [8,4,2,3], limit=10
Output: 2

Example 3

Input: nums = [8,4,2,3], limit=4
Output: 2
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all divisors from 1 to max(nums) and check when the sum becomes ≤ limit.

**Steps:**
- Compute maximum element (maxD)
- For divisor d from 1 to maxD:
  * compute sum of ceil(nums[i]/d)
  * if sum ≤ limit → return d

**Java Code:**
```java
public int smallestDivisor(int[] nums, int limit) {
    int max = 0;
    for (int x : nums) max = Math.max(max, x);

    for (int d = 1; d <= max; d++) {
        if (isPossible(nums, d, limit)) return d;
    }
    return -1;
}

private boolean isPossible(int[] nums, int d, int limit) {
    int sum = 0;
    for (int x : nums) {
        sum += (x + d - 1) / d; // ceil division
        if (sum > limit) return false;
    }
    return sum <= limit;
}
```

**💭 Intuition Behind the Approach:**
- Check every possible divisor. Very slow for large max element.

**Complexity (Time & Space):**
- O(max(nums) * n) → Too slow
- Space: O(1)

### Approach 2: Binary Search (l <= h) (Classic Search Style)

**Idea:**
- ✔ Explicitly finds smallest valid mid
- ✔ Checks all possibilities in binary-search manner
- Use classic binary search and store candidate answer when valid.

**Steps:**
- Initialize low=1, high=max element
- mid = (low + high)/2
- If mid works → store as candidate, search left
- If mid doesn't work → search right

**Java Code:**
```java
public int smallestDivisor(int[] nums, int limit) {
    int low = 1;
    int high = 0;
    for (int x : nums) high = Math.max(high, x);

    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (isPossible(nums, mid, limit)) {
            ans = mid;
            high = mid - 1;  // try smaller divisor
        } else {
            low = mid + 1;   // need larger divisor
        }
    }
    return ans;
}

private boolean isPossible(int[] nums, int d, int limit) {
    int sum = 0;
    for (int x : nums) {
        sum += (x + d - 1) / d;
        if (sum > limit) return false;
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Binary search tries divisors and narrows down the smallest valid.

**Complexity (Time & Space):**
- O(n * log(max(nums)))
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Binary Search works because:
  * sum(ceil(nums[i]/d)) is a monotonic decreasing function of divisor d.
- Meaning:
  * If divisor increases → sum decreases
  * If divisor decreases → sum increases
- So the valid/invalid pattern looks like:
  * invalid invalid invalid valid valid valid
- Binary search perfectly finds the first valid divisor.
---

## 7. Variants / Follow-Ups

- Koko eating bananas (minimum eating speed)
- Shipping packages (minimum capacity)
- Split array largest sum
- Painter partition
- Minimum time to complete tasks
- All follow same pattern.
---

## 8. Tips & Observations

- ALWAYS use ceil division: (x + d - 1) / d
- Divisor range always begins from 1, not 0
- Upper bound = max element
- isPossible() must stop early when sum exceeds limit
- (l < h) → shrinking to minimal valid
- (l <= h) → storing candidate while searching
- Classic BSOA pattern: minimize value satisfying condition
---

<!-- #endregion -->
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

### Approach 2: Binary Search  (l <= h)

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

### Approach 2: Binary Search (l <= h)

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
<!-- #region 154-Book Allocation Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q154: Book Allocation Problem</h1>

## 1. Problem Understanding

- You are given:
- nums[i] = pages in i-th book
- m = number of students
- You must allocate contiguous books
- Every student must receive at least 1 book
- A book cannot be split
- Goal → Minimize the maximum pages assigned to any student
- This is a classic Binary Search on Answer (BSOA) minimization problem.
- We want to find the minimum possible maximum pages a student reads.
---

## 2. Constraints

- 1 <= n, m <= 10^4
- 1 <= nums[i] <= 10^5
- Large values → greedy O(n) + binary search O(log(sum)) required
- Contiguous allocation required
- m > n → impossible (not enough books to give 1 per student)
---

## 3. Edge Cases

- If m > n → return -1
- Single book → answer is that book
- Single student → answer = sum of array
- All books same pages
- Very large pages → use long for sum
---

## 4. Examples

```text
Example 1
nums = [12, 34, 67, 90], m = 2
Output: 113

Example 2
nums = [25, 46, 28, 49, 24], m = 4
Output: 71

Example 3
nums = [15, 17, 20], m = 2
Output: -1 (when m > n)
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try every possible maximum pages value from:
  * max(nums) → sum(nums)
- and check feasibility.

**Steps:**
- For each X in the range
- Try to allocate books greedily
- If at any point allocations ≤ m, return X

**Java Code:**
```java
import java.util.*;

class Solution {
    /*Function to count the number of 
    students required given the maximum 
    pages each student can read*/
    private int countStudents(int[] nums, int pages) {
        // Size of array
        int n = nums.length;
        
        int students = 1;
        int pagesStudent = 0;
        
        for (int i = 0; i < n; i++) {
            if (pagesStudent + nums[i] <= pages) {
                // Add pages to current student
                pagesStudent += nums[i];
            } else {
                // Add pages to next student
                students++;
                pagesStudent = nums[i];
            }
        }
        return students;
    }
    
    /* Function to allocate the book to ‘m’ 
    students such that the maximum number 
    of pages assigned to a student is minimum*/
    public int findPages(int[] nums, int m) {
        int n = nums.length;
        
        // Book allocation impossible
        if (m > n) return -1;

        // Calculate the range for search
        int low = Integer.MIN_VALUE;
        int high = 0;
        for(int i = 0; i < n; i++){
            low = Math.max(low, nums[i]);
            high = high + nums[i];
        }

        // Linear search for minimum maximum pages
        for (int pages = low; pages <= high; pages++) {
            if (countStudents(nums, pages) <= m) {
                return pages;
            }
        }
        return low;
    }

    public static void main(String[] args) {
        int[] arr = {25, 46, 28, 49, 24};
        int m = 4;

        // Create an instance of the Solution class
        Solution sol = new Solution();

        int ans = sol.findPages(arr, m);

        // Output the result
        System.out.println("The answer is: " + ans);
    }
}
```

**💭 Intuition Behind the Approach:**
- Check all potential answers manually until a feasible one is found.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O((sum(nums) - max(nums)) * n) → impossible for large constraints
- 💾 Space Complexity
  * O(1)

### Approach 2: Binary Search on Answer (Correct Minimization using <=)

**Idea:**
- The answer lies in:
  * low = max(nums) (minimum max pages must be ≥ largest book)
  * high = sum(nums) (one student reads everything)
- Binary search for the smallest mid such that allocation is possible.

**Steps:**
- Set search space:
  * low = max(nums)
  * high = sum(nums)
- For each mid, check feasibility:
  * Allocate books to student until sum > mid
  * Then start new student
- If number of students needed ≤ m → feasible
- For minimization:
  * if feasible → ans = mid, high = mid - 1
  * else        → low = mid + 1

**Java Code:**
```java
class Solution {
    public int findPages(int[] nums, int m) {

        int n = nums.length;
        if (m > n) return -1;

        int low = 0, high = 0;

        for (int x : nums) {
            low = Math.max(low, x);
            high += x;
        }

        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isPossible(nums, m, mid)) {
                ans = mid;
                high = mid - 1;   // minimize
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }

    private boolean isPossible(int[] nums, int m, int limit) {
        int studentCount = 1;
        int pages = 0;

        for (int x : nums) {
            if (pages + x <= limit) {
                pages += x;
            } else {
                studentCount++;
                pages = x;

                if (studentCount > m) return false;
            }
        }
        return true;
    }
}
```

**💭 Intuition Behind the Approach:**
- We simulate “if each student can read at most mid pages”.
- If it's possible with ≤ m students → we try to reduce mid.
- If not → we increase mid.
- The monotonic property of feasibility makes binary search valid.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n * log(sum(nums)))
  * Why:
  * Each binary search step checks feasibility in O(n)
- 💾 Space Complexity
  * O(1)
  * Just counters and variables.

---

## 6. Justification / Proof of Optimality

- The solution is justified because:
- ✔ 1. The problem has a monotonic feasibility behavior
  * If we can allocate books such that no student reads more than X pages,
  * then we can also allocate for any value > X.
  * This monotonic pattern:
  * F F F F T T T T
  * is required for binary search.
- ✔ 2. The minimum possible maximum pages is always between:
- max(nums)  and  sum(nums)
  * You cannot assign less than the largest single book → lower bound
  * You cannot assign more than total pages → upper bound
  * This forms a valid search space.
- ✔ 3. Greedy feasibility check is optimal
  * Assigning books left → right ensures:
  * Each student gets contiguous books
  * We minimize splitting
  * We count how many students are needed for limit = mid
  * If students needed ≤ m → allocation is possible.
  * The greedy stays valid because:
  * Adding a new student only when needed is always optimal
  * Reordering books is not allowed
- ✔ 4. Binary Search narrows down the exact threshold
  * We search for the smallest mid for which allocation is possible.
  * This is a classic minimization BSOA, so:
  * if possible(mid):
      * ans = mid
      * high = mid - 1
  * else:
      * low = mid + 1
- This guarantees finding the minimum maximum pages.
---

## 7. Variants / Follow-Ups

- Allocate Books (GFG version)
- Painter Partition Problem
- Split Array Largest Sum (LeetCode Hard)
- Ship Packages Within D Days
- Minimize Max Distance to Gas Stations (similar feasibility)
---

## 8. Tips & Observations

- Always use low = max(nums)
- Always use high = sum(nums)
- Always check m > n early
- Feasibility always follows monotonic increasing property
- Use long for sum in bigger constraints
- The key idea: minimizing the maximum load across partitions

- **🕳️ Pitfalls**
    - ❌ Starting low = 0 → WRONG
    - ❌ Ignoring m > n → WRONG
    - ❌ Not resetting pages correctly when switching students
    - ❌ Using < instead of <= in binary search (breaks edge cases)
    - ❌ Handling feasibility incorrectly when pages == limit
    - ❌ Using sorted array (NOT required — order must be preserved)
---

<!-- #endregion -->
<!-- #region 155-Median of 2 sorted arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q155: Median of 2 sorted arrays</h1>

## 1. Problem Understanding

- You are given two sorted arrays arr1 and arr2.
- You must return the median of the combined sorted array without merging (optimal approach).
- Median rules:
  * If total length is odd → return the middle element.
  * If even → return average of the two middle elements.
---

## 2. Constraints

- 0 ≤ m, n ≤ 1000
- 1 ≤ m + n ≤ 2000
- Values can be negative.
---

## 3. Edge Cases

- One array empty.
- Arrays of very uneven sizes.
- Duplicate elements.
- Both arrays length 1.
- All elements in arr1 ≤ all in arr2 (or vice-versa).
---

## 4. Examples

```text
arr1 = [2, 4, 6], arr2 = [1, 3, 5] → median = 3.5
arr1 = [2, 4, 6], arr2 = [1, 3] → median = 3
arr1 = [2, 4, 5], arr2 = [1, 6] → median = 4
```

---

## 5. Approaches

### Approach 1: Brute Force (Merge Like MergeSort)

**Idea:**
- Merge both arrays into one sorted list.
- Find the median normally.

**Steps:**
- Use two pointers.
- Build a merged array.
- Pick median based on total length.

**Java Code:**
```java
public static double medianBrute(int[] a, int[] b) {
    int m = a.length, n = b.length;
    int[] merged = new int[m+n];
    
    int i=0, j=0, k=0;
    while(i<m && j<n){
        if(a[i] < b[j]) merged[k++] = a[i++];
        else merged[k++] = b[j++];
    }
    while(i<m) merged[k++] = a[i++];
    while(j<n) merged[k++] = b[j++];

    int total = m+n;
    if(total % 2 == 1) return merged[total/2];
    
    return (merged[total/2] + merged[total/2 - 1]) / 2.0;
}
```

**💭 Intuition Behind the Approach:**
- Classic merge — straightforward but not optimal.

**Complexity (Time & Space):**
- Time:
  * O(m+n) because we merge all elements.
- Space:
  * O(m+n) because we store merged array.

### Approach 2: Two-Pointer Without Full Merge (Better)

**Idea:**
- Simulate merge until middle index.
- Don’t store full merged array.
- Track only middle elements

**Steps:**
- Use two pointers.
- Iterate only till (m+n)/2.

**Java Code:**
```java
public static double medianBetter(int[] a, int[] b) {
    int m = a.length, n = b.length;
    int total = m+n;
    
    int i=0, j=0;
    int prev = -1, curr = -1;

    for(int k=0; k<=total/2; k++){
        prev = curr;

        if(i<m && (j>=n || a[i] <= b[j])){
            curr = a[i++];
        } else {
            curr = b[j++];
        }
    }

    if(total % 2 == 1) return curr;
    return (prev + curr) / 2.0;
}
```

**💭 Intuition Behind the Approach:**
- We don’t care about the entire merge—only until we hit the median position.

**Complexity (Time & Space):**
- Time:
  * O((m+n)/2) ≈ O(m+n)
- Space:
  * O(1) — no extra array.

### Approach 3: Binary Search on Smaller Array (Partition Method)

**Idea:**
- Use binary search to partition both arrays so that:
  * left half has exactly (m+n+1)/2 elements
  * All left side ≤ all right side
- We binary search on arr1 partition index.

**Steps:**
- Ensure arr1 is smaller array.
- Binary search partition cut1 in arr1.
- Compute cut2 in arr2 = required left half − cut1.
- Check if:
  * maxLeft1 ≤ minRight2
  * maxLeft2 ≤ minRight1
- If not balanced → shift binary search.

**Java Code:**
```java
public static double findMedianSortedArrays(int[] a, int[] b) {
    if(a.length > b.length) return findMedianSortedArrays(b, a);

    int m = a.length, n = b.length;
    int low = 0, high = m;

    while(low <= high){
        int cut1 = low + (high - low)/2;
        int cut2 = (m+n+1)/2 - cut1;

        int left1 = (cut1 == 0 ? Integer.MIN_VALUE : a[cut1-1]);
        int left2 = (cut2 == 0 ? Integer.MIN_VALUE : b[cut2-1]);
        int right1 = (cut1 == m ? Integer.MAX_VALUE : a[cut1]);
        int right2 = (cut2 == n ? Integer.MAX_VALUE : b[cut2]);

        if(left1 <= right2 && left2 <= right1){
            if((m+n) % 2 == 0){
                return (Math.max(left1,left2) + Math.min(right1,right2)) / 2.0;
            } else {
                return Math.max(left1,left2);
            }
        }
        else if(left1 > right2){
            high = cut1 - 1;
        } else {
            low = cut1 + 1;
        }
    }
    
    return 0.0;
}
```

**💭 Intuition Behind the Approach:**
- Instead of merging, we search for the correct partition such that:
  * All left side elements ≤ all right side elements.
  * Once partition is valid, median comes from edges.
- This uses the fact both arrays are sorted → binary search works.

**Complexity (Time & Space):**
- Time:
  * O(log(min(m,n))) because we binary search only the smaller array.
  * It’s optimal because we avoid touching all elements.
- Space:
  * O(1) — no extra space.

### Approach 4: K-th Element via Binary Search Recursion

**Idea:**
- We find the k-th element using recursive shrinking.
- Median =
  * odd → kth element
  * even → avg of k and k+1.

**Java Code:**
```java
public static double medianKth(int[] a, int[] b){
    int m = a.length, n = b.length;
    int total = m+n;

    if(total % 2 == 1){
        return kth(a,0,b,0,total/2 + 1);
    } else {
        double x = kth(a,0,b,0,total/2);
        double y = kth(a,0,b,0,total/2 + 1);
        return (x + y) / 2.0;
    }
}

private static int kth(int[] a, int i, int[] b, int j, int k){
    if(i >= a.length) return b[j + k - 1];
    if(j >= b.length) return a[i + k - 1];
    if(k == 1) return Math.min(a[i], b[j]);

    int midA = (i + k/2 - 1 < a.length) ? a[i + k/2 - 1] : Integer.MAX_VALUE;
    int midB = (j + k/2 - 1 < b.length) ? b[j + k/2 - 1] : Integer.MAX_VALUE;

    if(midA < midB){
        return kth(a, i + k/2, b, j, k - k/2);
    } else {
        return kth(a, i, b, j + k/2, k - k/2);
    }
}



Recursion Tree

Example: total = 10 → find k=5

k=5
 |
 |-- compare a[k/2], b[k/2]
 |
 k becomes 3
 |
 |-- compare next halves
 |
 k becomes 1
 |
 return min(...)
```

**💭 Intuition Behind the Approach:**
- At each step, eliminate k/2 elements from one of the arrays—like searching for the k-th number.

**Complexity (Time & Space):**
- Time:
  * O(log(m+n)) because every recursive step halves k.
- Space:
  * O(log(m+n)) due to recursion stack.

---

## 6. Justification / Proof of Optimality

- The arrays are sorted → median position depends only on ordering.
- Instead of merging all, find a partition where left half contains exactly half of total elements.
- Binary search is possible because:
  * when left1 > right2 → cut1 is too big
  * when left2 > right1 → cut1 is too small
- This monotonic behaviour enables O(log(min(m,n))).
---

## 7. Variants / Follow-Ups

- Find median of k sorted arrays.
- Find k-th smallest of two arrays.
- Find median of infinite sorted streams.
- Weighted median.
- Merging intervals using similar partition logic.
---

## 8. Tips & Observations

- Always binary search on the smaller array.
- Take care of boundaries using ±∞.
- For odd length, median is just max(left side).
- Partition logic is the heart of the problem.

- **⚠️ Pitfalls**
    - Forgetting to pick smaller array for binary search.
    - Boundary handling when partition is at index 0 or m.
    - Floating-point return for even length.
    - Off-by-one errors in partition formula.
---

<!-- #endregion -->
<!-- #region 156-Kth element of 2 sorted arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q156: Kth element of 2 sorted arrays</h1>

## 1. Problem Understanding

- We have two sorted arrays a and b with sizes m and n.
- We need to find the k-th smallest element of the merged sorted array without merging.
---

## 2. Constraints

- 1 ≤ m, n ≤ 10^4
- Values up to 10^9
- Arrays already sorted
- 1 ≤ k ≤ m + n
---

## 3. Edge Cases

- k = 1 (smallest of both first elements)
- k = m + n (largest of both last elements)
- One array completely smaller than the other
- One array empty
- Arrays with duplicates
- Very uneven lengths
---

## 4. Examples

```text
Input:
a = [2,3,6,7,9], b = [1,4,8,10], k = 5
Output: 6

Input:
a = [100,112,256,349,770], b = [72,86,113,119,265,445,892], k = 7
Output: 256
```

---

## 5. Approaches

### Approach 1: Brute Force Merge (Like MergeSort)

**Idea:**
- Merge both arrays and return kth element.

**Steps:**
- Use two pointers.
- Build merged list up to k-th position.
- Return k-th element.

**Java Code:**
```java
public static int kthBrute(int[] a, int[] b, int k) {
    int i=0, j=0;
    int count=0;

    while(i<a.length && j<b.length){
        int val;
        if(a[i] < b[j]) val = a[i++];
        else val = b[j++];
        count++;
        if(count == k) return val;
    }

    while(i < a.length){
        count++;
        if(count == k) return a[i];
        i++;
    }

    while(j < b.length){
        count++;
        if(count == k) return b[j];
        j++;
    }

    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Straight merging until we reach the k-th element.

**Complexity (Time & Space):**
- Time: O(k)
  * We scan up to k elements.
- Space: O(1)
  * No extra array.

### Approach 2: Binary Search on K (Partition Method)

**Idea:**
- Same as median partition logic but now we want exactly k elements on the left side.
- Binary search on cut point in array a.
- Let:
  * cut1 + cut2 = k
- Then we check cross-boundary conditions:
  * left1 <= right2
  * left2 <= right1

**Java Code:**
```java
public static int kthOptimal(int[] a, int[] b, int k){
    int n = a.length, m = b.length;

    // ensure a is smaller
    if(n > m) return kthOptimal(b, a, k);

    int low = Math.max(0, k - m);
    int high = Math.min(k, n);

    while(low <= high){
        int cut1 = low + (high - low)/2;
        int cut2 = k - cut1;

        int l1 = (cut1 == 0) ? Integer.MIN_VALUE : a[cut1 - 1];
        int l2 = (cut2 == 0) ? Integer.MIN_VALUE : b[cut2 - 1];

        int r1 = (cut1 == n) ? Integer.MAX_VALUE : a[cut1];
        int r2 = (cut2 == m) ? Integer.MAX_VALUE : b[cut2];

        if(l1 <= r2 && l2 <= r1){
            return Math.max(l1, l2);
        }
        else if(l1 > r2){
            high = cut1 - 1;
        } 
        else {
            low = cut1 + 1;
        }
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- We find a partition where exactly k elements are on the left.
- The maximum on the left is the k-th smallest.
- This avoids merging and uses binary search on cut index.

**Complexity (Time & Space):**
- Time: O(log(min(m,n)))
  * Binary search on smaller array.
- Space: O(1)

### Approach 3: K-th Element Using Recursion (Divide K by 2)

**Idea:**
- Compare a[k/2] and b[k/2].
- Eliminate k/2 elements from one array.
- Recurse with reduced k.

**Java Code:**
```java
public static int kthRecursive(int[] a, int i, int[] b, int j, int k){
    if(i >= a.length) return b[j + k - 1];
    if(j >= b.length) return a[i + k - 1];
    if(k == 1) return Math.min(a[i], b[j]);

    int midA = (i + k/2 - 1 < a.length) ? a[i + k/2 - 1] : Integer.MAX_VALUE;
    int midB = (j + k/2 - 1 < b.length) ? b[j + k/2 - 1] : Integer.MAX_VALUE;

    if(midA < midB){
        return kthRecursive(a, i + k/2, b, j, k - k/2);
    } else {
        return kthRecursive(a, i, b, j + k/2, k - k/2);
    }
}

Recursion Tree Example (k = 7)
k=7
 |
 |-- compare a[3], b[3]
 |
 k = 7 - 3 = 4
 |
 |-- compare a[?], b[?]
 |
 k = 4 - 2 = 2
 |
 k = 1 or 2
 return answer
```

**💭 Intuition Behind the Approach:**
- You eliminate half of k on each call → like binary search on rank.

**Complexity (Time & Space):**
- Time: O(log(k))
- Space: O(log(k)) recursion stack

---

## 6. Justification / Proof of Optimality

- Arrays are sorted → k-th element depends only on ordering around index k.
- Binary-search partition uses monotonic behavior of partitions.
- Recursive elimination removes half the candidates each step → optimal.
---

## 7. Variants / Follow-Ups

- Find median of two sorted arrays
- Find k-th smallest in multiple sorted arrays
- Find k-th element in infinite sorted streams
- Find k-th largest in sorted arrays (just adjust indexing)
---

## 8. Tips & Observations

- Always binary search on the smaller array.
- Use ±∞ when cut goes out of bounds.
- For k very small (1,2) handle separately to avoid mistakes.
- Partition logic is same as median problem.

- **⚠️ Pitfalls**
    - Wrong bounds for binary search (low = max(0, k-m) and high = min(k,n) are mandatory)
    - Missing base cases for recursion
    - Off-by-one in returning a[i + k - 1] or b[j + k - 1]
    - Forgetting to ensure smaller array in binary search
---

<!-- #endregion -->
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
<!-- #region 158-Split array - largest sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q158: Split array - largest sum</h1>

## 1. Problem Understanding

- You are given an array a of size n.
- You must split it into k non-empty, consecutive subarrays.
- Each split produces subarrays like:
  * [a[0] ... a[x]],
  * [a[x+1] ... a[y]],
  * ...
- For each subarray, compute its sum.
- Among all these sums, take the maximum.
- Your goal:
  * Split array such that the maximum subarray sum is as small as possible.
  * Return this minimized maximum sum.
- This is Minimize Maximum → a perfect Binary Search on Answer (BSOA) problem.
---

## 2. Constraints

- 1 ≤ n ≤ 10^4
- 1 ≤ k ≤ n
- 1 ≤ a[i] ≤ 10^4
---

## 3. Edge Cases

- k = 1 → answer = sum of whole array
- k = n → answer = max element
- Very large numbers → use long for sums
- Array in strictly increasing order → largest splits affect
- All elements equal → simple equal distribution
---

## 4. Examples

```text
Example 1

a = [1,2,3,4,5], k = 3  
Best split = [1,2,3], [4], [5]  
Largest sum = 6


Example 2

a = [3,5,1], k = 3  
Split = [3], [5], [1]  
Largest sum = 5
```

---

## 5. Approaches

### Approach 1: Brute Force (Try all partitions)

**Idea:**
- Try all ways to split array into k subarrays, calculate maximum subarray sum each time, return minimum of all.
- Why it's impossible
  * Number of ways to place k-1 dividers between n-1 gaps:
  * C(n-1, k-1) → exponential for n=10^4
  * Not feasible.

**💭 Intuition Behind the Approach:**
- Brute = check all partitions, but search space too huge.

**Complexity (Time & Space):**
- Time: O(2^n) (unusable)
- Space: O(n)

### Approach 2: Greedy Check + Linear Search (Better but still slow)

**Idea:**
- Try a candidate largest sum S, simulate partition:
  * Keep adding numbers until sum > S
  * When greater → make a cut
  * Count number of cuts (subarrays)
- But instead of binary search, you linearly test S from max(a[i]) to sum(a).
- Why this is bad
  * Worst case sum ~ 10^8 leads to O(sum) checks → impossible.

**💭 Intuition Behind the Approach:**
- Correct checking logic, but wrong outer loop.

**Complexity (Time & Space):**
- Time: O(sum * n) → too slow

### Approach 3: Optimal): Binary Search on Answer

**Idea:**
- We binary search on maximum subarray sum possible (call it mid).
- Range:
  * low = max(a)      (min possible max sum)
  * high = sum(a)     (max possible max sum)
- For each mid:
  * Check how many subarrays we need if no subarray sum exceeds mid.
  * If we need more than k subarrays → mid too small → increase low
  * Else → mid possible → try smaller mid

**Steps:**
- Compute:
  * low = max(a[i])
  * high = sum(a[i])
- Binary search:
  * mid = (low + high) / 2
- Use greedy to count how many subarrays required with max sum ≤ mid.
- Adjust search space.

**Java Code:**
```java
public class Solution {

    public int splitArray(int[] a, int k) {
        int n = a.length;

        int low = 0, high = 0;

        for(int x : a){
            low = Math.max(low, x);  // at least largest single element
            high += x;               // at most sum of all
        }

        while(low <= high){
            int mid = low + (high - low) / 2;

            if(canSplit(a, k, mid)){
                high = mid - 1;  // try smaller largest sum
            } else {
                low = mid + 1;   // mid too small → increase
            }
        }

        return low; // the minimized maximum sum
    }

    private boolean canSplit(int[] a, int k, int maxAllowed){
        int subarrays = 1;
        int currSum = 0;

        for(int x : a){
            if(currSum + x <= maxAllowed){
                currSum += x;
            } else {
                subarrays++;
                currSum = x;
                if(subarrays > k) return false;
            }
        }
        return true;
    }
}
```

**💭 Intuition Behind the Approach:**
- Why binary search works?
- Because:
  * If a max subarray sum S is possible,
  * then ANY sum > S is also possible.
  * If S is NOT possible,
  * then ANY sum < S is also not possible.
- This monotonic behavior makes BSOA perfect.
- Why greedy checking?
  * Because to minimize the maximum subarray sum:
  * We must pack as many elements as possible into each subarray without exceeding mid.
  * If we exceed, we cut immediately.
- This greedy is always optimal.

**Complexity (Time & Space):**
- Time
- O(n log(sum(a))) because:
  * Each mid-check = O(n)
  * Binary search range log(sum)
  * You get ~30–40 iterations for integers → very fast.
- Space
  * O(1) because only counters and current sum are stored.

---

## 6. Justification / Proof of Optimality

- Binary search correct due to monotonic property of possibility function.
- Greedy splitting is optimal:
- If sum exceeds mid, we MUST split (forced cut).
- After split, best choice is to start new subarray with current element.
- Together they guarantee minimum maximum sum.
---

## 7. Variants / Follow-Ups

- Painters Partition
- Allocate minimum number of pages
- Split array to minimize max OR maximize min
- Minimize largest bag size (LeetCode “Minimum Limit of Balls”)
---

## 8. Tips & Observations

- Always identify the pattern “Minimize the Maximum”
- low = max(a[i]) ensures subarray exists
- high = sum(a[i]) ensures all numbers in one subarray possible
- Greedy split logic must never exceed mid
- Use integer binary search (no double)

- **⚠️ Pitfalls**
    - Using mid incorrectly
    - Forgetting low = max(a)
    - Splitting incorrectly (splits must be consecutive)
    - Mistaking this for DP (DP is too slow: O(n*k))
    - Missing edge case: k = n → answer = max element
    - Off-by-one in low/high answer return
---

<!-- #endregion -->
<!-- #region 159-Find row with maximum 1's -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q159: Find row with maximum 1's</h1>

## 1. Problem Understanding

- You are given a binary matrix where:
  * Each row is sorted in non-decreasing order → pattern 0 0 0 ... 1 1 1
- You must return the index of the row that has the maximum number of 1s.
- If multiple rows have same count → return the smallest index.
- If no 1 exists in the grid → return -1.
- Goal:
  * Find the row with the maximum number of ones (using sorted row property efficiently).
---

## 2. Constraints

- 1 ≤ n, m ≤ 100
- Values are 0 or 1
- All rows sorted (0’s → 1’s)
---

## 3. Edge Cases

- Entire matrix is all 0’s → return -1
- Multiple rows with same number of 1’s
- First row has max 1’s
- Only one row
- Rows like [1,1,1] or [0,0,0]
---

## 4. Examples

```text
Example 1

mat = [
 [1,1,1],
 [0,0,1],
 [0,0,0]
]
Output: 0


Example 2

mat = [
 [0,0],
 [0,0]
]
Output: -1


Example 3

mat = [
 [0,0,1],
 [0,1,1],
 [0,1,1]
]
Output: 1   (Rows 1 and 2 have 2 ones, choose smaller index)
```

---

## 5. Approaches

### Approach 1: Brute Force (Count 1's per row)

**Idea:**
- For each row:
  * Count number of 1s using simple loop.
  * Keep track of row with maximum count.

**Steps:**
- For every row i:
  * Iterate all columns.
  * Count 1s.
- Track maxCount and index.
- Return index or -1.

**Java Code:**
```java
public int rowWithMax1sBrute(int[][] mat) {
    int n = mat.length, m = mat[0].length;
    int maxCount = 0;
    int rowIndex = -1;

    for(int i = 0; i < n; i++){
        int count = 0;
        for(int j = 0; j < m; j++){
            if(mat[i][j] == 1) count++;
        }
        if(count > maxCount){
            maxCount = count;
            rowIndex = i;
        }
    }

    return (maxCount == 0 ? -1 : rowIndex);
}
```

**💭 Intuition Behind the Approach:**
- Straightforward counting, but ignores sorted property.

**Complexity (Time & Space):**
- Time:
  * O(n * m)
- Space:
  * O(1)

### Approach 2: Binary Search in Each Row

**Idea:**
- Since each row is sorted:
  * Use binary search to find first occurrence of 1
  * Ones count = m - firstIndexOfOne

**Steps:**
- For each row:
  * Do binary search for first 1
- Using count = m - index
- Track max

**Java Code:**
```java
public int rowWithMax1sBS(int[][] mat) {
    int n = mat.length, m = mat[0].length;
    int maxCount = 0;
    int rowIndex = -1;

    for(int i = 0; i < n; i++){
        int firstOne = firstOneIndex(mat[i]);
        if(firstOne != -1){
            int count = m - firstOne;
            if(count > maxCount){
                maxCount = count;
                rowIndex = i;
            }
        }
    }

    return (maxCount == 0 ? -1 : rowIndex);
}

private int firstOneIndex(int[] row){
    int low = 0, high = row.length - 1, ans = -1;
    while(low <= high){
        int mid = low + (high - low) / 2;
        if(row[mid] == 1){
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Binary search uses the sorted nature of each row.

**Complexity (Time & Space):**
- Time:
  * O(n * log m)
- Space:
  * O(1)

### Approach 3: (Optimal): Start from Top-Right Corner

**Idea:**
- Use the property:
  * Rows sorted → if you see a 1, go left
  * If you see a 0, go down
- This visits each row at most once and each column at most once → O(n + m).

**Steps:**
- Start at (0, m-1) top-right corner
- While inside grid:
  * If mat[i][j] == 1:
    * Update answer row = i
    * Move left: j--
- Else:
  * Move down: i++
- Return found row or -1.

**Java Code:**
```java
public int rowWithMax1s(int[][] mat) {
    int n = mat.length, m = mat[0].length;

    int i = 0, j = m - 1;
    int rowIndex = -1;

    while(i < n && j >= 0){
        if(mat[i][j] == 1){
            rowIndex = i;
            j--;
        } else {
            i++;
        }
    }

    return rowIndex;
}
```

**💭 Intuition Behind the Approach:**
- Moving left reduces 1s count → valid updates
- Moving down checks next rows
- Top-right traversal ensures minimal operations
- This is a classic matrix trick (“staircase search”)

**Complexity (Time & Space):**
- Time:
  * O(n + m)
- Space:
  * O(1)

---

## 6. Justification / Proof of Optimality

- Brute force checks all cells → correct but slow.
- Binary search uses sorted row property → faster.
- Top-right corner method leverages global movement pattern:
  * From top-right, every left move means more 1s
  * Every down move eliminates current row
- Guarantees minimal row index if tie happens naturally.
---

## 7. Variants / Follow-Ups

- Search in a sorted 2D matrix
- Count 1s efficiently
- First 1 in a binary row
- Row with max zeros (flip 0 and 1 logic)
---

## 8. Tips & Observations

- Sorted rows ALWAYS hint at binary search or pointer sliding.
- Top-right pointer trick is VERY common in 2D problems.
- If all rows are sorted same way, global navigation is optimal.
- Keep track of row index only when moving left on a 1.

- **⚠️ Pitfalls**
    - Return -1 if no 1 exists (corner case)
    - Returning maxCount row instead of first such row
    - Forgetting sorted property and using unnecessary loops
    - Array bounds in top-right method
---

<!-- #endregion -->
<!-- #region 160-Search in 2D matrix - II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q160: Search in 2D matrix - II</h1>

## 1. Problem Understanding

- You are given a 2D matrix where:
- Every row is sorted left → right
- Every column is sorted top → bottom
- You must determine if a given target exists in this matrix.
---

## 2. Constraints

- 1 ≤ n, m ≤ 300
- -10^9 ≤ matrix[i][j] ≤ 10^9
- Matrix sorted row-wise & column-wise
---

## 3. Edge Cases

- Target smaller than top-left element → false
- Target larger than bottom-right element → false
- Single row or single column
- Target equals first/last element
- Duplicate numbers DO NOT exist (but algorithm works even if they did)
---

## 4. Examples

```text
1   4   7   11  15
2   5   8   12  19
3   6   9   16  22
10 13  14  17  24
18 21  23  26  30
Example 1

target = 5 → true


Example 2

target = 20 → false


Example 3

target = 1 → true
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Simply check every cell.

**Steps:**
- Loop through all rows and columns.

**Java Code:**
```java
public boolean searchBrute(int[][] mat, int target) {
    for(int[] row : mat){
        for(int val : row){
            if(val == target) return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Straightforward but ignores matrix sorted structure.

**Complexity (Time & Space):**
- Time: O(n*m)
- Space: O(1)

### Approach 2: Binary Search on Each Row

**Idea:**
- Since each row is sorted:
  * For each row → do binary search.

**Steps:**
- For each row:
  * If target in range [row[0], row[m-1]], binary search.
- If found, return true.

**Java Code:**
```java
public boolean searchEachRow(int[][] mat, int target) {
    for(int[] row : mat){
        if(target >= row[0] && target <= row[row.length-1]){
            if(Arrays.binarySearch(row, target) >= 0) return true;
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Uses row sorting but ignores column sorting.

**Complexity (Time & Space):**
- Time: O(n * log m)
- Space: O(1)

### Approach 3: Staircase Search (Top-Right Corner Method)

**Idea:**
- Start at top-right corner:
  * If target < current → move left
  * If target > current → move down
  * If equal → return true
- Why this works
  * From (i, j):
  * All values left of (i, j) are smaller
  * All values below (i, j) are larger
- So you eliminate one row OR one column in each step → O(n+m).

**Steps:**
- Set:
  * i = 0
  * j = m - 1
- While in bounds:
  * if mat[i][j] == target → true
  * if target < mat[i][j] → j-- (move left)
  * else → i++ (move down)
- If loop ends → false

**Java Code:**
```java
public boolean searchMatrix(int[][] matrix, int target) {
    int n = matrix.length;
    int m = matrix[0].length;

    int i = 0, j = m - 1;

    while(i < n && j >= 0){
        if(matrix[i][j] == target){
            return true;
        }
        else if(target < matrix[i][j]){
            j--;  // move left
        }
        else{
            i++;  // move down
        }
    }

    return false;
}
```

**💭 Intuition Behind the Approach:**
- You eliminate a FULL row or FULL column depending on comparison.
- This is like searching in a BST:
  * Left side smaller
  * Down side greater
- Traversal path looks like a staircase → hence the name.

**Complexity (Time & Space):**
- Time: O(n + m)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Starting from top-right is optimal because:
  * From here, moving left → values strictly decrease
  * Moving down → values strictly increase
- Sorted rows + sorted columns → monotonic movement
- Guarantees minimal comparisons
---

## 7. Variants / Follow-Ups

- Row with maximum 1s (same top-right staircase technique)
- Search in a sorted 2D matrix where entire matrix sorted as flattened 1D
- Search a target in rotated sorted 2D arrays (hard)
- Kth smallest element in sorted matrix (heap or binary search)
---

## 8. Tips & Observations

- Always start top-right OR bottom-left for matrix sorted both ways.
- Avoid binary search per row if you need faster than O(n log m).
- Staircase search = monotonic elimination.
- The moment you move left or down, you eliminate full row/column instantly.

- **⚠️ Pitfalls**
    - Don’t start from top-left → both right & down move increases
    - Don’t use BFS/DFS → waste of time
    - Don’t treat as binary matrix or DP
    - Carefully check boundaries
---

<!-- #endregion -->
<!-- #region 161-Find Peak Element - II -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q161: Find Peak Element - II</h1>

## 1. Problem Understanding

- You are given an n x m matrix mat where:
- No two adjacent cells are equal.
- Each cell has up to 4 neighbors: left, right, top, bottom.
- A 2D peak element is one where:
  * mat[i][j] > all adjacent neighbors
  * You must return any peak coordinate [i, j].
  * The matrix is conceptually surrounded by -1 boundaries, so outer edges can also be peaks.
- Goal:
  * Find ANY peak in O(n log m) or O(m log n)
- This is the classic 2D peak finding problem.
---

## 2. Constraints

- 1 ≤ n, m ≤ 500
- 1 ≤ mat[i][j] ≤ 1e5
- Adjacent cells have different values
- Multiple peaks possible
---

## 3. Edge Cases

- Peak at a corner
- Peak in the first or last row
- Peak in the first or last column
- Entire matrix strictly increasing → bottom-right is peak
- Entire matrix strictly decreasing → top-left is peak
---

## 4. Examples

```text
Example 1

mat = [
 [10, 20, 15],
 [21, 30, 14],
 [7, 16, 32]
]
Output: [1, 1]  → 30 is a peak


Example 2

mat = [
 [10, 7],
 [11, 17]
]
Output: [1, 1]
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check every cell, compare with up to 4 neighbors.

**Steps:**
- For each (i, j):
  * Check left, right, top, bottom safely.
  * If all are smaller → return [i, j].

**Java Code:**
```java
public int[] findPeakBrute(int[][] mat){
    int n = mat.length, m = mat[0].length;

    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            int val = mat[i][j];

            boolean left  = j-1 < 0       || mat[i][j-1] < val;
            boolean right = j+1 >= m      || mat[i][j+1] < val;
            boolean up    = i-1 < 0       || mat[i-1][j] < val;
            boolean down  = i+1 >= n      || mat[i+1][j] < val;

            if(left && right && up && down){
                return new int[]{i, j};
            }
        }
    }
    return new int[]{-1, -1};
}
```

**💭 Intuition Behind the Approach:**
- We check all cells — guaranteed to find a peak, but slow.

**Complexity (Time & Space):**
- Time: O(n*m)
- Space: O(1)

### Approach 2: Row-wise Binary Search

**Idea:**
- For each row, do binary search to find element bigger than neighbors.
- But worst case = O(n log m).

**Steps:**
- For each row:
  * Binary search column mid.
  * Compare with neighbors.
- Move search space accordingly.

**💭 Intuition Behind the Approach:**
- : peak in 2D can be found by treating each row like a 1D peak problem.

**Complexity (Time & Space):**
- Time: O(n log m)
- Space: O(1)

### Approach 3: Column-wise Binary Search

**Idea:**
- We apply binary search on columns, not rows.
- For a chosen column mid:
  * Find the row where the column value is maximum.
  * Check if that cell is a peak.
  * If right neighbor is bigger → move right (peak must be on right).
  * Else → move left.
- This works because the matrix adjacency and monotonic constraints create a directional slope.

**Steps:**
- Set:
  * low = 0
  * high = m - 1
- While low ≤ high:
  * mid = (low + high) / 2
  * Find maxRow = row with maximum value in column mid
  * Let val = mat[maxRow][mid]
- Compare neighbors:
  * if left > val → peak is on left half → high = mid - 1
  * else if right > val → peak is on right half → low = mid + 1
  * else → this cell is a peak, return [maxRow, mid]
- This guarantees peak because:
  * If neighbor is larger, moving in that direction must lead to a peak.

**Java Code:**
```java
public class Solution {

    public int[] findPeakGrid(int[][] mat) {
        int n = mat.length;
        int m = mat[0].length;

        int low = 0, high = m - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // find row with max in this column
            int maxRow = 0;
            for (int i = 0; i < n; i++) {
                if (mat[i][mid] > mat[maxRow][mid]) 
                    maxRow = i;
            }

            int left  = (mid - 1 >= 0) ? mat[maxRow][mid - 1] : -1;
            int right = (mid + 1 < m) ? mat[maxRow][mid + 1] : -1;
            int curr  = mat[maxRow][mid];

            // check if peak
            if (curr > left && curr > right) {
                return new int[]{maxRow, mid};
            }
            else if (right > curr) {
                low = mid + 1;  // go right
            }
            else {
                high = mid - 1; // go left
            }
        }

        return new int[]{-1, -1};  // should never reach here
    }
}
```

**💭 Intuition Behind the Approach:**
- Why choose the max element in the column?
  * Because the best chance to find a peak is at a local maximum in that column.
- Why move left/right?
  * If right neighbor is greater, the slope leads upward → peak must exist there.
  * Same logic for left.
- This works like mountain climbing:
  * If the slope goes up, move toward the higher direction until peak is found.

**Complexity (Time & Space):**
- Time: O(n log m)
  * Each binary search iteration → O(n) to find column max
  * log m iterations of binary search
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- A peak must exist by problem guarantee.
- Searching direction based on neighbor comparison always moves toward a higher cell.
- A strictly higher neighbor ensures a peak in that direction (like 1D peak logic).
- The matrix always has at least one strict peak because adjacent cells differ.
---

## 7. Variants / Follow-Ups

- 1D peak finding
- Find peak in 2D matrix using divide and conquer (classic MIT lecture)
- Find local maxima in a grid
- Mountain matrix traversal
---

## 8. Tips & Observations

- Start binary search on columns, not rows → fewer columns than elements.
- Always pick the max element of the mid column to compare.
- No need to check top/bottom because you're already checking max.
- Outer boundary assumed -1 → natural peak on edges possible.

- **⚠️ Pitfalls**
    - Forgetting to check matrix boundaries
    - Picking ANY element instead of column max
    - Checking only one neighbor
    - Using DFS/BFS (not needed)
    - Assuming sorted matrix (it is NOT sorted)
    - Confusing with "Search in 2D matrix" problem — totally different
---

<!-- #endregion -->
<!-- #region 162-Matrix Median -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q162: Matrix Median</h1>

## 1. Problem Understanding

- You are given a matrix where:
- Each row is sorted individually.
- The matrix is not globally sorted.
- Total number of elements (N * M) is odd.
- You must find the median of all elements if collected in a sorted list.
---

## 2. Constraints

- 1 ≤ N, M ≤ 10^5
- 1 ≤ N*M ≤ 10^6 (so total elements up to 1 million)
- Each row sorted
- 1 ≤ matrix[i][j] ≤ 10^9
- N*M is always odd → median is a single element
- You CANNOT flatten → too slow/memory heavy
---

## 3. Edge Cases

- N = 1 → median is median of single array
- Rows of different sizes (not here; all have M columns)
- All small or all large values
- Duplicate numbers
- Matrix values not globally sorted
---

## 4. Examples

```text
Example 1

Input:
[1 4 9]
[2 5 6]
[3 7 8]

Median = 5


Example 2

[1 3 8]
[2 3 4]
[1 2 5]

Flatten: 1 1 2 2 3 3 4 5 8 → median = 3
```

---

## 5. Approaches

### Approach 1: Flatten & Sort (Brute Force)

**Idea:**
- Create an array of size N*M
- Copy all elements
- Sort it
- Return middle element

**💭 Intuition Behind the Approach:**
- Works but wasteful.

**Complexity (Time & Space):**
- Time: O(N*M log(N*M))
- Space: O(N*M)

### Approach 2: Min-Heap / K-way merge

**Idea:**
- Use a min-heap to merge rows (each sorted)
- Pop (N*M)/2 elements
- Return next element

**Complexity (Time & Space):**
- Time: O((N*M) log N)
- Space: O(N)
- Still too slow for 10^6 elements.

### Approach 3: Optimal): Binary Search on Answer

**Idea:**
- Since each row is sorted, we can count:
  * How many elements in matrix are ≤ mid?
- If we can count that efficiently, we can binary search the value of the median, not position.
- Median position (0-indexed):
  * k = (N*M)/2
- We need the smallest value x such that:
  * countLessOrEqual(x) > k
- Key Insight
- Every row sorted → count elements ≤ mid in a row using binary search.

**Steps:**
- Compute:
  * low = min element (first elements of each row)
  * high = max element (last elements of each row)
- While low <= high:
  * mid = low + (high - low) / 2
  * count = total elements ≤ mid across all rows
  * If count > k → median might be mid → move left
  * Else → move right
- Return low (the smallest element satisfying count > k)

**Java Code:**
```java
public class Solution {

    public int findMedian(int[][] mat) {
        int n = mat.length;
        int m = mat[0].length;
        int low = Integer.MAX_VALUE, high = Integer.MIN_VALUE;

        // find global min and max
        for(int i = 0; i < n; i++){
            low = Math.min(low, mat[i][0]);
            high = Math.max(high, mat[i][m - 1]);
        }

        int k = (n * m) / 2; // median index

        while(low <= high){
            int mid = low + (high - low) / 2;

            int count = 0;

            // count elements <= mid in each row
            for(int i = 0; i < n; i++){
                count += upperBound(mat[i], mid);
            }

            if(count > k){
                high = mid - 1;
            }
            else{
                low = mid + 1;
            }
        }

        return low;
    }

    // number of elements <= x in sorted array
    private int upperBound(int[] row, int x){
        int low = 0, high = row.length - 1, ans = row.length;

        while(low <= high){
            int mid = low + (high - low) / 2;
            if(row[mid] > x){
                ans = mid;
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }
}
```

**💭 Intuition Behind the Approach:**
- Median value is the K-th smallest element.
- Instead of merging arrays, we binary search over the value range.
- Each mid represents a candidate value for median.
- Using binary search in each row (sorted), counting is efficient.
- Because the matrix isn’t globally sorted, but each row is, you can treat each row independently during count.
- This is a classic BSOA pattern.

**Complexity (Time & Space):**
- Time:
  * Counting each mid: O(N * log M)
  * Binary search over value range → about 32 iterations (log of 1e9)
  * Total: O(N * log M * log(1e9))
  * For constraints (N*M ≤ 10^6), this is optimal.
- Space:
  * O(1)

---

## 6. Justification / Proof of Optimality

- Each row sorted → count elements ≤ x using binary search.
- Counting is monotonic → if mid is large, count increases.
- Because global sorted structure is not present, you cannot do one binary search on a flattened index.
- BSOA is the only scalable method.
---

## 7. Variants / Follow-Ups

- Median of row-wise sorted matrix (same problem)
- Kth smallest element in a sorted matrix
- Kth smallest in sorted lists
- Find first element greater than X in matrix
- Median in a stream (heaps)
---

## 8. Tips & Observations

- Always binary search on value, not index, when multi-row sorted only row-wise.
- Use upperBound to count ≤ mid.
- Lowest possible matrix element → low
- Highest possible → high
- Final answer = low
- Total length is odd → no need to average two middle values.

- **⚠️ Pitfalls**
    - Using flatten or heap → TLE
    - Using full matrix min & max incorrectly
    - Mistaking for 2D sorted full matrix (it's not)
    - Confusing median index condition
    - Using lowerBound instead of upperBound incorrectly
---

<!-- #endregion -->
<!-- #region 163-Minimum time to make Cakes -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q163: Minimum time to make Cakes</h1>

## 1. Problem Understanding

- You are given:
  * An array A of size N, where
  * A[i] = day on which the i-th flavour becomes available
  * M = number of cakes needed
  * K = each cake requires K consecutive flavours
  * Each flavour can be used in only 1 cake
  * You must find the minimum day D such that it is possible to form M cakes using K consecutive available flavours.
- If not possible → print -1.
---

## 2. Constraints

  * 1 ≤ T ≤ 10
  * 3 ≤ N ≤ 1000
  * 3 ≤ M ≤ 10000
  * 1 ≤ K ≤ N
  * 0 ≤ A[i] ≤ 10000
- Total flavours = N
- Total cakes = M
- Cake requires K consecutive flavours.
---

## 3. Edge Cases

- M * K > N → impossible → return -1
- All A[i] = 0 → you can make all cakes immediately if count allows
- One giant unavailable segment makes arrangement impossible
- K = 1 reduces to counting how many elements ≤ D
- Flavours become available very late → return max(A[i])
---

## 4. Examples

```text
Example 1
N=5, M=3, K=1
A = [1,10,3,10,2]

Day 1: only A[0] available → 1 cake  
Day 2: A[0], A[4] → 2 cakes  
Day 3: A[0], A[2], A[4] → 3 cakes
Answer = 3

Example 2
N=5, M=1, K=2
A = [1,10,3,10,2]

Need 1 cake = 2 consecutive available flavours.

Day 10: all flavours available → can form 1 cake.
Answer = 10
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try each day from 0 → max(A):
  * For each day D, check if at least M cakes possible.
  * For each D:
    * Create boolean array available[i] = (A[i] ≤ D)
    * Count consecutive sequences of length K

**💭 Intuition Behind the Approach:**
- Simulate everything day by day.

**Complexity (Time & Space):**
- Time: O(max(A) * N) → too big (10k * 1000 = 1e7 per test)
- Space: O(N)

### Approach 2: Binary Search on Answer

**Idea:**
- Binary search on minimum day D such that it is possible to make M cakes.
- For a given D:
  * Mark flavour i as available if A[i] ≤ D
  * Traverse array to count how many consecutive blocks of size K can be formed.
- If cakes ≥ M → day D is possible → try smaller
- Else → increase day
- This is same pattern as:
  * Minimum days to make bouquets
  * Minimum time to make m bananas
  * Minimum time to make m chocolates

**Steps:**
- Compute:
  * low = min(A)
  * high = max(A)
- Binary search on days:
  * mid = (low + high) / 2
- Check if M cakes can be formed on day mid.
- Adjust search range.

**Java Code:**
```java
class Solution {

    public int solve(int[] A, int M, int K) {
        int n = A.length;

        // If total flavours < required flavours → impossible
        if ((long)M * K > n) return -1;

        int low = Integer.MAX_VALUE, high = Integer.MIN_VALUE;

        for (int x : A) {
            low = Math.min(low, x);
            high = Math.max(high, x);
        }

        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canMake(A, M, K, mid)) {
                ans = mid;
                high = mid - 1;       // try earlier day
            } else {
                low = mid + 1;        // need more days
            }
        }

        return ans;
    }

    private boolean canMake(int[] A, int M, int K, int day) {
        int consecutive = 0;
        int cakes = 0;

        for (int x : A) {
            if (x <= day) {         // flavour available
                consecutive++;
                if (consecutive == K) {
                    cakes++;
                    consecutive = 0;  // K consecutive used → reset
                    if (cakes >= M) return true;
                }
            } else {
                consecutive = 0;     // break the block
            }
        }

        return cakes >= M;
    }
}
```

**💭 Intuition Behind the Approach:**
- If by day D we can form M cakes:
  * Any day ≥ D is also possible.
- If day D is not enough:
  * Any day < D will also not be enough.
- This monotonic behavior → perfect for binary search.
- We search the SMALLEST day that satisfies the requirement.
- Counting consecutive blocks is greedy:
  * Each block of K consecutive available flavours = 1 cake
  * Reset after using K flavours (each flavour used once)

**Complexity (Time & Space):**
- Time:
  * Binary search iterations: log(max(A)) ≈ log(10000) ≈ 14
  * Each check: O(N)
  * Total: O(N log Amax) ≈ 1000 * 14 = 14000 operations per test
- Space:
  * O(1)

---

## 6. Justification / Proof of Optimality

- This is the classic Minimum Days to Make Bouquets problem with:
  * Bouquet size = K
  * Number of bouquets = M
  * Flower available on day A[i]
- Here:
  * Flavour available on day A[i]
  * Cake requires K consecutive flavours
  * Need M cakes
- Perfect mapping, so same greedy + BSOA logic works.
---

## 7. Variants / Follow-Ups

- Minimum days to make bouquets
- Minimum time to complete m tasks with cooldown
- Create k consecutive groups
- Allocate painters to boards (variant structure)
---

## 8. Tips & Observations

- Always check M*K > N early → impossible
- The key is consecutive availability
- Use greedy scanning to form groups
- No need to build boolean array, just compare A[i] ≤ day
- Low = min(A), high = max(A)
- Always binary search on answer, not index

- **⚠️ Pitfalls**
    - Forgetting that each flavour can be used ONCE
    - Forgetting to reset when block breaks
    - Doing sliding window incorrectly (blocks must be consecutive, not ANY K available)
    - Using sum or counting A[i] ≤ mid — DOES NOT work because flavours must be consecutive
    - Off-by-one in binary search
---

<!-- #endregion -->
<!-- #region 164-Minimum Limit of Balls in a Bag -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q164: Minimum Limit of Balls in a Bag</h1>

## 1. Problem Understanding

- You are given:
  * nums[i] = number of balls in the i-th bag
  * maxOperations = number of times you can split a bag
- Operation allowed:
  * Pick any bag with x balls
  * Split into two bags: a and b where a + b = x, a > 0, b > 0
- Your penalty = maximum number of balls among all bags.
- Goal:
  * Minimize the penalty after doing at most maxOperations splits.
  * Return the minimum possible penalty.
---

## 2. Constraints

- 1 ≤ n ≤ 1e5
- 1 ≤ nums[i] ≤ 1e9
- 1 ≤ maxOperations ≤ 1e9
---

## 3. Edge Cases

- maxOperations = 0 → penalty = max(nums)
- Already small numbers → answer = max(nums)
- Extremely large bag (like 1e9) → binary search required
- Many bags with small values → operations not needed
- Many operations → can make all bags = 1
---

## 4. Examples

```text
Example 1
nums = [9], maxOperations = 2
Possible final: [3,3,3] → penalty = 3

Example 2
nums = [2,4,8,2], maxOperations = 4
Final: [2,2,2,2,2,2,2,2] → penalty = 2
```

---

## 5. Approaches

### Approach 1: Simulation (Brute Force)

**Idea:**
- Split largest bag repeatedly.
- Use a max heap.

**💭 Intuition Behind the Approach:**
- Feels greedy: keep splitting largest bag.
- ❌ Why it fails / too slow
    * Splitting 1e9 can take too many operations because:
    * heap operations cost O(log n)
    * up to maxOperations = 1e9 → impossible

**Complexity (Time & Space):**
- Time: O(maxOperations * log n) → TLE
- Space: O(n)

### Approach 2: Binary Search on Answer

**Idea:**
- Key Insight
- Let penalty = X (the maximum allowed balls in any bag).
- Then we ask:
- ❓ How many splits needed to make every bag ≤ X?
- For a bag with size v:
- We need:
  * splits = (v - 1) / X
- Explanation:
  * Example: v = 9, X = 3
  * (9 - 1) / 3 = 8 / 3 = 2 splits → which is correct: 9 → 6+3 → 3+3+3
- We sum splits for all bags:
  * totalSplits = Σ (v - 1) / X
- Then:
  * if totalSplits <= maxOperations
       * X is possible → try smaller X
  * else
       * X too small → try bigger X
- This is monotonic:
  * If penalty X is possible → any X’ > X is also possible
  * If X is not possible → any X’ < X is also not possible
- → Perfect for binary search.

**Steps:**
- low = 1
- high = max(nums)
- While low ≤ high:
  * mid = possible penalty
  * compute splits needed
  * if splits ≤ maxOperations → high = mid - 1
  * else → low = mid + 1
- Answer = low

**Java Code:**
```java
class Solution {
    public int minimumSize(int[] nums, int maxOperations) {
        int low = 1;
        int high = 0;

        for (int x : nums) 
            high = Math.max(high, x);

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canMake(nums, maxOperations, mid)) {
                high = mid - 1;   // try smaller penalty
            } else {
                low = mid + 1;    // need bigger penalty
            }
        }

        return low;
    }

    private boolean canMake(int[] nums, int maxOperations, int limit) {
        long ops = 0;

        for (int x : nums) {
            if (x > limit) {
                ops += (x - 1) / limit;
                if (ops > maxOperations) return false;
            }
        }
        return true;
    }
}
```

**💭 Intuition Behind the Approach:**
- If penalty = X is allowed, each bag of size v becomes roughly v/X bags.
- Number of splits needed is (v - 1)/X.
- Lower penalty → more splits → harder.
- Higher penalty → fewer splits → easier.
- So:
  * Penalty too small → too many splits required → infeasible
  * Penalty large enough → splits fit within maxOperations → feasible
- This monotonic property → Binary Search on Answer.
- Same idea as:
  * Gas stations problem
  * Split array largest sum
  * Minimum days to make m bouquets
  * Aggressive cows (maximize minimum distance)
  * Koko eating bananas

**Complexity (Time & Space):**
- ⏱️ Time Complexity
- Each check: O(n)
- Binary search: O(log max(nums)) ≈ 31
- Total:
  * O(n log max(nums))
  * ✔ Fits constraints: n = 1e5
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- If we fix a penalty X, the number of splits needed for each bag is:
  * splits = (v - 1) / X
- As X decreases (smaller penalty):
  * Required splits increase
  * Harder to achieve within maxOperations
- As X increases (larger penalty):
  * Required splits decrease
  * Easier to achieve within maxOperations
- Thus:
  * If penalty X is feasible → any value > X is also feasible
  * If penalty X is not feasible → any value < X is also not feasible
- This monotonic feasibility allows a binary search on the answer.
- No other approach can beat this time complexity due to range (1 to 1e9) and constraints.
---

## 7. Variants / Follow-Ups

- Minimum Time to Make m Bouquets
- Minimize maximum array sum after operations
- Split Bags problem (same)
- Koko Eating Bananas (penalty = hours)
- Minimum limit of chocolates in a box
- Gas stations split + minimize max gap
---

## 8. Tips & Observations

- Whenever an operation divides something → BSOA appears
- Splitting until all ≤ X uses formula (v-1)/X
- Lower penalty = harder = more operations
- Higher penalty = easier = fewer operations

- **⚠️ Pitfalls**
    - Using greedy splits on heap → TLE
    - Overflow if using int for ops (use long)
    - Wrong split formula:
      * ❌ v / X
      * ✔ splits = (v - 1) / X
    - Returning high instead of low—use low at the end
    - Missing binary search on penalty logic (not on indices)
---

<!-- #endregion -->
