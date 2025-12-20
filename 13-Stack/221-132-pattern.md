<!-- #region 221-132 Pattern -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q221: 132 Pattern</h1>

## 1. Problem Statement

- Given an integer array nums of size n, determine if there exists a subsequence of three integers
- nums[i], nums[j], nums[k] such that:
  * i < j < k
  * nums[i] < nums[k] < nums[j]
- Return true if such a pattern exists, otherwise return false.
---

## 2. Problem Understanding

- We are not looking for contiguous elements → this is a subsequence
- Order matters: i < j < k
- Value condition defines the 132 shape:
  * nums[j] is the largest
  * nums[i] is the smallest
  * nums[k] lies between them
- Key difficulty:
  * Brute force checking all triplets is too slow for n = 2 * 10^5.
---

## 3. Constraints

- 1 <= n <= 2 * 10^5
- -10^9 <= nums[i] <= 10^9
- Need solution better than O(N^2)
---

## 4. Edge Cases

- Strictly increasing array → always false
- Strictly decreasing array → always false
- Presence of negative numbers
- Duplicate values
- Very large input size
---

## 5. Examples

```text
Example 1
Input

4
1 2 3 4
Output

false
Explanation

There is no 132 pattern in the sequence.

Example 2
Input

4
3 1 4 2
Output

true
Explanation

There is a 132 pattern in the sequence: [1, 4, 2]

Example 3
Input

4
-1 3 2 0
Output

true
Explanation

There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].
```

---

## 6. Approaches

### Approach 1: Brute Force (TLE)

**Idea:**
- Try all triplets (i, j, k)
- Check condition directly

**Steps:**
- Three nested loops
- Check i < j < k and value condition

**Java Code:**
```java
public boolean find132pattern(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                if (nums[i] < nums[k] && nums[k] < nums[j]) {
                    return true;
                }
            }
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- Direct and obvious, but repeats massive work.

**Complexity (Time & Space):**
- Time: O(N^3) — impossible for large n
- Space: O(1)

### Approach 2: Prefix Min + Middle Check (Still TLE)

**Idea:**
- Fix j as the middle element
- Track smallest value on the left

**Steps:**
- Precompute minLeft[j]
- For each j, search k > j such that
- minLeft[j] < nums[k] < nums[j]

**Java Code:**
```java
public boolean find132pattern(int[] nums) {
    int n = nums.length;
    int[] minLeft = new int[n];
    minLeft[0] = nums[0];

    for (int i = 1; i < n; i++) {
        minLeft[i] = Math.min(minLeft[i - 1], nums[i]);
    }

    for (int j = 1; j < n; j++) {
        for (int k = j + 1; k < n; k++) {
            if (minLeft[j - 1] < nums[k] && nums[k] < nums[j]) {
                return true;
            }
        }
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- We reduce one loop but still search linearly for k.

**Complexity (Time & Space):**
- Time: O(N^2) — still too slow
- Space: O(N)

### Approach 3: Monotonic Stack (Optimal)

**Idea:**
- Traverse from right to left and treat:
  * nums[j] → potential 3
  * Stack elements → potential 2
  * Maintain a variable third → best candidate for 2

**Steps:**
- Initialize third = -∞
- Traverse array from right to left
- If nums[i] < third → 132 found
- While stack top < nums[i]:
  * Update third = stack.pop()
- Push nums[i] to stack

**Java Code:**
```java
public boolean find132pattern(int[] nums) {
    int n = nums.length;
    Stack<Integer> st = new Stack<>();
    int third = Integer.MIN_VALUE;

    for (int i = n - 1; i >= 0; i--) {
        if (nums[i] < third) return true;

        while (!st.isEmpty() && st.peek() < nums[i]) {
            third = st.pop();
        }
        st.push(nums[i]);
    }
    return false;
}
```

**💭 Intuition Behind the Approach:**
- We try to fix nums[j] first (largest value)
- Stack maintains decreasing order → possible nums[k]
- third stores the maximum valid nums[k] seen so far
- If we ever find nums[i] < third, pattern is complete

**Complexity (Time & Space):**
- Time: O(N) — each element pushed and popped once
- Space: O(N) — stack

---

## 7. Justification / Proof of Optimality

- Brute force and prefix-based approaches fail due to time
- Right-to-left monotonic stack cleverly enforces ordering
- This is the standard interview solution for 132 Pattern
---

## 8. Variants / Follow-Ups

- 231 Pattern
- 213 Pattern
- Increasing Triplet Subsequence
- Pattern detection using stacks
---

## 9. Tips & Observations

- Always think which index is fixed first
- Right-to-left traversal is the key insight
- Stack represents candidates for the 2 in 132
- third must be the largest possible valid middle
---

## 10. Pitfalls

- Iterating left-to-right (fails logic)
- Resetting third incorrectly
- Using stack for indices instead of values
- Confusing this with contiguous subarray problems
---

<!-- #endregion -->
