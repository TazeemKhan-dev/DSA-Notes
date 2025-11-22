<!-- #region 36-Remove Duplicates from Sorted Array -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q36: Remove Duplicates from Sorted Array</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given a sorted array, remove duplicates in-place so that each unique element appears only once.
- **Goal:** Return the count of unique elements and modify the array so that the first k elements contain the unique values in order.
- **Paraphrase:** Iterate through the array, skip duplicates, and shift unique elements to the front.

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- Array is sorted in non-decreasing order.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[0, 0, 3, 3, 5, 6]
```
Output:
```text
4, Array = [-2, 2, 4, 5, _, _, _, _]
```

**Example 2 (Normal Case):**
Input:
```text
[-30, -30, 0, 0, 10, 20, 30, 30]
```
Output:
```text
5, Array = [-30, 0, 10, 20, 30, _, _, _]
```


---

## 4. Approaches

### Approach 1: Brute Force (Using Extra Array)

- **Idea:**
  - Traverse the array and add unique elements to a new array.
  - Copy the unique elements back to the original array.

**Java Code:**
```java
public static int removeDuplicatesBrute(int[] nums) {
    int n = nums.length;
    if(n == 0) return 0;
    int[] temp = new int[n];
    int index = 0;
    temp[index++] = nums[0];
    for(int i = 1; i < n; i++){
        if(nums[i] != nums[i-1]){
            temp[index++] = nums[i];
        }
    }
    for(int i = 0; i < index; i++){
        nums[i] = temp[i];
    }
    return index;
}
```

**Complexity:**
- Time: O(n)
- Space: O(n)

### Approach 2: Optimal (Two Pointers, In-place)

- **Idea:**
  - Use two pointers: i for iteration, j for placing unique elements.
  - If nums[i] != nums[i-1], copy nums[i] to nums[j].
  - Return j+1 as count of unique elements.

**Java Code:**
```java
public static int removeDuplicatesOptimal(int[] nums) {
    if(nums.length == 0) return 0;
    int j = 0; // pointer for placing unique elements
    for(int i = 1; i < nums.length; i++){
        if(nums[i] != nums[j]){
            j++;
            nums[j] = nums[i];
        }
    }
    return j + 1; // number of unique elements
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approach only traverses the array once and does not require extra space.
- Preserves relative order of unique elements.
- Works for negative numbers, duplicates, and arrays of size 1.

---

## 6. Variants / Follow-Ups

- Remove duplicates from unsorted array → requires HashSet or sorting first.
- Count frequency of unique elements while removing duplicates.
- Return array of unique elements instead of modifying in-place.

<!-- #endregion -->

