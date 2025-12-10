<!-- #region 183-Longest Substring with Unique Characters -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q183: Longest Substring with Unique Characters</h1>

## 1. Problem Statement

- Given a string s, find the length of the longest substring such that each character appears at most once.
- A substring must be contiguous.
---

## 2. Problem Understanding

- You need to find the longest contiguous part of the string where:
- No character repeats
- Order matters
- Only the length is required, not the substring itself
- This is a classic sliding window problem.
---

## 3. Constraints

- 0 <= s.length() <= 10^4
- String can contain alphabets, digits, spaces, and symbols
---

## 4. Edge Cases

- Empty string → answer 0
- String with all same characters → answer 1
- String with all unique characters → answer is full length
- String containing spaces or symbols
- Mixed characters
---

## 5. Examples

```text
Example 1

Input:

xyzxyzyy


Output:

3


Explanation: "xyz"

Example 2

Input:

xxxxxx


Output:

1
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Substrings)

**Idea:**
- Generate all possible substrings and check if all characters in each substring are unique.

**Java Code:**
```java
public static int longestSubstringBrute(String s) {
    int n = s.length();
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            boolean[] seen = new boolean[256];
            boolean valid = true;

            for (int k = i; k <= j; k++) {
                char c = s.charAt(k);
                if (seen[c]) {
                    valid = false;
                    break;
                }
                seen[c] = true;
            }

            if (valid) {
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(n^3) — generating all substrings and checking uniqueness for each
- Space: O(1) — fixed-size character array (constant 256)

### Approach 2: Sliding Window + HashSet (Optimal ✅)

**Idea:**
- Use a sliding window:
  * Expand right pointer and add characters to a set
  * If a duplicate appears, move left until the duplicate is removed
  * Track maximum window size

**Java Code:**
```java
public static int longestSubstring(String s) {
    int n = s.length();
    Set<Character> set = new HashSet<>();

    int left = 0, maxLen = 0;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);

        while (set.contains(c)) {
            set.remove(s.charAt(left));
            left++;
        }

        set.add(c);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- A substring with unique characters must not contain duplicates
- Sliding window allows us to expand and shrink efficiently
- HashSet provides fast lookup and removal
- Each character is added and removed at most once

**Complexity (Time & Space):**
- Time: O(n) — each character enters and leaves the window once
- Space: O(n) — in worst case, all characters are unique and stored in the set

---

## 7. Justification / Proof of Optimality

- Brute force is inefficient for large strings
- Sliding window achieves linear time
- HashSet ensures uniqueness efficiently
- This is a standard interview solution
---

## 8. Variants / Follow-Ups

- Longest substring with at most k distinct characters
- Longest substring with exactly k distinct characters
- Minimum window substring
- Distinct window problems
---

## 9. Tips & Observations

- Sliding window is the key pattern here
- Use HashSet when frequency does not matter
- For index tracking, HashMap with last seen index can also be used
---

## 10. Pitfalls

- Treating this as a subsequence problem
- Forgetting to remove characters while shrinking the window
- Resetting the window completely instead of sliding
- Using nested loops unnecessarily
---

<!-- #endregion -->
