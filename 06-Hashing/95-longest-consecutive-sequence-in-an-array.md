<!-- #region 95-Longest Consecutive Sequence in an Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q95: Longest Consecutive Sequence in an Array</h1>

## 1. Problem Understanding

- You are given an integer array nums.
- You must find the length of the longest consecutive elements sequence, where the sequence numbers appear in any order.
- ➡️ A “consecutive sequence” means numbers that come one after another with a difference of 1 (like 1,2,3,4).
- Important: You only care about the length, not the sequence itself.
---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9
---

## 3. Edge Cases

- Empty array → return 0
- Array with duplicates → count only unique numbers
- Array already consecutive → return full length
- Negative and mixed values handled normally
---

## 4. Examples

```text
Input: nums = [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: Longest consecutive sequence is [1, 2, 3, 4].
```

---

## 5. Approaches

### Approach 1: Sorting-Based

**Idea:**
- Sort the array and then count consecutive elements. Skip duplicates and reset when the sequence breaks.

**Steps:**
- Sort the array.
- Use variables currLen and maxLen to track streaks.
- Compare consecutive elements and update counts.

**Java Code:**
```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;
    Arrays.sort(nums);
    int maxLen = 1, currLen = 1;
    
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1]) continue;
        if (nums[i] == nums[i - 1] + 1) currLen++;
        else currLen = 1;
        maxLen = Math.max(maxLen, currLen);
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Sorting: O(n log n)
  * Single traversal: O(n)
  * Total: O(n log n)
- 💾 Space Complexity
  * O(1) (ignoring sort overhead)

### Approach 2: Using HashSet (Optimal)

**Idea:**
- Use a HashSet to check existence of consecutive numbers quickly.
- Start counting only when the current number is the start of a sequence (i.e., num - 1 is not present).

**Steps:**
- Add all numbers to a HashSet.
- For each number, if num - 1 doesn’t exist, start a new sequence.
- Keep counting num + 1, num + 2, ... until break.
- Track the maximum streak length.

**Java Code:**
```java
public int longestConsecutive(int[] nums) {
    HashSet<Integer> set = new HashSet<>();
    for (int num : nums) set.add(num);

    int maxLen = 0;

    for (int num : set) {
        if (!set.contains(num - 1)) { // sequence start
            int curr = num;
            int len = 1;
            
            while (set.contains(curr + 1)) {
                curr++;
                len++;
            }
            maxLen = Math.max(maxLen, len);
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Insert all elements: O(n)
  * Traverse and count sequences: O(n) (each element checked once)
  * Total: O(n)
- 💾 Space Complexity
  * O(n) for HashSet storage

### Approach 3: Using HashMap (Alternate)

**Idea:**
- Use a HashMap to dynamically merge sequences.
- Each number connects to the length of its current sequence, and merging happens when neighboring sequences exist.

**Steps:**
- For each element, get lengths of left and right neighboring sequences.
- New sequence length = left + right + 1.
- Update boundaries (num - left and num + right) with new length.
- Keep track of the longest sequence.

**Java Code:**
```java
public int longestConsecutive(int[] nums) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int maxLen = 0;

    for (int num : nums) {
        if (map.containsKey(num)) continue;

        int left = map.getOrDefault(num - 1, 0);
        int right = map.getOrDefault(num + 1, 0);
        int len = left + right + 1;

        map.put(num, len);
        map.put(num - left, len);
        map.put(num + right, len);

        maxLen = Math.max(maxLen, len);
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Each element processed once: O(n)
  * HashMap operations: O(1) average
  * Total: O(n)
- 💾 Space Complexity
  * O(n) for HashMap

---

## 6. Justification / Proof of Optimality

- Sorting is simpler but slower (O(n log n)).
- HashSet is optimal and easy to reason about.
- HashMap approach is powerful for merging intervals (advanced variant).
---

## 7. Variants / Follow-Ups

- Longest consecutive subsequence in a sorted array.
- Longest sequence with given difference k.
- Used in problems like “Longest Band” or “Union of consecutive ranges.”
---

## 8. Tips & Observations

- Always check num - 1 to find the start of a sequence.
- HashSet avoids unnecessary duplicate counting.
- The logic applies for negative, positive, and unordered values.
- This is a great example of how to convert O(n log n) sorting logic into O(n) using hashing.
---

<!-- #endregion -->
