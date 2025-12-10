<!-- #region 180-Distinct Window -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q180: Distinct Window</h1>

## 1. Problem Statement

- Given a string s, find the smallest substring (window) that contains all distinct characters present in the entire string s.
---

## 2. Problem Understanding

- You are given one string only.
- Steps to understand the problem:
  * First, determine how many distinct characters exist in the whole string s.
  * Then, find the smallest contiguous substring of s that contains all those distinct characters at least once.
- This is not about unique characters inside the window, but about covering all unique characters of the full string.
---

## 3. Constraints

- 1 <= |s| <= 10000
- Characters can repeat
- Need an efficient solution → brute force will TLE
---

## 4. Edge Cases

- String with all same characters → answer length = 1
- String already has all distinct characters → answer is full string
- Single character string
- Repeated patterns like "aaaaab"
---

## 5. Examples

```text
Example 1

Input:
s = "aabcbcdbca"

Output:
"dbca"

Explanation:
Distinct characters in s = {a, b, c, d}
Smallest substring covering all = "dbca"

Example 2

Input:
s = "aaab"

Output:
"ab"

Explanation:
Distinct characters = {a, b}
Smallest valid window = "ab"
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Generate all substrings
- Check if each substring contains all distinct characters of s
- Track the minimum length substring
- ❌ Why Not Used
  * Substrings → O(n^2)
  * Checking each substring → O(n)
  * Total → O(n^3) → TLE

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Count number of distinct characters in entire string → required
- Use sliding window with two pointers
- Expand window until it contains all required characters
- Shrink window to minimize length

**Steps:**
- Build a set or map to count distinct characters in s
- Initialize two pointers left = 0, right = 0
- Expand right, add characters to window map
- When window contains all distinct characters:
  * Try shrinking from left
  * Update minimum window
- Continue until end of string

**Java Code:**
```java
public static String distinctWindow(String s) {
    int n = s.length();

    Set<Character> set = new HashSet<>();
    for (char c : s.toCharArray()) set.add(c);
    int required = set.size();

    Map<Character, Integer> window = new HashMap<>();
    int left = 0, formed = 0;
    int minLen = Integer.MAX_VALUE, start = 0;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);

        if (window.get(c) == 1) formed++;

        while (formed == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }

            char lChar = s.charAt(left);
            window.put(lChar, window.get(lChar) - 1);
            if (window.get(lChar) == 0) formed--;

            left++;
        }
    }

    return s.substring(start, start + minLen);
}
```

**💭 Intuition Behind the Approach:**
- We only care whether each distinct character appears at least once
- Sliding window allows linear traversal
- Shrinking ensures minimal window size
- HashMap helps track current window state efficiently

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
    * Each character enters and exits the window once
- 💾 Space Complexity
  * O(k)
    * k = number of distinct characters in s

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible for large strings
- Sliding window ensures optimal linear time
- HashMap cleanly tracks presence and removal
- This is the standard interview-accepted solution
---

## 8. Variants / Follow-Ups

- Minimum Window Substring (with frequency constraints)
- Longest substring with all distinct characters
- At most k distinct characters
- Substring covering a given set of characters
---

## 9. Tips & Observations

- First step is always counting distinct characters of the entire string
- Presence check is enough (frequency > 1 doesn’t matter)
- Sliding window problems usually follow expand → validate → shrink
---

## 10. Pitfalls

- ❌ Confusing this with minimum window substring (frequency does NOT matter here)
- ❌ Forgetting to precompute total distinct characters
- ❌ Not shrinking window after it becomes valid
- ❌ Off-by-one error in window size calculation
- ❌ Using nested loops instead of sliding window
---

<!-- #endregion -->
