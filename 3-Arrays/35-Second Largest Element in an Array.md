<!-- #region 35-Second Largest Element in an Array -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q35: Second Largest Element in an Array</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** Traverse the array once (or optimally) to determine the largest and second-largest distinct elements. If only one distinct element exists, return -1.

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- Array may contain duplicate elements.


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
[8, 8, 7, 6, 5]
```
Output:
```text
7
```

**Example 2 (Normal Case):**
Input:
```text
[10, 10, 10, 10, 10]
```
Output:
```text
-1
```


---

## 4. Approaches

### Approach 1: Brute Force (Sort + Unique)

- **Idea:**
  - Remove duplicates.
  - Sort array descending.
  - Return second element if exists, else -1.

**Java Code:**
```java
public static int secondLargestBrute(int[] nums) {
    TreeSet<Integer> set = new TreeSet<>();
    for(int num : nums) set.add(num); // automatically sorts and keeps unique
    if(set.size() < 2) return -1;
    set.pollLast(); // remove largest
    return set.last(); // second largest
}
```

**Complexity:**
- Time: O(n log n) (due to sorting)
- Space: O(n) (for storing unique elements)

### Approach 2: Optimal (Single Pass)

- **Idea:**
  - Track two variables: largest and secondLargest.
  - Traverse the array once.
  - Update largest and secondLargest as you encounter higher values.

**Java Code:**
```java
public static int secondLargestOptimal(int[] nums) {
    int largest = Integer.MIN_VALUE;
    int secondLargest = Integer.MIN_VALUE;
    for(int num : nums){
        if(num > largest){
            secondLargest = largest;
            largest = num;
        } else if(num < largest && num > secondLargest){
            secondLargest = num;
        }
    }
    return secondLargest == Integer.MIN_VALUE ? -1 : secondLargest;
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)


---

## 5. Justification / Proof of Optimality

- Optimal approach traverses the array once (O(n)) and does not require extra space, making it better than the sorting approach.
- Handles duplicates and negative values correctly.

---

## 6. Variants / Follow-Ups

- Find third-largest, fourth-largest, etc. → can extend with more variables.
- Handle kth largest element using min-heap for large arrays.
- Return indices of the largest and second-largest elements instead of values.

<!-- #endregion -->

