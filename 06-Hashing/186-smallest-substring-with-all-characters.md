<!-- #region 186-Smallest Substring with all Characters -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q186: Smallest Substring with all Characters</h1>

## 1. Problem Statement

- Given two strings a and b, find the smallest substring of a that contains all characters of b (including duplicates).
- Rules:
  * If multiple substrings have the same minimum length, return the one with the least starting index
  * If no such substring exists, return "-1"
---

## 2. Problem Understanding

- You need to find a contiguous substring of string a such that:
  * Every character present in b is present in the substring
  * Character frequency matters (if b has two 'o', substring must have two 'o')
  * Among all valid substrings, choose the smallest length
  * If tie → choose earliest starting index
- This is a minimum window substring problem.
---

## 3. Constraints

- 1 <= a.length(), b.length() <= 1000
- Characters may repeat
- Efficient solution required
---

## 4. Edge Cases

- b.length() > a.length() → return "-1"
- a == b
- Characters in b not present in a
- Duplicate characters in b
- Multiple valid windows with same size
---

## 5. Examples

```text
Example 1

Input:

a = "acciojob"
b = "coo"


Output:

"ciojo"

Example 2

Input:

a = "timetopractice"
b = "cot"


Output:

"toprac"
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Substrings)

**Idea:**
- Generate all substrings of a and check whether each contains all characters of b with correct frequency.

**Java Code:**
```java
public static String smallestSubstringBrute(String a, String b) {
    int n = a.length();
    int minLen = Integer.MAX_VALUE;
    String ans = "-1";

    Map<Character, Integer> need = new HashMap<>();
    for (char c : b.toCharArray())
        need.put(c, need.getOrDefault(c, 0) + 1);

    for (int i = 0; i < n; i++) {
        Map<Character, Integer> freq = new HashMap<>();
        for (int j = i; j < n; j++) {
            char c = a.charAt(j);
            freq.put(c, freq.getOrDefault(c, 0) + 1);

            boolean valid = true;
            for (char key : need.keySet()) {
                if (freq.getOrDefault(key, 0) < need.get(key)) {
                    valid = false;
                    break;
                }
            }

            if (valid && (j - i + 1) < minLen) {
                minLen = j - i + 1;
                ans = a.substring(i, j + 1);
            }
        }
    }
    return ans;
}
```

**Complexity (Time & Space):**
- Time: O(n^3) — generating substrings and validating frequency each time
- Space: O(n) — frequency maps
- Not suitable for large inputs.

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Use sliding window:
  * Expand right to include characters
  * Track frequency of required characters
  * When window becomes valid, shrink from left to minimize length
  * Track minimum window and starting index

**Java Code:**
```java
public static String SmallestSubstring(String a, String b) {
    if (b.length() > a.length()) return "-1";

    Map<Character, Integer> need = new HashMap<>();
    for (char c : b.toCharArray())
        need.put(c, need.getOrDefault(c, 0) + 1);

    Map<Character, Integer> window = new HashMap<>();
    int left = 0, formed = 0;
    int minLen = Integer.MAX_VALUE, start = 0;
    int required = b.length();

    for (int right = 0; right < a.length(); right++) {
        char c = a.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);

        if (need.containsKey(c) && window.get(c) <= need.get(c))
            formed++;

        while (formed == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }

            char lChar = a.charAt(left);
            window.put(lChar, window.get(lChar) - 1);

            if (need.containsKey(lChar) && window.get(lChar) < need.get(lChar))
                formed--;

            left++;
        }
    }

    return minLen == Integer.MAX_VALUE ? "-1"
            : a.substring(start, start + minLen);
}
```

**💭 Intuition Behind the Approach:**
- We expand until all required characters are present
- Shrinking ensures minimal window
- formed ensures frequency correctness
- Sliding window avoids recomputation

**Complexity (Time & Space):**
- Time: O(n) — each character enters and exits window once
- Space: O(n) — frequency maps

---

## 7. Justification / Proof of Optimality

- Brute force is too slow
- Sliding window ensures optimal time
- HashMap correctly handles duplicate characters
- Tie-breaking handled by earliest index naturally
---

## 8. Variants / Follow-Ups

- Minimum Window Substring
- Distinct window problems
- Substring with exactly k distinct characters
- Longest substring with frequency constraints
---

## 9. Tips & Observations

- Always use frequency comparison, not just presence
- Modifying formed correctly is crucial
- Sliding window problems follow expand → validate → shrink
---

## 10. Pitfalls

- Ignoring character frequency
- Shrinking window before it becomes valid
- Not handling duplicate characters in b
- Returning any window instead of smallest
- Forgetting "-1" case
---

<!-- #endregion -->
