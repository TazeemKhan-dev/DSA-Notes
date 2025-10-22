<!-- #region 67-Array Swaps -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q67: Array Swaps</h1>

## 1. Problem Understanding

- You’re given:
  * An array A of size N.
  * A number X.
- You can swap elements at indices i and j only if
- |i - j| ≥ X.
- Your task is to determine whether it’s possible to sort the array in non-decreasing order using such operations (possibly zero).
- Goal:
- Return "YES" if sorting is possible under this constraint, otherwise "NO".
---

## 2. Constraints

- 1 ≤ X ≤ N ≤ 10^5
- 1 ≤ A[i] ≤ 10^9
- Large N → must be O(N log N) or better.
- The operation constraint limits which indices can interact.
---

## 3. Edge Cases

- X = 1 → Can swap any two indices → always "YES".
- X = N → No swaps possible → "YES" only if already sorted.
- Array already sorted → "YES", regardless of X.
- X > N/2 → Restricted swaps; some positions can’t move freely.
- Duplicate values should not affect the logic.
---

## 4. Examples

```text
Example 1
Input:
3 3
3 2 1
Output:
NO
Explanation:
|i-j| ≥ 3 → no valid swap possible.
Since array isn’t sorted → "NO".

Example 2
Input:
5 2
5 1 2 3 4
Output:
YES
Explanation:
You can swap elements with at least distance 2 apart,
which allows rearranging the array to [1, 2, 3, 4, 5].
```

---

## 5. Approaches

### Approach 1: Observation-Based Logic

**Idea:**
- Depending on X, check how “free” the array elements are to move.
  * When X ≤ N/2:
  * Enough overlapping reachable indices → can rearrange freely → always YES.
  * When X > N/2:
  * Some edges can’t move to the middle → must already be in correct positions.

**Steps:**
- If X <= N/2: print "YES".
- Else:
  * Copy and sort the array → sortedA.
  * For each index i that cannot be moved freely,
  * i.e. i < X or i > N - X + 1,
  * check if A[i] == sortedA[i].
- If all fixed positions match → "YES", else "NO".

**Java Code:**
```java
public class Solution {
    public static String canBeSorted(int N, int X, int[] A) {
        int[] sorted = A.clone();
        Arrays.sort(sorted);

        // If X <= N / 2, we can always sort
        if (X <= N / 2) return "YES";

        // Otherwise, only check the middle unaffected region
        for (int i = N - X; i < X; i++) {
            if (i >= 0 && i < N && A[i] != sorted[i]) {
                return "NO";
            }
        }
        return "YES";
    }
}
```

**Complexity (Time & Space):**
- Sorting: O(N log N)
- Checking: O(N)
- Total: O(N log N)
- Space: O(N) (for sorted array)

---

## 6. Justification / Proof of Optimality

- When X ≤ N/2, segments overlap → full rearrangement possible → always YES.
- When X > N/2, edge elements are fixed because they can’t reach beyond |i−j| ≥ X →
- So, only sort possible if fixed parts are already in place.
- This ensures all constraints are respected while still determining sortability.
---

## 7. Variants / Follow-Ups

- Restricted Swap Distance in Other Forms:
  * |i−j| = X (exact distance swaps only)
  * |i−j| ≤ X (limited local swaps)
- Multi-Segment Sorting Problems:
- Where you can only swap inside certain groups or segments.
- Graph Formulation Variant:
- Each index forms a node, and swap rules define edges — check if all nodes form a connected component to allow full reordering.
---

## 8. Tips & Observations

- X <= N/2 → Always “YES” — memorize this shortcut ⚡
- Only when X > N/2, we need to compare the edge parts with the sorted version.
- Be careful with 1-based vs 0-based indexing in implementation.
- This problem teaches:
  * How constraints affect sorting ability.
  * How to derive logical shortcuts using symmetry and range reasoning.
- Commonly seen in Codeforces / CodeChef challenges — tests reasoning + array manipulation skills.
---

<!-- #endregion -->
