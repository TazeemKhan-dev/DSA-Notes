<!-- #region 27-Divisible Sum Pairs -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q27: Divisible Sum Pairs</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an integer array arr and a positive integer k. We need to find all pairs (i, j) with i < j such that (arr[i] + arr[j]) % k == 0.
- **Goal:** Count all valid pairs where the sum is divisible by k.
- **Paraphrase:** Iterate through all possible pairs (i,j) with i < j.  Check divisibility and count the pairs.  Optimize using modulo frequencies.

---

## 2. Constraints

- 1 <= n <= 10^3
- 1 <= arr[i] <= 10^6
- 1 <= k <= 10^6


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
6 3
1 3 2 6 1 2
```
Output:
```text
5
Explanation:
Valid pairs: (0,2),(0,5),(1,3),(2,4),(4,5)
```

**Example 2 (Normal Case):**
Input:
```text
4 5
1 3 2 6
```
Output:
```text
1
Explanation:
Valid pair: (1,2)
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Iterate all pairs (i,j) with i<j
  - Check (arr[i]+arr[j]) % k == 0
  - Count valid pairs

**Java Code:**
```java
public static int divisibleSumPairsBrute(int[] arr, int k) {
    int n = arr.length;
    int count = 0;
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if ((arr[i] + arr[j]) % k == 0) count++;
        }
    }
    return count;
}
```

**Complexity:**
- Time: O(n²) → nested loops
- Space: O(1) → constant

### Approach 2: Optimal (Using Modulo Frequency)

- **Idea:**
  - Use frequency array of mod k values
  - For each element, compute mod = arr[i] % k
  - Number of valid pairs = frequency of (k - mod) % k seen so far

**Java Code:**
```java
public static int divisibleSumPairsOptimal(int[] arr, int k) {
    int[] freq = new int[k];
    int count = 0;

    for (int num : arr) {
        int mod = num % k;
        int complement = (k - mod) % k; // pair sum divisible by k
        count += freq[complement];
        freq[mod]++;
    }

    return count;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(k) → store modulo frequencies


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n²) → acceptable only for small n
- Optimal approach leverages modulo complement counting → O(n) and safe for larger arrays
- Correctly counts all (i,j) with i < j without checking all pairs explicitly

---

## 6. Variants / Follow-Ups

- Count pairs with sum divisible by any number k
- Find all actual pairs instead of count
- Consider triplets instead of pairs
- Allow negative numbers and handle modulo correctly

<!-- #endregion -->

