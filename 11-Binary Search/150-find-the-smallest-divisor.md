<!-- #region 150-Find the smallest divisor -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q150: Find the smallest divisor</h1>

## 1. Problem Understanding

- You are given:
- An array nums[]
- An integer limit
- You must find the smallest positive integer divisor d such that:
  * ceil(nums[0] / d)
  * + ceil(nums[1] / d)
  * + ...
  * + ceil(nums[n-1] / d)
  * <= limit
- You must minimize the divisor.
- This is Binary Search on Answer.
---

## 2. Constraints

- 1 ≤ nums.length ≤ 5 * 10^4
- 1 ≤ nums[i] ≤ 10^6
- nums.length ≤ limit ≤ 10^6
- Divisor is a positive integer
- Divisor range: 1 → max(nums[])
---

## 3. Edge Cases

- limit very large → smallest divisor = 1
- limit smaller than n → impossible unless divisor very large
- nums containing large values (up to 10^6)
- Single-element array
- All elements equal
---

## 4. Examples

```text
Example 1

Input: nums = [1,2,3,4,5], limit=8
Output: 3

Example 2

Input: nums = [8,4,2,3], limit=10
Output: 2

Example 3

Input: nums = [8,4,2,3], limit=4
Output: 2
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Try all divisors from 1 to max(nums) and check when the sum becomes ≤ limit.

**Steps:**
- Compute maximum element (maxD)
- For divisor d from 1 to maxD:
  * compute sum of ceil(nums[i]/d)
  * if sum ≤ limit → return d

**Java Code:**
```java
public int smallestDivisor(int[] nums, int limit) {
    int max = 0;
    for (int x : nums) max = Math.max(max, x);

    for (int d = 1; d <= max; d++) {
        if (isPossible(nums, d, limit)) return d;
    }
    return -1;
}

private boolean isPossible(int[] nums, int d, int limit) {
    int sum = 0;
    for (int x : nums) {
        sum += (x + d - 1) / d; // ceil division
        if (sum > limit) return false;
    }
    return sum <= limit;
}
```

**💭 Intuition Behind the Approach:**
- Check every possible divisor. Very slow for large max element.

**Complexity (Time & Space):**
- O(max(nums) * n) → Too slow
- Space: O(1)

### Approach 2: Binary Search (l < h) (Range Shrinking)

**Idea:**
- ✔ We shrink search space until one divisor remains
- ✔ We do NOT check equality during loop
- ✔ After loop, verify the candidate
- Search space = 1 → max(nums)
- We want the first divisor where condition becomes valid.

**Steps:**
- low = 1, high = max(nums)
- While low < high:
  * mid = (low + high) / 2
  * If mid is valid → move left
  * Else → move right
- After loop, check low

**Java Code:**
```java
public int smallestDivisor(int[] nums, int limit) {
    int low = 1;
    int high = 0;
    for (int x : nums) high = Math.max(high, x);

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (isPossible(nums, mid, limit)) {
            high = mid; // keep mid
        } else {
            low = mid + 1;
        }
    }

    return low;
}
private boolean isPossible(int[] nums, int d, int limit) {
    int sum = 0;

    // Loop through every element in the array
    for (int x : nums) {

        // Divide x by d and take CEIL of the division
        // (x + d - 1) / d is the standard integer trick for ceil(x / d)
        sum += (x + d - 1) / d;

        // If sum already exceeds limit, we can stop early
        // because divisor d is NOT valid
        if (sum > limit) {
            return false;
        }
    }

    // If we finish the loop without exceeding limit,
    // divisor d is valid.
    return sum <= limit;
}

```

**💭 Intuition Behind the Approach:**
- Shrink the possible divisor space to the smallest possible valid divisor.

**Complexity (Time & Space):**
- O(n * log(max(nums)))
- Space: O(1)

### Approach 3: Binary Search (l <= h) (Classic Search Style)

**Idea:**
- ✔ Explicitly finds smallest valid mid
- ✔ Checks all possibilities in binary-search manner
- Use classic binary search and store candidate answer when valid.

**Steps:**
- Initialize low=1, high=max element
- mid = (low + high)/2
- If mid works → store as candidate, search left
- If mid doesn't work → search right

**Java Code:**
```java
public int smallestDivisor(int[] nums, int limit) {
    int low = 1;
    int high = 0;
    for (int x : nums) high = Math.max(high, x);

    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (isPossible(nums, mid, limit)) {
            ans = mid;
            high = mid - 1;  // try smaller divisor
        } else {
            low = mid + 1;   // need larger divisor
        }
    }
    return ans;
}

private boolean isPossible(int[] nums, int d, int limit) {
    int sum = 0;
    for (int x : nums) {
        sum += (x + d - 1) / d;
        if (sum > limit) return false;
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Binary search tries divisors and narrows down the smallest valid.

**Complexity (Time & Space):**
- O(n * log(max(nums)))
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Binary Search works because:
  * sum(ceil(nums[i]/d)) is a monotonic decreasing function of divisor d.
- Meaning:
  * If divisor increases → sum decreases
  * If divisor decreases → sum increases
- So the valid/invalid pattern looks like:
  * invalid invalid invalid valid valid valid
- Binary search perfectly finds the first valid divisor.
---

## 7. Variants / Follow-Ups

- Koko eating bananas (minimum eating speed)
- Shipping packages (minimum capacity)
- Split array largest sum
- Painter partition
- Minimum time to complete tasks
- All follow same pattern.
---

## 8. Tips & Observations

- ALWAYS use ceil division: (x + d - 1) / d
- Divisor range always begins from 1, not 0
- Upper bound = max element
- isPossible() must stop early when sum exceeds limit
- (l < h) → shrinking to minimal valid
- (l <= h) → storing candidate while searching
- Classic BSOA pattern: minimize value satisfying condition
---

<!-- #endregion -->
