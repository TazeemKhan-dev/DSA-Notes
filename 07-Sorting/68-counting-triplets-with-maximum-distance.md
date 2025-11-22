<!-- #region 68-Counting Triplets With Maximum Distance -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q68: Counting Triplets With Maximum Distance</h1>

## 1. Problem Understanding

- You are given:
- An integer N → number of points.
- An array points[] of integers → positions on a number line.
- An integer L → maximum allowed distance.
- You must count the total number of triplets (i, j, k)
- such that the distance between the farthest two points in the triplet
- is ≤ L, i.e.
  * points[k] - points[i] ≤ L (after sorting).
---

## 2. Constraints

- 1 ≤ N ≤ 100
- 0 ≤ points[i] ≤ 1000
- 1 ≤ L ≤ 500
- Time complexity up to O(N²) is acceptable.
---

## 3. Edge Cases

- If N < 3 → No triplets possible → return 0.
- All points the same → all triplets valid.
- Very large L → all possible triplets valid.
- Very small L (e.g., 0) → only triplets with same value valid.
---

## 4. Examples

```text
Example 1:
Input
4
2 1 3 4
3
Output
4
Explanation
Sorted array → [1, 2, 3, 4]
Valid triplets (max - min ≤ 3):
{1,2,3}, {1,2,4}, {1,3,4}, {2,3,4}

Example 2:
Input
4
2 1 3 4
2
Output
2
Explanation
Sorted array → [1, 2, 3, 4]
Valid triplets:
{1,2,3}, {2,3,4}
```

---

## 5. Approaches

### Approach 1: Brute Force (O(N³))

**Idea:**
- Check every triplet (i, j, k) and see if max - min ≤ L.

**Steps:**
- Sort the array.
- Loop through all triplets.
- Check condition and count if valid.

**Java Code:**
```java
public static int countTripletsBruteForce(int n, int[] arr, int L) {
    Arrays.sort(arr);
    int count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                if (arr[k] - arr[i] <= L) count++;
            }
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N³)
- Space: O(1)

### Approach 2: Two Pointers / Sliding Window (O(N²)) ✅ (Optimized)

**Idea:**
- For each starting index i, find the farthest index j
- such that arr[j] - arr[i] ≤ L.
- All elements between them can form valid triplets.

**Steps:**
- Sort the array.
- Fix i (start).
- Move j until arr[j] - arr[i] > L.
- If there are count = j - i points in range,
  * → triplets = (count - 1) * (count - 2) / 2.

**Java Code:**
```java
public static int countTriplets(int n, int[] arr, int L) {
    Arrays.sort(arr);
    int count = 0;

    for (int i = 0; i < n; i++) {
        int j = i;
        while (j < n && arr[j] - arr[i] <= L) {
            j++;
        }
        int totalPoints = j - i;
        if (totalPoints >= 3) {
            count += (totalPoints - 1) * (totalPoints - 2) / 2;
        }
    }

    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N²)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Sorting ensures the difference arr[j] - arr[i] is monotonic,
- allowing an efficient window check.
- The combination formula counts all possible triplets efficiently.
- This approach avoids unnecessary triplet enumeration.
---

## 7. Variants / Follow-Ups

- Counting Pairs instead of Triplets → similar approach but use count - 1.
- K-sized subsets within a max distance → use combinatorics formula C(count - 1, k - 1).
- 2D or 3D coordinates → use distance formula instead of subtraction.
---

## 8. Tips & Observations

- Always sort before applying difference-based logic.
- When you fix one point and slide another pointer,
- you often reduce nested loops → O(N²) → O(N).
- Combinatorial counting (nC2, nC3) is a key trick in such problems.
- For C(n, 3), formula = n*(n-1)*(n-2)/6.
---

<!-- #endregion -->
