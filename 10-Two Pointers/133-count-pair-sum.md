<!-- #region 133-Count Pair Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q133: Count Pair Sum</h1>

## 1. Problem Understanding

- You are given two sorted, distinct-element arrays arr1 (size m) and arr2 (size n).
- You must count how many pairs (a from arr1, b from arr2) satisfy:
  * a + b == x
- Return only count, not the pairs.
---

## 2. Constraints

- 1 <= m, n <= 5 * 10^4
- Arrays are sorted
- Elements are distinct inside each array
- 1 <= x <= 2 * 10^5
- Overall operations must ideally be O(m + n) or O(m log n).
---

## 3. Edge Cases

- All values smaller than x → result may be 0.
- All values greater than x → result may be 0.
- All pairs valid only once (distinct arrays guarantee no duplicates in each array).
- Very large arrays → brute force will TLE.
---

## 4. Examples

```text
Example 1:
arr1 = [1,2,4,5]
arr2 = [3,5,7,8]
x = 9
Pairs → (1,8), (2,7), (4,5) → count = 3.

Example 2:
arr1 = [1,2,3]
arr2 = [4,5,6,7,8]
x = 8
Pairs → (1,7), (2,6), (3,5) → count = 3.
```

---

## 5. Approaches

### Approach 1: Brute Force (Nested Loops) — O(m * n)

**Idea:**
- Check all m*n pairs and count those with sum x.

**Java Code:**
```java
public static int countPairs(int[] arr1, int[] arr2, int x) {
    int count = 0;
    for (int a : arr1) {
        for (int b : arr2) {
            if (a + b == x) count++;
        }
    }
    return count;
}
```

**💭 Intuition Behind the Approach:**
- Straightforward brute-force checking of all possibilities.
- Simple to write, but too slow for large inputs.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(m * n)
  * Why: Each element of arr1 is paired with each element of arr2.
- 💾 Space Complexity
  * O(1).

### Approach 2: Binary Search on Second Array — O(m log n)

**Idea:**
- For each a from arr1:
  * Look for x - a in arr2 using binary search.

**Java Code:**
```java
public static int countPairs(int[] arr1, int[] arr2, int x) {
    int count = 0;
    for (int a : arr1) {
        int target = x - a;
        int lo = 0, hi = arr2.length - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (arr2[mid] == target) {
                count++;
                break;
            } else if (arr2[mid] < target) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
    }
    return count;
}
```

**💭 Intuition Behind the Approach:**
- Since arr2 is sorted, binary search helps locate the matching pair in log n time.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(m log n)
  * Why: For each of m elements, binary search takes log n.
- 💾 Space Complexity
  * O(1).

### Approach 3: HashSet Approach — O(m + n)  (Better than binary search if arrays weren't sorted)

**Idea:**
- Store all elements of arr2 in a HashSet, then for each a check if (x - a) exists.

**Java Code:**
```java
public static int countPairs(int[] arr1, int[] arr2, int x) {
    HashSet<Integer> set = new HashSet<>();
    for (int b : arr2) set.add(b);

    int count = 0;
    for (int a : arr1) {
        if (set.contains(x - a)) count++;
    }
    return count;
}
```

**💭 Intuition Behind the Approach:**
- Hashing gives O(1) lookup for each pair check.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(m + n)
  * Why: Build hash set → O(n), loop arr1 → O(m).
- 💾 Space Complexity
  * O(n) for the hash set.

### Approach 4: Two Pointers (Optimal) — O(m + n)

**Idea:**
- Since both arrays are sorted:
  * Use pointer i = 0 on arr1 (smallest values)
  * Use pointer j = n-1 on arr2 (largest values)
- Move pointers based on arr1[i] + arr2[j]:
  * If sum == x → count++, move both
  * If sum < x → need bigger sum → i++
  * If sum > x → need smaller sum → j--

**Java Code:**
```java
public static int countPairs(int[] arr1, int[] arr2, int x) {
    int i = 0;
    int j = arr2.length - 1;
    int count = 0;

    while (i < arr1.length && j >= 0) {
        int sum = arr1[i] + arr2[j];

        if (sum == x) {
            count++;
            i++;
            j--;
        } else if (sum < x) {
            i++;    // need larger sum
        } else {
            j--;    // need smaller sum
        }
    }
    return count;
}
```

**💭 Intuition Behind the Approach:**
- Sorted arrays allow a linear scan from opposite ends.
- You always eliminate one pointer depending on sum, guaranteeing no missed pairs.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(m + n)
  * Why: Each pointer moves monotonically across its array once.
- 💾 Space Complexity
  * O(1).

---

## 6. Justification / Proof of Optimality

- Fully uses sorted property.
- Inspects each element at most once.
- Eliminates impossible sums quickly.
- Best complexity achievable under given constraints.
- Thus Two-Pointer is the optimal approach.
---

## 7. Variants / Follow-Ups

- Return the pairs instead of count.
- Arrays unsorted → sort + two pointers OR use hashset.
- Duplicate elements present → handle frequency counts.
- K arrays sum → similar expansion as K-sum problems.
---

## 8. Tips & Observations

- When both arrays sorted → two-pointer is usually optimal for sum problems.
- Hashing is best when arrays not sorted.
- Avoid brute force for n = 50,000.
---

<!-- #endregion -->
