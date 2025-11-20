<!-- #region 128-Subarray Product Less Than K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q128: Subarray Product Less Than K</h1>

## 1. Problem Understanding

- You are given:
  * an array nums[]
  * an integer k
- You must return how many contiguous subarrays have a product < k.
- A subarray must be continuous.
---

## 2. Constraints

- 1 ≤ n ≤ 3 * 10^4
- 1 ≤ nums[i] ≤ 1000
- 0 ≤ k ≤ 10^6
- Time must be better than O(N²) (since N is 30k)
---

## 3. Edge Cases

- k <= 1 → answer = 0
  * Because product of positive integers ≥ 1 cannot be < 1.
- All elements > k → only single elements < k count
- All elements = 1 → every subarray counts
---

## 4. Examples

```text
Example 1
Input

4
10 5 2 5
100
Output

8
Explanation

The 8 subarrays that have product less than 100 are: [10], [5], [2], [5], [10, 5], [5, 2], [2, 5], [5, 2, 5]

Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.

Example 2
Input

3
1 2 3
0
Output

0
Explanation

No subarray is possible with product less than K.
```

---

## 5. Approaches

### Approach 1: Brute Force (Nested Loops)

**Idea:**
- Check every possible subarray, compute product, count if < k.
- ❌ Why it fails
  * Worst case:
  * 30,000 elements
  * 450 million subarrays
  * Too slow

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N²)
- 💾 Space Complexity
  * O(1)

### Approach 2: Sliding Window (Optimal)

**Idea:**
- Since all nums[i] > 0:
  * Expanding the window increases product
  * Shrinking the window decreases product
- So we use a sliding window:
  * Use left and right pointers
  * Maintain product of current window
  * If product < k, then:
    * all subarrays ending at right and starting from [left..right] are valid
    * count += (right - left + 1)
  * Otherwise shrink from left
- This works because for each right, every subarray inside the window is valid.

**Steps:**
- Initialize:
  * product = 1
  * left = 0
  * count = 0
- For each right from 0→n-1:
  * multiply product with nums[right]
  * while product ≥ k:
  * divide product by nums[left] and move left
  * now product < k, so:
  * add (right - left + 1) to count
  * (these are valid subarrays ending at right)

**Java Code:**
```java
public int numSubarrayProductLessThanK(int[] nums, int k) {
    if (k <= 1) return 0;

    int left = 0;
    int count = 0;
    long product = 1;

    for (int right = 0; right < nums.length; right++) {
        product *= nums[right];

        while (product >= k) {
            product /= nums[left];
            left++;
        }

        count += (right - left + 1);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Every time we expand our window by including nums[right],
- the only reason product could become ≥ k is because the window is too large.
- Shrinking from the left gradually reduces product until the window is valid again.
- Once valid:
  * Every subarray ending at right is valid:
    * [left..right]
    * [left+1..right]
    * [left+2..right]
    * ...
    * [right..right]
  * Count = (right - left + 1)
- Since each element enters and leaves the window once, total operations are linear.
- It is a classic sliding-window two-pointer trick.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Sliding window → O(N)
  * Because each element is multiplied once and divided once.
- 💾 Space Complexity
  * O(1)
  * Only variables used.

---

## 6. Justification / Proof of Optimality

- Sliding window works because:
  * All numbers are positive
  * Increasing window increases product
  * Decreasing window decreases product
- Therefore, window is monotonic and expanding/shrinking is valid.
- This gives the optimal solution.
---

## 7. Variants / Follow-Ups

- Subarray sum < k (same logic)
- Subarray averages < k
- Subarray with product ≤ k
- Count of subarrays with at most K distinct elements
- Longest subarray with product < k
- All follow the sliding-window pattern.
---

## 8. Tips & Observations

- If k <= 1, return 0 immediately
- Use long for product (prevent overflow)
- Never reset product—always shrink window naturally
- Positive numbers guarantee monotonic window behavior
- (right - left + 1) is the key formula to count subarrays
---

<!-- #endregion -->
