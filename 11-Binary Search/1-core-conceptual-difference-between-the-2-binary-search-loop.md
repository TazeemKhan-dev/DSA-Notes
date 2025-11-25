<!-- #region 1-core conceptual difference between the 2 binary-search loop -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q1: core conceptual difference between the 2 binary-search loop</h1>

## 1. Problem Understanding

- You want to understand the core conceptual difference between the 2 binary-search loop styles:
  * while (low < high)
  * while (low <= high)
- The real question you had:
- “When there’s only 1 element left, aren’t ‘element’ and ‘candidate’ the same?
- Why do we sometimes STOP, and sometimes CHECK that single element?”
- This is exactly where your confusion was.
---

## 2. Constraints

- These rules apply to all binary-search problems, including:
  * searching for a target
  * finding a minimum/maximum
  * finding pivot/peak
  * BS on answer problems (aggressive cows, painters partition etc.)
  * rotated sorted array problems
- You must choose the correct loop condition depending on the nature of the problem.
---

## 3. Edge Cases

- You must understand these cases:
  * When the last remaining index is always the answer
  * When the last remaining index might OR might not be the answer
  * When you need to examine the last element
  * When you only need to narrow down to the last element
  * These differences define which loop condition is correct.
---

## 4. Examples

```text
I will use two simple examples later in the notes to illustrate the difference:

Find minimum in rotated array

Search for a target in sorted or rotated array

These will prove why (low < high) vs (low <= high) matter.
```

---

## 5. Approaches

### Approach 1: (low < high): Range Shrinking (Binary Search on Answer)

**Idea:**
- You are NOT looking for a specific element/value at mid.
- You are just trying to shrink the search space until only ONE possible index remains.
- Once only one index remains, that index must be the answer.
- In simple language
  * "The answer will reveal itself if I keep reducing the range."

**Steps:**
- You STOP when:
  * low == high
- Because at that moment:
- You have EXACTLY ONE candidate
- That candidate is guaranteed to be the correct answer
- You DON’T need to check it manually

**Java Code:**
```java
while (low < high) {
    int mid = (low + high) / 2;

    if (condition mid belongs right half)
        low = mid + 1;
    else
        high = mid;
}

return nums[low];
```

**💭 Intuition Behind the Approach:**
- You aren’t searching for equality
- You don’t need to inspect the final element
- The loop narrows to THE answer
- The logic itself ensures the final index is correct
- Where used
  * Find minimum in rotated array
  * Find peak
  * BS on answer (aggressive cows, allocate books)
  * Find first bad version (monotonic boolean)

**Complexity (Time & Space):**
- Time → O(log N)
- Space → O(1)

### Approach 2: (low <= high): Classic Search (Search for a Target or Condition)

**Idea:**
- Here you ARE looking for a specific value or satisfying a condition.
- Meaning:
  * The last element might be your answer
  * You MUST check the final element
  * The loop stops only when every index has been considered
- In simple language
  * "Even if ONE index remains, I must CHECK it, because it might or might not be my answer."
- You STOP when:
  * low > high

**Java Code:**
```java
while (low <= high) {
    int mid = (low + high) / 2;

    if (nums[mid] == target) return mid;

    if (nums[mid] < target)
        low = mid + 1;
    else
        high = mid - 1;
}

return -1;
```

**💭 Intuition Behind the Approach:**
- You might find the answer at ANY position
- Even the last remaining element must be tested
- You cannot stop early
- You need to use equality checks (==)
- Where used
  * Binary search to find a number
  * Search in rotated array
  * Lower bound / upper bound
  * First/last occurrence
  * Segment search problems

**Complexity (Time & Space):**
- Time → O(log N)
- Space → O(1)

---

## 6. Justification / Proof of Optimality

- Why both loops seem similar (they both test mid)
- But they are not solving the same TYPE of problem.
- The two questions clarify this:
- 🟢 Key Question 1: “Do I want to STOP when only 1 element is left?”
  * Use (low < high)
  * Because that 1 element is guaranteed to be the answer.
  * You stop without checking it.
- 🔵 Key Question 2: “Even when ONE element is left, MUST I CHECK it?”
  * Use (low <= high)
  * Because that last element might or might not be the answer.
  * You must verify it.
---

## 7. Variants / Follow-Ups

- These loop conditions appear in:
  * search for min/max
  * rotated array min
  * rotated array search
  * peak element search
  * function optimization
  * BS on answer problems
  * target index search
---

## 8. Tips & Observations

- Both loops (low < high and low <= high) DO check the mid.
- But they are used for COMPLETELY DIFFERENT GOALS.
- So the question is NOT:
- “Am I traversing mid or not?”
- You ALWAYS traverse mid.
- The REAL question is:
- “Do I want to STOP when only 1 element is left, or do I want to CHECK that 1 element too?”
- This changes EVERYTHING.
- ✔ Case 1: (low < high)
  * → "I want the loop to end when only ONE candidate is left."
  * Purpose: REDUCE the range.
  * You are NOT searching for an exact match.
  * Example
    * find minimum
    * find peak
    * find first bad version (binary reduce)
    * maximize or minimize value using BS (BS on answer)
    * Here the algorithm guarantees:
    * There is exactly one answer
    * When we narrow to that one index → DONE
  * That's why we stop when:
  * low == high
- We DO NOT need to check mid for equality.
- ✔ Case 2: (low <= high)
  * → "Even when ONE element is left, I MUST CHECK it."
  * Purpose: SEARCH for a target
  * or search for a condition.
  * Examples:
    * search element in array
    * search pivot index
    * search lower bound
    * search upper bound
    * search element in rotated array
    * Here the answer may be anywhere, and even the LAST remaining element must be checked.
    * So we stop only when:
    * low > high
  * Because the last remaining element might be the answer.

- **"When 1 element left, element and candidate are same, so why difference?"**
    - ✔ “One candidate left” means
      * → The algorithm has mathematically proven that this is the answer.
    - ✔ “One element left” means
      * → It is just the last element in the range.
      * → It may or may not be the answer.
      * → Equality must be checked.
    - They only LOOK the same, but the meaning is completely different.

- **When you think “candidate = element”, think this:**
    - In (low < high) →
      * The algorithm GUARANTEES the last index is correct.
    - In (low <= high) →
      * The last index is simply the only one left untested.

- **Stopping and checking are DIFFERENT operations.**
    - (low < high) → you stop and declare the answer
    - (low <= high) → you inspect the last element

- **FINAL SUMMARY YOU MUST MEMORIZE**
    - 🔥 Use low < high ONLY IF:
      * The algorithm doesn't check mid for equality
      * You are shrinking the space
      * The last index is automatically the answer
    - 🔥 Use low <= high ONLY IF:
      * You might find your answer ANYWHERE
      * You need to check the last element
      * You rely on ==, >=, <= conditions
---

<!-- #endregion -->
