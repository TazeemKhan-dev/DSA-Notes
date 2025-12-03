<!-- #region 169-Successful Pairs of Spells and Potions -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q169: Successful Pairs of Spells and Potions</h1>

## 1. Problem Statement

- You are given two positive integer arrays spells and potions, of length n and m respectively, where spells[i] represents the strength of the ith spell and potions[j] represents the strength of the jth potion.
- You are also given an integer success. A spell and potion pair is considered successful if the product of their strengths is at least success.
- Return an integer array pairs of length n where pairs[i] is the number of potions that will form a successful pair with the ith spell.
---

## 2. Problem Understanding

- This problem asks:
  * For each spell, how many potions create a product ≥ success?
- Important insights:
  * For a given spell s, a potion p is successful if:
    * s * p >= success
    * ⇒ p >= success / s
  * This means:
  * For each spell, we must count how many potions are ≥ threshold.
- Since potions may be up to 100,000 in size, and n, m are up to 100,000:
- Brute force O(n*m) = 10^10 → TOO SLOW
- Instead:
  * Sort potions
  * For each spell, compute:
    * requiredPotion = ceil(success / spell)
  * Binary search first index ≥ requiredPotion
  * Count = (total potions - index)
- This is Binary Search on sorted potions
- NOT Binary Search on Answer (BSOA).
---

## 3. Constraints

- 1 <= n, m <= 100000
- Spell & potion strengths ≤ 100000
- 1 <= success <= 10^10
- Large success value requires long multiplication/division
- Potions array needs sorting
---

## 4. Edge Cases

- If spell = 0 → always 0 pairs
- If spell[i] > success → ALL potions work
- If requiredPotion is greater than max potion → count = 0
- Integer overflow must be prevented → use long
- threshold = ceil(success / spell) must be computed carefully
---

## 5. Examples

```text
Input:
spells = [5,1,3]
potions = [1,2,3,4,5]
success = 7

Output: [4, 0, 3]
text
Copy code
Input:
spells = [3,1,2]
potions = [8,5,8]
success = 16

Output: [2, 0, 2]
```

---

## 6. Approaches

### Approach 1: Brute Force (Too slow)

**Idea:**
- Compute product for every pair.

**Steps:**
- For each spell
- For each potion
- Check if product ≥ success

**💭 Intuition Behind the Approach:**
- Simple but inefficient.

**Complexity (Time & Space):**
- Time: O(n * m) — impossible for 100k × 100k
- Space: O(1)

### Approach 2: Sort + Binary Search (Optimal) 

**Idea:**
- For each spell, the condition is:
  * spell[i] * potion[j] >= success
  * → potion[j] >= success / spell[i]
- So we can binary search the first valid potion.

**Steps:**
- Sort potions
- For each spell:
  * compute minRequired = ceil(success / spell)
  * binary search for first index where potions[j] ≥ minRequired
  * count = m - index
- Store count in result array

**Java Code:**
```java
class Solution {
    public int[] successfulPairs(int[] spells, int[] potions, long success) {
        Arrays.sort(potions);
        int m = potions.length;
        int n = spells.length;

        int[] ans = new int[n];

        for (int i = 0; i < n; i++) {
            long needed = (success + spells[i] - 1) / spells[i]; // ceil division

            int idx = lowerBound(potions, (int)needed);
            ans[i] = (idx == m) ? 0 : (m - idx);
        }

        return ans;
    }

    private int lowerBound(int[] arr, int target) {
        int low = 0, high = arr.length - 1;
        int ans = arr.length;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] >= target) {
                ans = mid;
                high = mid - 1;
            } else low = mid + 1;
        }

        return ans;
    }
}
```

**💭 Intuition Behind the Approach:**
- Sorting lets us treat potions as a monotonic list.
- For a fixed spell, all potions above a certain value work.
- So we only need to find the first valid potion.
- Binary search makes each query O(log m), making total complexity efficient.

**Complexity (Time & Space):**
- Time:
  * Sorting: O(m log m)
  * For each spell: binary search O(log m)
  * Total: O(m log m + n log m)
  * Why:
    * Sorting dominates, then each spell is processed in log time.
- Space:
  * O(1) extra (besides output array)

---

## 7. Justification / Proof of Optimality

- Binary search works because after sorting, the potion array is monotonic:
  * potions smaller than threshold → all invalid
  * potions ≥ threshold → all valid
- So a lower bound search perfectly captures this transition.
---

## 8. Variants / Follow-Ups

- Count pairs with sum ≥ target
- Count pairs with product ≤ target
- Find number of elements ≥ X for many queries
- Two arrays forming successful pairs based on multiplication constraints
---

## 9. Tips & Observations

- Use long for multiplication/division with success
- Use ceil(success / spell) safely:
  * (success + spell - 1) / spell
- Use built-in Arrays.binarySearch only if you understand its behavior
- otherwise custom lowerBound is cleaner
- Sorting potions once is key to efficiency
---

## 10. Pitfalls

- Using int for success → overflow
- Not sorting potions
- Incorrect ceil division
- Forgetting to subtract index from total length
- Missing edge case where no potion is strong enough
---

<!-- #endregion -->
