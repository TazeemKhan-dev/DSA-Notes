<!-- #region 46-Next Permutation -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q46: Next Permutation</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given array nums, rearrange into next lexicographically greater permutation.

---

## 2. Constraints

- 1 <= nums.length <= 100
- 0 <= nums[i] <= 100


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[1, 2, 3]
```
Output:
```text
[1,3,2]
```


---

## 4. Approaches

### Approach 1: Brute Force (Generate All, Pick Next)

- **Idea:**
  - Generate all permutations, sort, and pick the next one.

**Complexity:**
- Time: O(n! * n)
- Space: O(n!)

### Approach 2: Optimal In-Place O(n)

- **Idea:**
  - Find pivot index i where nums[i] < nums[i+1].
  - Find rightmost element j > nums[i].
  - Swap nums[i] and nums[j].
  - Reverse i+1 ... end.

**Java Code:**
```java
class NextPermutation {
    public static void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while (i >= 0 && nums[i] >= nums[i+1]) i--;

        if (i >= 0) {
            int j = nums.length - 1;
            while (nums[j] <= nums[i]) j--;
            swap(nums, i, j);
        }
        reverse(nums, i+1, nums.length-1);
    }

    private static void swap(int[] nums, int i, int j) {
        int tmp = nums[i]; nums[i] = nums[j]; nums[j] = tmp;
    }

    private static void reverse(int[] nums, int l, int r) {
        while (l < r) {
            swap(nums, l, r);
            l++;
            r--;
        }
    }
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Brute Force is impractical for large n (n! growth).
- Optimal solution achieves result in linear time, which is best possible.

---

## 6. Variants / Follow-Ups

- Previous permutation.
- K-th next permutation.
- Next permutation with repeated elements.

<!-- #endregion -->

