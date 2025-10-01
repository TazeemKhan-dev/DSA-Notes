<!-- #region 19-Array Subtracting -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q19: Array Subtracting</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given two arrays, arr1 and arr2, representing digits of two numbers. The arrays may have different sizes.
- **Goal:** Subtract the number represented by arr2 from the number represented by arr1 (arr1 - arr2) and return the result as an array of digits.
- **Paraphrase:** Perform subtraction digit by digit from the end (least significant digit).  Handle borrow if needed.  Return the resulting number as an array.

---

## 2. Constraints

- 1 <= n1, n2 <= 10
- 0 <= arr1[i], arr2[i] < 10
- arr1 represents a number greater than or equal to arr2


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
4
1 2 3 4
2
2 1
```
Output:
```text
1 2 1 3
```


---

## 4. Approaches

### Approach 1: Digit-by-Digit Subtraction

- **Idea:**
  - Start from the last index of both arrays (least significant digit).
  - Subtract corresponding digits, handling borrow.
  - Store result in a list, reverse at the end, and remove leading zeros.

**Java Code:**
```java
public static int[] subtractArrays(int[] arr1, int[] arr2) {
    int i = arr1.length - 1;
    int j = arr2.length - 1;
    int borrow = 0;
    java.util.ArrayList<Integer> resList = new java.util.ArrayList<>();

    while (i >= 0) {
        int diff = arr1[i] - borrow;
        if (j >= 0) diff -= arr2[j--];

        if (diff < 0) {
            diff += 10;
            borrow = 1;
        } else {
            borrow = 0;
        }

        resList.add(diff);
        i--;
    }

    // Remove leading zeros
    while (resList.size() > 1 && resList.get(resList.size() - 1) == 0) {
        resList.remove(resList.size() - 1);
    }

    // Reverse to correct order
    int[] result = new int[resList.size()];
    for (int k = 0; k < resList.size(); k++) {
        result[k] = resList.get(resList.size() - 1 - k);
    }
    return result;
}
```

**Complexity:**
- Time: O(n1) → each digit processed once
- Space: O(n1) → for result

### Approach 2: Convert Arrays to Numbers (Limited)

- **Idea:**
  - Convert both arrays to numbers
  - Subtract numbers
  - Convert result back to array
  - Limitation:
  - Works only if numbers fit in int or long.
  - Not recommended for large arrays.


---

## 5. Justification / Proof of Optimality

- Optimal approach: digit-by-digit subtraction works for any array size.
- Correctly handles borrow and leading zeros.
- Approach 2 is simple but limited by numeric type size.

---

## 6. Variants / Follow-Ups

- Subtract multiple numbers represented by arrays
- Subtract numbers in reverse order arrays
- Implement array multiplication or division similarly
- Handle negative results if arr2 > arr1

<!-- #endregion -->

