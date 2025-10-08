<!-- #region 53-Maximum Product Subarray -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q53: Maximum Product Subarray</h1>

## 1. Problem Understanding

- Given an integer array nums, find the subarray (contiguous elements) with the maximum product.
- Return the product of elements of that subarray.
- A subarray must contain at least one element.
- Must consider negative numbers and zeros carefully because they can change the sign of the product.
---

## 2. Constraints

- 1 <= nums.length <= 10^4
- -10 <= nums[i] <= 10
- Product of any prefix or suffix is within -10^9 <= product <= 10^9
---

## 3. Edge Cases

- Array contains zero → product may reset
- Array contains all negative numbers → even length negative subarray may give max product
- Single element array → product = element itself
- All positive numbers → max product = product of whole array
- Array with mix of negatives, zeros, and positives
---

## 4. Examples

```text
Example 1:
Input: [4, 5, 3, 7, 1, 2]
Output: 840
Explanation: Whole array gives maximum product → 45371*2 = 840

Example 2:
Input: [-5, 0, -2]
Output: 0
Explanation: Maximum product subarray is [0] → 0

Example 3:
Input: [1, -2, 3, 4, -4, -3]
Output: 288
Explanation: Maximum product subarray is [3, 4, -4, -3] → 34-4*-3 = 288
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Generate all possible subarrays and calculate their product.
- Keep track of maximum product found.

**Steps:**
- Initialize maxProduct = Integer.MIN_VALUE
- Iterate i from 0 to n-1 → start index
- Iterate j from i to n-1 → end index
- Multiply elements from i to j → calculate product
- Update maxProduct if current product is larger

**Java Code:**
```java
class MaxProductSubarrayBrute {
    public int maxProduct(int[] nums){
        int n = nums.length;
        int maxProduct = Integer.MIN_VALUE;
        for(int i=0;i<n;i++){
            int product = 1;
            for(int j=i;j<n;j++){
                product *= nums[j];
                maxProduct = Math.max(maxProduct, product);
            }
        }
        return maxProduct;
    }
}
```

**Complexity (Time & Space):**
- Time: O(n²)                         
-  Space: O(1)

### Approach 2: Dynamic Programming / Prefix and Suffix Scan

**Idea:**
- Keep track of maximum and minimum product ending at current index
- Negative numbers may turn minimum into maximum and vice versa

**Steps:**
- Initialize maxProd = nums[0], minProd = nums[0], result = nums[0]
- Iterate i = 1 to n-1:
- If nums[i] is negative → swap maxProd and minProd
- Update maxProd = max(nums[i], nums[i]*maxProd)
- Update minProd = min(nums[i], nums[i]*minProd)
- Update result = max(result, maxProd)

**Java Code:**
```java
class MaxProductSubarrayDP {
    public int maxProduct(int[] nums){
        int n = nums.length;
        int maxProd = nums[0], minProd = nums[0], result = nums[0];
        for(int i=1;i<n;i++){
            if(nums[i] < 0){
                int temp = maxProd;
                maxProd = minProd;
                minProd = temp;
            }
            maxProd = Math.max(nums[i], nums[i]*maxProd);
            minProd = Math.min(nums[i], nums[i]*minProd);
            result = Math.max(result, maxProd);
        }
        return result;
    }
}
```

**Complexity (Time & Space):**
- Time: O(n)                               
- Space: O(1)

### Approach 3: Prefix and Suffix Product Scan

**Idea:**
- Compute prefix product from left and right
- Max product = maximum among all prefix and suffix products
- Reset product to 1 if zero encountered

**Steps:**
- Initialize maxProduct = Integer.MIN_VALUE, prefix = 1, suffix = 1
- Iterate from left to right → prefix *= nums[i], update maxProduct, reset prefix if zero
- Iterate from right to left → suffix *= nums[i], update maxProduct, reset suffix if zero

**Java Code:**
```java
class MaxProductSubarrayPrefixSuffix {
    public int maxProduct(int[] nums){
        int n = nums.length;
        int maxProduct = Integer.MIN_VALUE;
        int prefix = 1, suffix = 1;
        for(int i=0;i<n;i++){
            prefix = (prefix==0)? nums[i] : prefix*nums[i];
            suffix = (suffix==0)? nums[n-1-i] : suffix*nums[n-1-i];
            maxProduct = Math.max(maxProduct, Math.max(prefix, suffix));
        }
        return maxProduct;
    }
}
```

**Complexity (Time & Space):**
- Time: O(n)                               
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Brute Force → simple but inefficient, works only for small arrays
- Dynamic Programming → optimal, handles negative numbers and zeros
- Prefix/Suffix Scan → alternative O(n) approach, slightly simpler to code
---

## 7. Variants / Follow-Ups

- Maximum sum subarray → Kadane’s Algorithm
- Maximum product of k elements in an array
- Maximum product subarray with modulo constraints
---

## 8. Tips & Observations

- Keep track of both max and min products due to negatives
- Zero splits subarrays → reset product calculation
- Carefully handle integer overflow if product exceeds limits
- DP approach is most widely used in interviews
---

<!-- #endregion -->
