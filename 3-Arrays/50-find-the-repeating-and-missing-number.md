<!-- #region 50-Find the Repeating and Missing Number -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q50: Find the Repeating and Missing Number</h1>

## 1. Problem Understanding

- Given an array of size n with numbers from [1, n].
- One number appears twice → repeating number (A).
- One number is missing → missing number (B).
- Goal: Return [A, B].
- Constraint: Cannot modify the original array.
- Key idea: detect duplicate and missing number without altering array, efficiently.
---

## 2. Constraints

- n == nums.length
- 1 ≤ n ≤ 10^5
- All numbers in nums are in [1, n]
- Exactly one number repeats
- Exactly one number is missing
---

## 3. Edge Cases

- Array of minimum size n = 2
- Repeating number at the start or end of array
- Missing number at the start or end of array
- Array with consecutive numbers except for missing/repeating
- Only one element repeats (no other duplicates)
---

## 4. Examples

```text
Example 1:
Input: [3, 5, 4, 1, 1]
Output: [1, 2]
Explanation: 1 repeats, 2 is missing

Example 2:
Input: [1, 2, 3, 6, 7, 5, 7]
Output: [7, 4]
Explanation: 7 repeats, 4 is missing

Example 3:
Input: [6, 5, 7, 1, 8, 6, 4, 3, 2]
Output: [6, 9]
Explanation: 6 repeats, 9 is missing
```

---

## 5. Approaches

### Approach 1: Brute Force (HashMap / Counting)

**Idea:**
- Count frequency of each number using a HashMap
- Identify the number appearing twice → repeating
- Identify the number not present → missing

**Steps:**
- Create a frequency map of numbers.
- Iterate from 1 to n:
- If count = 2 → repeating
- If count = 0 → missing

**Java Code:**
```java
class FindRepeatingMissingBrute {
    public int[] findRepeatingMissing(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> map = new HashMap<>();
        for(int num: nums) map.put(num, map.getOrDefault(num, 0)+1);
        int repeating = -1, missing = -1;
        for(int i=1; i<=n; i++){
            if(!map.containsKey(i)) missing = i;
            else if(map.get(i) == 2) repeating = i;
        }
        return new int[]{repeating, missing};
    }
}
```

**Complexity (Time & Space):**
- Time: O(n), Space: O(n)

### Approach 2: Mathematical / Sum & XOR

**Idea:**
- Use formulas for sum and sum of squares:
- sum = 1 + 2 + ... + n = n*(n+1)/2
- sumSq = 1^2 + 2^2 + ... + n^2 = n*(n+1)*(2n+1)/6
- Let repeating = A, missing = B
- Observed sum difference: sum(nums) - sum = A - B
- Observed sum of squares difference: sumSq(nums) - sumSq = A^2 - B^2 = (A-B)*(A+B)
- Solve the two equations to find A and B.

**Java Code:**
```java
class FindRepeatingMissingMath {
    public int[] findRepeatingMissing(int[] nums){
        int n = nums.length;
        long sum = n*(n+1)/2;
        long sumSq = n*(n+1)*(2*n+1)/6;
        long sumArr = 0, sumSqArr = 0;
        for(int num: nums){
            sumArr += num;
            sumSqArr += (long)num*num;
        }
        long diff = sumArr - sum; // A - B
        long sumDiff = (sumSqArr - sumSq)/diff; // A + B
        int A = (int)((diff + sumDiff)/2);
        int B = (int)(sumDiff - A);
        return new int[]{A, B};
    }
}
```

**Complexity (Time & Space):**
- Time: O(n), Space: O(1)

### Approach 3: Optimal / XOR Based

**Idea:**
- XOR all numbers from 1 to n and all elements in array → result = A ^ B
- Find any set bit in XOR → divide numbers into 2 groups → separate A and B
- Efficient and avoids extra space

**Java Code:**
```java
class FindRepeatingMissingXOR {
    public int[] findRepeatingMissing(int[] nums){
        int n = nums.length;
        int xor = 0;
        for(int num: nums) xor ^= num;
        for(int i=1;i<=n;i++) xor ^= i;
        
        int setBit = xor & -xor;
        int x=0, y=0;
        for(int num: nums){
            if((num & setBit) != 0) x ^= num;
            else y ^= num;
        }
        for(int i=1;i<=n;i++){
            if((i & setBit)!=0) x ^= i;
            else y ^= i;
        }
        // determine which is repeating
        for(int num: nums){
            if(num==x) return new int[]{x, y};
        }
        return new int[]{y, x};
    }
}
```

**Complexity (Time & Space):**
- Time: O(n), Space: O(1)

---

## 6. Justification / Proof of Optimality

- Brute Force → simple, works, uses extra space
- Math → elegant, constant space, careful with overflow
- XOR → optimal, constant space, avoids arithmetic overflow
---

## 7. Variants / Follow-Ups

- Multiple missing numbers or multiple duplicates
- Arrays with multiple constraints (e.g., numbers in 0 to n-1)
- Similar problems like “Single Number” or “Find Duplicate Number”
---

## 8. Tips & Observations

- XOR trick works because XOR cancels out identical numbers
- Sum & SumSq method leverages simple algebra
- Always check for integer overflow in sum-of-squares for large n
- Brute force is safe but extra memory heavy
---

<!-- #endregion -->
