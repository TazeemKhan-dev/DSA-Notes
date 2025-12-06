<!-- #region 175-Find Hinged Element -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q175: Find Hinged Element</h1>

## 1. Problem Statement

- Given an array arr of length N, find an index i such that:
  * All elements before i are strictly smaller than arr[i]
  * All elements after i are strictly greater than arr[i]
- Return the 0-based index if such an element exists, otherwise return -1.
- Notes:
  * The first element can never be a hinged element.
  * If the rightmost element is the largest element, then it can be the hinged element (only if all before it are smaller).
---

## 2. Problem Understanding

- We need an element that sits like a “hinge”:
  * left side: strictly smaller
  * current element
  * right side: strictly greater
- This is essentially:
  * For index i,
    * max(left[0..i-1]) < arr[i] < min(right[i+1..end])
- You cannot check this for all i in brute-force (O(N^2)) because N can be 100000.
- We must preprocess:
  * Left max array
  * Right min array
- Then find index satisfying the condition.
---

## 3. Constraints

- 0 <= N <= 100000
- 0 <= arr[i] <= 10000
- Must run in O(N) or O(N) memory
---

## 4. Edge Cases

- Array length < 3 → always return -1
- (because first cannot be hinge, and last must be > everything else)
- Duplicate values break strict comparison
- All increasing array → last element is hinged
- All decreasing array → no hinge
---

## 5. Examples

```text
Example 1
Input:
9
5 1 4 3 6 8 10 7 9

Output:
4


At index 4 → 6:
Left: 5,1,4,3 (all < 6)
Right: 8,10,7,9 (all > 6)

Example 2
Input:
4
5 1 4 4

Output:
-1


No valid hinge.
```

---

## 6. Approaches

### Approach 1: Prefix-Max + Suffix-Min (Optimal)

**Idea:**
- Precompute:
  * leftMax[i] = max element from 0 → i
  * rightMin[i] = min element from i → end
- For index i to be hinged:
  * leftMax[i-1] < arr[i] < rightMin[i+1]

**Steps:**
- Build leftMax[]
- Build rightMin[]
- Iterate from 1 to n-2:
  * Check if leftMax[i-1] < arr[i] < rightMin[i+1]
- Special case: last index — if arr[n-1] > leftMax[n-2], it’s hinged.

**Java Code:**
```java
public int findElement(int[] arr, int n) {

    // Special case: N = 1 -> return 0
    if (n == 1) return 0;

    int[] leftMax = new int[n];
    int[] rightMin = new int[n];

    leftMax[0] = arr[0];
    for (int i = 1; i < n; i++) {
        leftMax[i] = Math.max(leftMax[i - 1], arr[i]);
    }

    rightMin[n - 1] = arr[n - 1];
    for (int i = n - 2; i >= 0; i--) {
        rightMin[i] = Math.min(rightMin[i + 1], arr[i]);
    }

    // MUST check last element FIRST (as per problem statement)
    if (arr[n - 1] > leftMax[n - 2]) return n - 1;

    // Now check internal elements
    for (int i = 1; i < n - 1; i++) {
        if (leftMax[i - 1] < arr[i] && arr[i] < rightMin[i + 1]) {
            return i;
        }
    }

    return -1;
}

```

**💭 Intuition Behind the Approach:**
- We avoid re-checking left and right sides by precomputing:
- Maximum left element up to each index
- Minimum right element from each index
- This allows O(1) condition check per index.

**Complexity (Time & Space):**
- Time: O(N)
  * One pass for leftMax, one for rightMin, one for answer.
- Space: O(N)

### Approach 2: Optimized Memory Using Single Pass (Tricky)

**Idea:**
- Skip prefix arrays and compute rightMin on the fly using a second pass.
- Not worth the complexity; stick to Approach 1.

**Complexity (Time & Space):**
- Still O(N) time
- O(1) space (but implementation gets messy)

---

## 7. Justification / Proof of Optimality

- Prefix-max + suffix-min is the industry-standard way to solve hinge/pivot/index-middle problems cleanly and reliably.
---

## 8. Variants / Follow-Ups

- Find all hinged elements instead of one.
- Find pivot where left sum equals right sum.
- Find element greater than all to its left and smaller than all to its right.
---

## 9. Tips & Observations

- Strict inequality matters → duplicates break hinge.
- First element can't be hinge because no left values.
- Last element can be hinge if it’s the largest overall.
---

## 10. Pitfalls

- Forgetting strict comparison: use <, not <=
- Missing last element special condition
- Arrays of length < 3 should return -1
---

<!-- #endregion -->
