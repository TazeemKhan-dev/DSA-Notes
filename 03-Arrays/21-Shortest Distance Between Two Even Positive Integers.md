<!-- #region 21-Shortest Distance Between Two Even Positive Integers -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q21: Shortest Distance Between Two Even Positive Integers</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an array of integers. We need to find the shortest distance between any two even positive integers.
- **Goal:** Find two even positive integers arr[i] and arr[j] with minimum |i - j|.  Return -1 if there is zero or one even positive integer.
- **Paraphrase:** Iterate through the array, track positions of even positive numbers.  Calculate distance to the previous even positive number.  Keep the minimum distance.

---

## 2. Constraints

- 2 <= n <= 10^6
- 0 < arr[i] <= 10^9
- Elements are positive integers


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
2
1 2
```
Output:
```text
-1
Explanation: Only 1 even positive number.
```

**Example 2 (Normal Case):**
Input:
```text
5
2 4 1 6 7
```
Output:
```text
1
Explanation:

Distance(2,4) = 1

Distance(2,6) = 3

Distance(4,6) = 2

Shortest = 1
```


---

## 4. Approaches

### Approach 1: Single Scan (Optimal)

- **Idea:**
  - Keep track of the index of the previous even positive integer.
  - For each even positive integer, compute distance to previous and update minimum.

**Java Code:**
```java
public static int shortestEvenDistance(int[] arr) {
    int prevIndex = -1;
    int minDist = Integer.MAX_VALUE;
    
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] > 0 && arr[i] % 2 == 0) { // even positive
            if (prevIndex != -1) {
                minDist = Math.min(minDist, i - prevIndex);
            }
            prevIndex = i;
        }
    }
    
    return (minDist == Integer.MAX_VALUE) ? -1 : minDist;
}
```

**Complexity:**
- Time: O(n) → single pass
- Space: O(1) → only index and minDist

### Approach 2: Store All Even Indices (Not Needed)

- **Idea:**
  - Store indices of all even positive numbers in a list
  - Iterate through list to compute distances
  - Limitation:
  - Extra space O(k) where k = number of even positive numbers
  - Slower in practice for very large n


---

## 5. Justification / Proof of Optimality

- Optimal approach uses single pass and O(1) space.
- Handles all edge cases: no even numbers, single even number, consecutive even numbers.

---

## 6. Variants / Follow-Ups

- Shortest distance between odd positive integers
- Shortest distance for all positive numbers divisible by k
- Shortest distance with array in circular form
- Find all pairs with minimum distance instead of just distance

<!-- #endregion -->

