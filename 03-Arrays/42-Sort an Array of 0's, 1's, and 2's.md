<!-- #region 42-Sort an Array of 0's, 1's, and 2's -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q42: Sort an Array of 0's, 1's, and 2's</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array containing only 0, 1, and 2, sort the array in non-decreasing order.
- **Paraphrase:** Arrange all 0’s first, followed by 1’s, then 2’s in the original array.

---

## 2. Constraints

- 2 <= nums.length <= 10^5
- 1 <= |nums[i]| <= 10^4
- Array length is even and positives = negatives.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[1,0,2,1,0]
```
Output:
```text
[0,0,1,1,2]
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Count number of 0’s, 1’s, and 2’s.
  - Overwrite the array with 0’s, 1’s, 2’s according to counts.

**Java Code:**
```java
public static void sortColorsCounting(int[] nums) {
    int count0 = 0, count1 = 0, count2 = 0;
    for (int num : nums) {
        if (num == 0) count0++;
        else if (num == 1) count1++;
        else count2++;
    }
    
    int i = 0;
    while (count0-- > 0) nums[i++] = 0;
    while (count1-- > 0) nums[i++] = 1;
    while (count2-- > 0) nums[i++] = 2;
}
```

**Complexity:**
- Time: O(n) → single pass to count + single pass to fill.
- Space: O(1) → 3 counters only.

### Approach 2: Optimal (Dutch National Flag Algorithm)

- **Idea:**
  - Use three pointers: low, mid, high.
  - low → next position for 0, mid → current element, high → next position for 2.
  - Traverse array with mid:
  - If nums[mid] == 0 → swap with nums[low], low++, mid++
  - If nums[mid] == 1 → mid++
  - If nums[mid] == 2 → swap with nums[high], high--

**Java Code:**
```java
public static void sortColorsDutchFlag(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;
    while (mid <= high) {
        if (nums[mid] == 0) {
            int temp = nums[low];
            nums[low] = nums[mid];
            nums[mid] = temp;
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else { // nums[mid] == 2
            int temp = nums[mid];
            nums[mid] = nums[high];
            nums[high] = temp;
            high--;
        }
    }
}
```

**Complexity:**
- Time: O(n) → single traversal.
- Space: O(1) → in-place.


---

## 5. Justification / Proof of Optimality

- Dutch National Flag Algorithm ensures single pass and in-place sorting, optimal for large arrays.
- Counting method is simple and uses constant space, but requires two passes.

---

## 6. Variants / Follow-Ups

- Array contains more than 3 distinct elements → can extend with counting sort.
- Sort array in decreasing order instead of non-decreasing.
- Array elements are not consecutive integers → use hashmap or quicksort.
- Online streaming input → maintain three queues instead of array.

<!-- #endregion -->

