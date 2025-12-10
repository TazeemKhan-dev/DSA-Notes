<!-- #region 181-Substring With K Unique characters -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q181: Substring With K Unique characters</h1>

## 1. Problem Statement

- Given a string s and an integer k, find the length of the longest substring that contains exactly k unique characters.
- If no such substring exists, return -1.
---

## 2. Problem Understanding

- You are asked to find a contiguous substring such that:
  * It contains exactly k distinct characters
  * Among all such substrings, return the maximum possible length
- Important clarifications:
  * “Exactly k” → not less, not more
  * Order matters → substring, not subsequence
  * Only the length is required, not the substring itself
---

## 3. Constraints

- 1 <= |s| <= 100000
- 1 <= k <= 100000
- Large input → need O(n) solution
---

## 4. Edge Cases

- k > number of distinct characters in s → return -1
- k == 0 → invalid (by constraints)
- Single character string
- All characters same
- k == 1 → longest block of same characters
---

## 5. Examples

```text
Example 1

Input:

s = "aabbcc", k = 1


Output:

2


Explanation: "aa", "bb", "cc"

Example 2

Input:

s = "aabbcc", k = 2


Output:

4


Explanation: "aabb", "bbcc"
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Generate all substrings and count unique characters for each.
- ❌ Why Not Used
  * Substrings → O(n^2)
  * Counting unique → O(n)
  * Total → O(n^3) → TLE

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Use a sliding window:
  * Expand window by moving right
  * Track character frequencies in a map
  * If unique characters exceed k, shrink from left
  * If unique characters == k, update maximum length

**Steps:**
- Initialize two pointers left = 0, right = 0
- Use HashMap to track character counts
- Expand window by moving right
- If map size > k, shrink from left
- If map size == k, update answer
- Continue till end

**Java Code:**
```java
public static int longestSubstringKUnique(String s, int k) {
    int n = s.length();
    if (k > n) return -1;

    Map<Character, Integer> map = new HashMap<>();
    int left = 0, maxLen = -1;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);
        map.put(c, map.getOrDefault(c, 0) + 1);

        while (map.size() > k) {
            char lChar = s.charAt(left);
            map.put(lChar, map.get(lChar) - 1);
            if (map.get(lChar) == 0) map.remove(lChar);
            left++;
        }

        if (map.size() == k) {
            maxLen = Math.max(maxLen, right - left + 1);
        }
    }

    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- Sliding window ensures linear traversal
- HashMap size directly tells number of unique characters
- Shrinking keeps the window valid
- We only update answer when condition is exactly k

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
    * Each character enters and exits the window once
- 💾 Space Complexity
  * O(k)
    * At most k characters in the map

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible
- Sliding window is optimal and clean
- Handles large input efficiently
- Common interview pattern
---

## 8. Variants / Follow-Ups

- Longest substring with at most k distinct characters
- Longest substring with all distinct characters
- Minimum window substring
- Distinct window problems
---

## 9. Tips & Observations

- “Exactly k” → update result only when map size equals k
- Use map.size() instead of recounting
- Sliding window problems usually follow expand → validate → shrink
---

## 10. Pitfalls

- ❌ Updating answer when unique characters < k
- ❌ Forgetting to shrink when map size > k
- ❌ Returning 0 instead of -1 when no valid substring
- ❌ Confusing with “at most k” variant
- ❌ Off-by-one errors in window length calculation
---

<!-- #endregion -->
