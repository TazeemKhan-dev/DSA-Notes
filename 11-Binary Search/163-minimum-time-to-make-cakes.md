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
