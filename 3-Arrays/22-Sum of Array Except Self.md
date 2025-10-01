<!-- #region 22-Sum of Array Except Self -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q22: Sum of Array Except Self</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given an array of integers. We need to return a new array such that each element is the sum of all other elements except itself.
- **Goal:** For each index i, calculate sum(arr) - arr[i] and store in result array.
- **Paraphrase:** Compute total sum of array once.  Subtract arr[i] from total sum for each index to get output[i].  Avoid nested loops for efficiency.

---

## 2. Constraints

- 1 <= n <= 5000
- 1 <= arr[i] <= 1000000


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
4 3 2 10
```
Output:
```text
15 16 17 9
Explanation:

totalSum = 4 + 3 + 2 + 10 = 19

output[0] = 19 - 4 = 15

output[1] = 19 - 3 = 16

output[2] = 19 - 2 = 17

output[3] = 19 - 10 = 9
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - For each element, iterate through the array and sum all other elements.

**Java Code:**
```java
public static long[] sumExceptSelfBrute(int[] arr) {
    int n = arr.length;
    long[] result = new long[n];

    for (int i = 0; i < n; i++) {
        long sum = 0;
        for (int j = 0; j < n; j++) {
            if (i != j) sum += arr[j];
        }
        result[i] = sum;
    }
    return result;
}
```

**Complexity:**
- Time: O(n^2) → nested loops
- Space: O(n) → result array

### Approach 2: Optimal (Total Sum Subtraction)

- **Idea:**
  - Compute total sum of array.
  - For each index, subtract current element from total sum to get output.

**Java Code:**
```java
public static long[] sumExceptSelfOptimal(int[] arr) {
    int n = arr.length;
    long totalSum = 0;
    for (int i = 0; i < n; i++) {
        totalSum += arr[i];
    }

    long[] result = new long[n];
    for (int i = 0; i < n; i++) {
        result[i] = totalSum - arr[i];
    }
    return result;
}
```

**Complexity:**
- Time: O(n) → two passes
- Space: O(n) → result array


---

## 5. Justification / Proof of Optimality

- Optimal approach is better for large n and avoids unnecessary nested loops.
- Brute force is intuitive but inefficient.
- Both approaches handle positive integers correctly.

---

## 6. Variants / Follow-Ups

- Product of array except self
- Sum except self modulo k
- Arrays containing negative integers
- Sparse arrays with zeros

<!-- #endregion -->

