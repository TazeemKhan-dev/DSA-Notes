<!-- #region 37-Find Missing Number -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q37: Find Missing Number</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array of distinct integers of size n containing values from 0 to n inclusive, one number is missing in the range.
- **Paraphrase:** The array has n elements, but the complete range 0..n has n+1 elements. Identify the number not present in the array.

---

## 2. Constraints

- n == nums.length
- 1 <= n <= 10^4
- 0 <= nums[i] <= n
- All numbers in nums are unique.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[1, 2, 3]
```
Output:
```text
0
```

**Example 2 (Normal Case):**
Input:
```text
[0, 2, 3, 1, 4]
```
Output:
```text
5
```

**Example 3 (Normal Case):**
Input:
```text
[0, 1, 2, 4, 5, 6]
```
Output:
```text
3
```


---

## 4. Approaches

### Approach 1: Brute Force (Check Each Number)

- **Idea:**
  - Traverse numbers 0..n.
  - For each number, check if it exists in the array.
  - Return the number which is missing.

**Java Code:**
```java
public static int missingNumberBrute(int[] nums) {
    int n = nums.length;
    for(int num = 0; num <= n; num++){
        boolean found = false;
        for(int i = 0; i < n; i++){
            if(nums[i] == num){
                found = true;
                break;
            }
        }
        if(!found) return num;
    }
    return -1; // should not happen
}
```

**Complexity:**
- Time: O(n^2)
- Space: O(1)

### Approach 2: Optimal (Sum Formula / XOR)

- **Idea:**
  - Idea 1 (Sum Formula):
  - Sum of numbers from 0..n = n*(n+1)/2.
  - Subtract sum of elements in nums.
  - Remaining value = missing number.
  - Idea 2 (XOR):
  - XOR all numbers 0..n.
  - XOR all elements in nums.
  - XOR of the two results = missing number (because duplicates cancel out).

**Java Code:**
```java
Java Code (Sum Formula):

public static int missingNumberOptimal(int[] nums) {
    int n = nums.length;
    int sum = n * (n + 1) / 2;
    int arrSum = 0;
    for(int num : nums){
        arrSum += num;
    }
    return sum - arrSum;
}


Java Code (XOR):

public static int missingNumberXOR(int[] nums) {
    int n = nums.length;
    int xor = 0;
    for(int i = 0; i <= n; i++){
        xor ^= i;
    }
    for(int num : nums){
        xor ^= num;
    }
    return xor;
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approaches are O(n) and O(1) extra space.
- Works regardless of array order.
- Sum formula may risk integer overflow for large n, XOR avoids overflow.

---

## 6. Variants / Follow-Ups

- Array may have duplicates → find missing number or repeated numbers.
- Numbers not in range [0, n] → adjust formula or XOR range.
- Find all missing numbers in [0, n] if multiple missing.

<!-- #endregion -->

