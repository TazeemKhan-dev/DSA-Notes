<!-- #region 25-Find Geometric Triplets -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q25: Find Geometric Triplets</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given a sorted array of integers. We need to find all triplets (a, b, c) such that they form a geometric progression (GP).
- **Goal:** Print all triplets (arr[i], arr[j], arr[k]) where arr[j]/arr[i] == arr[k]/arr[j] → common ratio same.  Output triplets in lexicographic order (sorted by first, then second, then third element).
- **Paraphrase:** For every possible triplet in the array, check if it forms a geometric progression.  Print them sorted automatically since the array is already sorted.

---

## 2. Constraints

- 1 <= N <= 10
- Array elements are positive integers


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
6
1 2 6 10 18 54
```
Output:
```text
2 6 18
6 18 54
```

**Example 2 (Normal Case):**
Input:
```text
8
2 8 10 15 16 30 32 64
```
Output:
```text
2 8 32
8 16 32
16 32 64
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Iterate all triplets (i < j < k) using 3 nested loops
  - Check if arr[j]^2 == arr[i] * arr[k] → avoids floating point division
  - Print valid triplets

**Java Code:**
```java
public static void findGPTripletsBrute(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 2; i++) {
        for (int j = i + 1; j < n - 1; j++) {
            for (int k = j + 1; k < n; k++) {
                if ((long)arr[j] * arr[j] == (long)arr[i] * arr[k]) {
                    System.out.println(arr[i] + " " + arr[j] + " " + arr[k]);
                }
            }
        }
    }
}
```

**Complexity:**
- Time: O(n³) → three nested loops
- Space: O(1) → no extra space

### Approach 2: Optimal (Using Hashing / Map)

- **Idea:**
  - Use a HashSet for fast lookup
  - For each pair (i, j) as first two elements, compute expectedThird = arr[j]*arr[j]/arr[i]
  - Check if expectedThird exists in the array using HashSet

**Java Code:**
```java
import java.util.*;

public static void findGPTripletsOptimal(int[] arr) {
    int n = arr.length;
    Set<Integer> set = new HashSet<>();
    for (int num : arr) set.add(num);

    for (int i = 0; i < n - 2; i++) {
        for (int j = i + 1; j < n - 1; j++) {
            if ((arr[j] * arr[j]) % arr[i] == 0) { // ensure integer division
                int expectedThird = (arr[j] * arr[j]) / arr[i];
                if (set.contains(expectedThird) && expectedThird > arr[j]) {
                    System.out.println(arr[i] + " " + arr[j] + " " + expectedThird);
                }
            }
        }
    }
}
```

**Complexity:**
- Time: O(n²) → iterate all pairs (i, j)
- Space: O(n) → for HashSet lookup


---

## 5. Justification / Proof of Optimality

- Brute force is simple but O(n³) → acceptable since n <= 10.
- Optimal approach reduces complexity using a HashSet lookup for third element → O(n²).
- Lexicographic order is naturally satisfied because the array is already sorted.

---

## 6. Variants / Follow-Ups

- Count number of GP triplets instead of printing
- Triplets for negative numbers or zeros
- Triplets with common ratio = r given, instead of any ratio
- Longest GP subsequence instead of triplets

<!-- #endregion -->

