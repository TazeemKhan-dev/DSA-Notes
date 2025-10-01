<!-- #region 39-Leaders in an Array -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q39: Leaders in an Array</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** Given an array of integers, identify elements that are strictly greater than all elements to their right.
- **Goal:** Return a list of all such leaders in the order they appear in the array.
- **Paraphrase:** : A leader is any element that is bigger than all elements on its right, including the last element (which is always a leader).

---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4


---

## 3. Examples & Edge Cases

**Example 1 (Edge Case):**
Input:
```text
[10]
```
Output:
```text
[10]
```

**Example 2 (Normal Case):**
Input:
```text
[-3, 4, 5, 1, -4, -5]
```
Output:
```text
[5, 1, -4, -5]
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - For each element nums[i], check all elements to its right.
  - If nums[i] is greater than all right elements, add to result.

**Java Code:**
```java
public static List<Integer> leadersBrute(int[] nums) {
    List<Integer> leaders = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        boolean isLeader = true;
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] <= nums[j]) {
                isLeader = false;
                break;
            }
        }
        if (isLeader) leaders.add(nums[i]);
    }
    return leaders;
}
```

**Complexity:**
- Time: O(n^2)
- Space: O(1)

### Approach 2: Optimal (Right-to-Left Scan)

- **Idea:**
  - Initialize maxFromRight = nums[n-1] → last element is always a leader.
  - Traverse array from right to left:
  - If nums[i] > maxFromRight, then nums[i] is a leader.
  - Update maxFromRight = max(maxFromRight, nums[i]).
  - Reverse result list at the end to maintain original order.

**Java Code:**
```java
public static List<Integer> leadersOptimal(int[] nums) {
    List<Integer> leaders = new ArrayList<>();
    int n = nums.length;
    int maxFromRight = nums[n - 1];
    leaders.add(maxFromRight); // last element is always a leader
    
    for (int i = n - 2; i >= 0; i--) {
        if (nums[i] > maxFromRight) {
            leaders.add(nums[i]);
            maxFromRight = nums[i];
        }
    }
    
    Collections.reverse(leaders); // to maintain order of appearance
    return leaders;
}
```

**Complexity:**
- Time: O(n) → single pass.
- Space: O(n) → for result list.


---

## 5. Justification / Proof of Optimality

- Optimal approach is linear time and efficient, scanning the array once from right.
- Avoids nested loops → much faster for large arrays (n <= 10^5).
- Always maintains order by reversing the result at the end.

---

## 6. Variants / Follow-Ups

- Find all leaders except the last element → modified right-to-left scan.
- Find leaders in a circular array → consider array wraps around.
- Find minimum leaders → elements smaller than all right elements.
- Count the number of leaders instead of listing them.

<!-- #endregion -->

