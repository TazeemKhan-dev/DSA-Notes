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
