<!-- #region 177-Roll number problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q177: Roll number problem</h1>

## 1. Problem Statement

- You are given an array nums of size N containing numbers from 1 to N.
- Exactly one number is missing and one number appears twice.
- Find the repeating and missing numbers.
---

## 2. Problem Understanding

- The problem gives us numbers from 1 to N, but:
  * One number is repeated
  * One number is missing
- Example:
  * [1, 4, 2, 5, 2]
  * Here:
  * 2 occurs twice → repeating
  * 3 never appears → missing
- We must find both.
---

## 3. Constraints

- 2 <= N <= 100000
- 1 <= nums[i] <= N
- Exactly one missing and one repeating number.
---

## 4. Edge Cases

- Repeating number at the beginning: [2, 2]
- Missing number at the beginning: [2, 3, 4, 4]
- Large N → efficiency matters.
---

## 5. Examples

```text
Example 1
Input: 5
1 4 2 5 2
Output: 2 3

Example 2
Input: 2
2 2
Output: 2 1
```

---

## 6. Approaches

### Approach 1: HashMap Counting (Brute but Efficient)

**Idea:**
- Count frequency of each number.
  * The one with frequency 2 → repeating
  * The one with frequency 0 → missing

**Steps:**
- Build a frequency map.
- Traverse from 1 to N.
- Detect repeating and missing

**Java Code:**
```java
static int[] findRepeatingAndMissingNumbers(int[] nums) {
    HashMap<Integer, Integer> h = new HashMap<>();
    int repeating = -1, missing = -1;

    for (int x : nums)
        h.put(x, h.getOrDefault(x, 0) + 1);

    for (int i = 1; i <= nums.length; i++) {
        if (!h.containsKey(i)) missing = i;
        else if (h.get(i) == 2) repeating = i;
    }

    return new int[]{repeating, missing};
}
```

**💭 Intuition Behind the Approach:**
- We know each number from 1 to N should appear exactly once.
- By counting occurrences:
  * A number appearing twice → the repeated
  * A number not appearing → the missing
- Simple and clear.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) because we traverse array + numbers from 1..N.
- 💾 Space Complexity
  * O(N) because of HashMap.

### Approach 2: Math Formula (Optimal, O(1) Space)

**Idea:**
- Let:
  * S = sum(nums)
  * S_actual = N*(N+1)/2
  * sq = sum of squares(nums)
  * sq_actual = N*(N+1)*(2N+1)/6
- Let:
  * x = repeating
  * y = missing
- We get:
  * x - y = S - S_actual
  * x^2 - y^2 = sq - sq_actual
  * => (x - y)(x + y) = ...
- Solve equations to get x and y.

**Java Code:**
```java
static int[] findRepeatingAndMissingNumbers(int[] nums) {
    long n = nums.length;
    long S = 0, sq = 0;

    for (int x : nums) {
        S += x;
        sq += (long)x * x;
    }

    long S_actual = n * (n + 1) / 2;
    long sq_actual = n * (n + 1) * (2 * n + 1) / 6;

    long diff = S - S_actual;           // x - y
    long sqDiff = sq - sq_actual;       // x^2 - y^2 = (x-y)(x+y)

    long sum = sqDiff / diff;           // x + y

    long x = (diff + sum) / 2;          // repeating
    long y = sum - x;                   // missing

    return new int[]{(int)x, (int)y};
}
```

**💭 Intuition Behind the Approach:**
- We use the fact that with perfect numbers 1..N, the formulas for sum and square sum are known.
- Deviations from actual → tell us:
  * One number added twice
  * One number removed
- This gives two equations to solve for repeating & missing.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
- 💾 Space Complexity
  * O(1) (optimal)

### Approach 3: XOR Method (Also O(1) Space)

**Idea:**
- If we XOR everything:
- X = (1 ⊕ 2 ⊕ 3 ⊕ ... ⊕ N) ⊕ (all elements in array)
- We will get:
- X = missing ⊕ repeating   (because every normal number cancels out)
- Now we have one combined XOR result.
- We must separate missing and repeating.
- We use the rightmost set bit of X to divide numbers into 2 groups:
- Group A → numbers with that bit set
- Group B → numbers without that bit set
- Because missing and repeating differ at that bit,
- they fall into different groups → letting us isolate them.
- Then we XOR again within groups to get individual values.
- After that, we check which one occurs twice to know which is repeating.

**Steps:**
- Compute XOR of:
  * All numbers from 1..N
  * All numbers in array
  * This gives: xor = repeating ⊕ missing
- Extract rightmost set bit:
  * rsb = xor & -xor
- Split numbers into 2 groups:
  * Group 1: numbers having this rsb bit
  * Group 2: numbers NOT having this rsb bit
- XOR inside both groups separately to find two values:
- val1 and val2
- Check which value actually appears twice → repeating
- The other is missing.

**Java Code:**
```java
static int[] findRepeatingAndMissingNumbers(int[] nums) {

    int n = nums.length;
    int xor = 0;

    // Step 1: XOR all nums and numbers from 1..N
    for (int x : nums) xor ^= x;
    for (int i = 1; i <= n; i++) xor ^= i;

    // xor = repeating ^ missing

    // Step 2: Get rightmost set bit
    int rsb = xor & -xor;

    int bucket1 = 0, bucket2 = 0;

    // Step 3: Divide numbers based on rsb
    for (int x : nums) {
        if ((x & rsb) != 0) bucket1 ^= x;
        else bucket2 ^= x;
    }

    for (int i = 1; i <= n; i++) {
        if ((i & rsb) != 0) bucket1 ^= i;
        else bucket2 ^= i;
    }

    // Now bucket1 and bucket2 contain missing and repeating in some order
    int repeating = 0, missing = 0;

    // Step 4: Detect which is repeating
    for (int x : nums) {
        if (x == bucket1) {
            repeating = bucket1;
            missing = bucket2;
            return new int[]{repeating, missing};
        }
    }

    // Else bucket2 is repeating
    repeating = bucket2;
    missing = bucket1;

    return new int[]{repeating, missing};
}
```

**💭 Intuition Behind the Approach:**
- XOR has a unique property:
    * a ⊕ a = 0
    * a ⊕ 0 = a
- So when all correct numbers cancel each other out,
- we are left with:
  * repeating ⊕ missing
- The rightmost set bit ensures the two target numbers lie in different buckets,
- allowing us to isolate them—like splitting data based on a unique fingerprint bit.
- This avoids extra memory, unlike HashMap.
- Pitfalls
  * Forgetting to check which bucket is repeating → wrong answer
  * Not using the rightmost set bit — needed for separation
  * Using overflow-prone math formulas — XOR avoids that

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) (single pass XOR operations)
- 💾 Space Complexity
  * O(1) (no extra arrays / maps)
- Why?
  * We use only a few integers (xor buckets).
  * XOR cancels extra values automatically — very memory-efficient.

---

## 7. Justification / Proof of Optimality

- We covered all valid approaches:
- HashMap → simple, intuitive
- Math → optimal, constant space
- XOR → optimal, no overflow issues
- Given constraints (N <= 1e5), all approaches run in time.
---

## 8. Variants / Follow-Ups

- Find missing numbers when k numbers missing
- Find only duplicate numbers
- Find missing ranges
- Detect missing + repeating when array starts from 0
---

## 9. Tips & Observations

- Simple sum formula often works best.
- Be careful: sum might overflow → use long.
- HashMap is the most readable but not optimal for memory.
---

## 10. Pitfalls

- Doing missing = sum_expected - sum_array is WRONG when duplicates exist.
- Not using long → overflow.
- Forgetting that exactly one number repeats twice.
---

<!-- #endregion -->
