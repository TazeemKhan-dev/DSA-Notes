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
