<!-- #region 176-Count Number of Pairs With Absolute Difference K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q176: Count Number of Pairs With Absolute Difference K</h1>

## 1. Problem Statement

- You are given an integer array A of size n and a non-negative integer k.
- You must count distinct unordered pairs (A[i], A[j]) such that:
  * |A[i] - A[j]| = k   AND   i != j
- Unordered pair means:
  * (1, 2) is the same as (2, 1)
  * Count it once
- You must return the count of distinct pairs, not total occurrences.
---

## 2. Problem Understanding

- We only want unique value pairs, NOT index-based pairs.
- For example:
  * arr = [1, 2, 2, 1], k = 1
  * Valid pair value = (1, 2)
  * Even though many index pairs exist, we count only ONCE.
- Two cases:
- Case 1: k > 0
  * Pair values must satisfy:
    * A[j] = A[i] + k
  * So we can use a HashSet.
- Case 2: k == 0
  * We count distinct values that appear at least twice.
    * Example:
    * arr = [1, 5, 4, 1, 2], k = 0
    * Only value 1 appears twice → answer = 1
---

## 3. Constraints

- 1 ≤ n ≤ 100000
- 0 ≤ k ≤ 100000 (given as possibly negative, but logically k must be non-negative)
- 1 ≤ A[i] ≤ 100000
---

## 4. Edge Cases

- k = 0 → count duplicates only
- Repeated elements → must count only one unordered pair
- No valid pair → answer = 0
- Large n requires O(n) or O(n log n) solution
---

## 5. Examples

```text
Example 1

Input:

5 0
1 5 4 1 2


Output:

1


Because only (1,1) is valid.

Example 2

Input:

4 1
1 2 2 1


Output:

1


Because only (1,2) is valid.
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Pairs)

**Idea:**
- Try every pair (i,j) and check if:
  * |A[i] - A[j]| = k
  * store pair in a set as (min, max) to avoid duplicates.

**Steps:**
- Loop i from 0 to n-1
- Loop j from i+1 to n-1
- If difference = k → store pair in a HashSet
- Count size of set

**Java Code:**
```java
public int countPairsBrute(int[] A, int k) {
    HashSet<String> set = new HashSet<>();

    for (int i = 0; i < A.length; i++) {
        for (int j = i + 1; j < A.length; j++) {
            if (Math.abs(A[i] - A[j]) == k) {
                int a = Math.min(A[i], A[j]);
                int b = Math.max(A[i], A[j]);
                set.add(a + "," + b);
            }
        }
    }

    return set.size();
}
```

**💭 Intuition Behind the Approach:**
- Check everything → guaranteed correct but too slow.

**Complexity (Time & Space):**
- Time: O(n^2)
- Space: O(n) for storing unique pairs

### Approach 2: Sorting + Two Pointer (Better, O(n log n))

**Idea:**
- If array is sorted, then:
  * For each i, search for value A[i] + k using:
    * Two pointers OR
    * Binary search
- Store pair only once.

**Steps:**
- Sort array
- Use pointer i and pointer j
- If A[j] - A[i] == k → count pair, move both
- If smaller → move j
- If larger → move i
- Also skip duplicates.

**Java Code:**
```java
public int countPairsTwoPointer(int[] A, int k) {
    Arrays.sort(A);
    int i = 0, j = 1, count = 0;

    while (j < A.length) {
        if (i == j) { j++; continue; }

        int diff = A[j] - A[i];

        if (diff == k) {
            count++;
            int val = A[i];
            // Skip duplicates
            while (i < A.length && A[i] == val) i++;
        } 
        else if (diff < k) j++;
        else i++;
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Sorting organizes numbers, making difference checks easier and duplicate skipping possible.

**Complexity (Time & Space):**
- Time: O(n log n)
- Space: O(1) or O(n) depending on sort

### Approach 3: HashMap + HashSet (Optimal, O(n)) → Best Approach

**Idea:**
- Two cases:
- Case 1: k = 0
- Count numbers appearing at least twice.
- Each gives one pair.
- Case 2: k > 0
- For each unique number x:
- Check if x + k exists.
- Count each pair once.

**Steps:**
- Build frequency map
- If k == 0:
- count how many freq[x] ≥ 2
- If k > 0:
- for each unique key:
- check if key + k exists

**Java Code:**
```java
public int countPairs(int[] A, int k) {

    HashMap<Integer, Integer> freq = new HashMap<>();
    for (int x : A)
        freq.put(x, freq.getOrDefault(x, 0) + 1);

    int count = 0;

    if (k == 0) {
        for (int f : freq.values())
            if (f >= 2) count++;
        return count;
    }

    HashSet<Integer> set = new HashSet<>(freq.keySet());
    for (int x : set)
        if (set.contains(x + k))
            count++;

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Difference only concerns values, not positions.
- HashSet gives instant lookup → fastest solution.

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

---

## 7. Justification / Proof of Optimality

- Brute force is too slow for n = 100000
- Two pointer requires sorting → good but slower
- Hashing uses value-difference logic directly → optimal
---

## 8. Variants / Follow-Ups

- Count ordered pairs (i,j)
- Find all pairs, not just count
- Count pairs with sum K instead of difference K
- Return boolean instead of count
---

## 9. Tips & Observations

- Always separate the k = 0 case.
- For unordered pairs, use value pairs, not indices.
- HashSet simplifies pair uniqueness.
---

## 10. Pitfalls

- Counting the same value pair multiple times (e.g., duplicates → count only once).
- Forgetting the special case k = 0, where only numbers with freq ≥ 2 count.
- Treating (a, b) and (b, a) as different → unordered pair must be counted once.
- Checking both x + k AND x - k → leads to double counting; only check x + k.
- Failing to skip duplicates when using sorting + two pointers.
- Counting index pairs instead of distinct value pairs.
---

<!-- #endregion -->
