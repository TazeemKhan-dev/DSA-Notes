<!-- #region 43-Move Zeros to End -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q43: Move Zeros to End</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array nums, move all the 0s to the end of the array without changing the relative order of non-zero elements.
- **Paraphrase:** Shift all zero elements to the right while keeping non-zero elements in their original sequence.

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- Must be in-place (no extra array allowed).


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[0, 1, 4, 0, 5, 2]
```
Output:
```text
[1, 4, 5, 2, 0, 0]
```


---

## 4. Approaches

### Approach 1: Brute Force (Shift Zeros Iteratively)

- **Idea:**
  - For each zero, shift all elements on its right one step left and append zero at the end.

**Java Code:**
```java
public void moveZerosBrute(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        if (nums[i] == 0) {
            for (int j = i; j < n - 1; j++) {
                nums[j] = nums[j + 1];
            }
            nums[n - 1] = 0;
        }
    }
}
```

**Complexity:**
- Time: O(n^2) → shifting elements for each zero.
- Space: O(1) → in-place.

### Approach 2: Optimal (Two Pointers)

- **Idea:**
  - Use a pointer pos to track the position to place the next non-zero element.
  - Traverse the array: if element is non-zero, swap it with nums[pos] and increment pos.
  - All zeros naturally move to the end.

**Java Code:**
```java
public void moveZerosOptimal(int[] nums) {
    int pos = 0; // position for the next non-zero
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            int temp = nums[i];
            nums[i] = nums[pos];
            nums[pos] = temp;
            pos++;
        }
    }
}
```

**Complexity:**
- Time: O(n) → single traversal.
- Space: O(1) → in-place.


---

## 5. Justification / Proof of Optimality

- Brute Force: Works but inefficient for large n due to repeated shifting.
- Two-Pointer Approach:
- Only swaps non-zero elements, maintains relative order,
- linear time complexity, minimal swaps → optimal solution.

---

## 6. Variants / Follow-Ups

- Move all non-zero elements to the start instead.
- Move all zeros to the beginning.
- Count minimum swaps required to move zeros to end.
- Multiple arrays merged → move zeros in combined array.

<!-- #endregion -->

