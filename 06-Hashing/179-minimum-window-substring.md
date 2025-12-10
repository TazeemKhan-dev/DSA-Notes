<!-- #region 179-Minimum Window Substring -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q179: Minimum Window Substring</h1>

## 1. Problem Statement

- Given two strings s and t, return the minimum window substring of s such that every character in t (including duplicates) is included in the window.
- If no such window exists, return an empty string "".
- The answer is guaranteed to be unique.
---

## 2. Problem Understanding

- You are given:
  * A source string s
  * A target string t
- Your task is to find the smallest contiguous substring of s that:
  * Contains all characters of t
  * Respects frequency (if t has 2 'A', window must also have 2 'A')
- This is not about subsequences — the window must be continuous.
---

## 3. Constraints

- 1 <= s.length, t.length <= 100000
- Characters can repeat
- Large input size → brute force will TLE
---

## 4. Edge Cases

- t longer than s → return ""
- s == t → answer is s
- Characters in t not present in s
- Duplicate characters in t
- Single-character strings
---

## 5. Examples

```text
Example 1

Input:
s = "ADOBECODEBANC"
t = "ABC"

Output:
"BANC"

Example 2

Input:
s = "a"
t = "a"

Output:
"a"
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Check all substrings of s and verify if each substring contains all characters of t with correct frequency.

**Steps:**
- Generate all substrings
- For each substring, count frequency
- Compare with frequency of t
- Track minimum valid substring
- ❌ Why Not Used
  * Substrings: O(n^2)
  * Frequency check: O(n)
  * Total: O(n^3) → TLE

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Use a sliding window with two pointers:
  * Expand the window until it becomes valid
  * Shrink it to make it minimum
- Use a HashMap to track:
  * Required character counts (t)
  * Current window counts (s)

**Steps:**
- Build frequency map for t
- Initialize two pointers left = 0, right = 0
- Expand right and include characters in window
- When all required characters are matched:
  * Try shrinking from left
  * Update minimum window
- Continue until right reaches end

**Java Code:**
```java
public static String minWindow(String s, String t) {
    if (s.length() < t.length()) return "";

    Map<Character, Integer> need = new HashMap<>();
    for (char c : t.toCharArray())
        need.put(c, need.getOrDefault(c, 0) + 1);

    int left = 0, count = 0;
    int minLen = Integer.MAX_VALUE, start = 0;
    Map<Character, Integer> window = new HashMap<>();

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);

        if (need.containsKey(c) && window.get(c) <= need.get(c))
            count++;

        while (count == t.length()) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }

            char lChar = s.charAt(left);
            window.put(lChar, window.get(lChar) - 1);
            if (need.containsKey(lChar) && window.get(lChar) < need.get(lChar))
                count--;

            left++;
        }
    }

    return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
}
```

**💭 Intuition Behind the Approach:**
- Expanding ensures all required characters are present
- Shrinking ensures the window is minimal
- count ensures we only shrink when the window is valid
- HashMaps help manage duplicates correctly

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
    * Each character enters and leaves the window once
- 💾 Space Complexity
  * O(k)
    * k = number of unique characters in t

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible due to high complexity
- Sliding window ensures linear traversal
- HashMap correctly handles duplicate character requirements
- This is the optimal and interview-accepted solution
---

## 8. Variants / Follow-Ups

- Longest substring with at most k distinct characters
- Minimum window with distinct characters
- Substring containing all vowels
- Sliding window with frequency constraints
---

## 9. Tips & Observations

- Always compare frequency, not just presence
- Use count == t.length() instead of checking maps repeatedly
- Sliding Window problems often follow expand → validate → shrink
---

## 10. Pitfalls

- ❌ Ignoring character frequency
  * Checking only presence instead of required counts (duplicates in t matter).
- ❌ Shrinking window too early
  * Shrinking before the window becomes valid leads to missing required characters.
- ❌ Incorrect validity check
  * Rechecking the whole frequency map instead of maintaining a count slows down the solution.
- ❌ Forgetting edge case t.length() > s.length()
  * Should return "" immediately.
- ❌ Off-by-one errors in window size
  * Always calculate window size as (right - left + 1).
- ❌ Not updating window count correctly while shrinking
  * Missing the condition where frequency falls below required count.
- ❌ Using == instead of <= in count logic
  * Can break handling of duplicates.
---

<!-- #endregion -->
