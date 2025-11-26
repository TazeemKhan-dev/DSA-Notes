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
- Same concept, but keep track of best candidate using ans.

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
