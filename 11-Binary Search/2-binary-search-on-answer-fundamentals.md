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
