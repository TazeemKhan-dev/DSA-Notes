<!-- #region 40-Rearrange Array Elements by Sign -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q40: Rearrange Array Elements by Sign</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array with equal numbers of positive and negative integers, rearrange it such that every consecutive pair has opposite signs.
- **Goal:** Return the rearranged array starting with a positive number while maintaining relative order of positive and negative numbers.
- **Paraphrase:** Interleave positive and negative integers while preserving their original order, starting with a positive integer.

---

## 2. Constraints

- 2 <= nums.length <= 10^5
- 1 <= |nums[i]| <= 10^4
- Array length is even and positives = negatives.


---

## 3. Examples & Edge Cases

**Example 1 (Edge Case):**
Input:
```text
[1, -1]
```
Output:
```text
[1, -1]
```

**Example 2 (Normal Case):**
Input:
```text
[2, 4, 5, -1, -3, -4]
```
Output:
```text
[2, -1, 4, -3, 5, -4]
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Create two separate lists: positives and negatives.
  - Traverse the array and fill the two lists.
  - Interleave elements from positives and negatives into a new array.

**Java Code:**
```java
public static int[] rearrangeBrute(int[] nums) {
    List<Integer> pos = new ArrayList<>();
    List<Integer> neg = new ArrayList<>();
    
    for (int num : nums) {
        if (num > 0) pos.add(num);
        else neg.add(num);
    }
    
    int[] result = new int[nums.length];
    int p = 0, n = 0;
    
    for (int i = 0; i < nums.length; i++) {
        if (i % 2 == 0) result[i] = pos.get(p++);
        else result[i] = neg.get(n++);
    }
    
    return result;
}
```

**Complexity:**
- Time: O(n) → single pass for separation + single pass for merging.
- Space: O(n) → extra arrays for positive and negative numbers.

### Approach 2: Optimal (In-Place Using Extra Indices)

- **Idea:**
  - Use two pointers p and n to track positions of next positive and negative numbers.
  - Traverse the array and place positive and negative numbers alternately in a new array (or can do in-place with rotation logic).
  - Maintains relative order without nested loops.

**Java Code:**
```java
public static int[] rearrangeOptimal(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    int p = 0, q = 0; // indices for positives and negatives
    
    // find first positive index
    for (int i = 0; i < n; i++) {
        if (nums[i] > 0) p++;
        else q++;
    }
    
    List<Integer> pos = new ArrayList<>();
    List<Integer> neg = new ArrayList<>();
    
    for (int num : nums) {
        if (num > 0) pos.add(num);
        else neg.add(num);
    }
    
    int pi = 0, ni = 0;
    for (int i = 0; i < n; i++) {
        if (i % 2 == 0) result[i] = pos.get(pi++);
        else result[i] = neg.get(ni++);
    }
    
    return result;
}
```

**Complexity:**
- Time: O(n) → single pass.
- Space: O(n) → result array needed (in-place is more complex but possible).


---

## 5. Justification / Proof of Optimality

- Optimal approach ensures O(n) time and preserves relative order of positives and negatives.
- Brute force is easier to implement but uses extra space.
- In-place rearrangement is possible but increases code complexity significantly.

---

## 6. Variants / Follow-Ups

- Array length odd → more positives or negatives → start with dominant sign.
- Rearrange without extra space → in-place cyclic replacement.
- Rearrange to alternate starting with negative.
- Extend to k-alternating signs (e.g., 2 positives, 1 negative pattern).

<!-- #endregion -->

