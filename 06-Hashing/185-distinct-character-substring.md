<!-- #region 185-Distinct Character Substring -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q185: Distinct Character Substring</h1>

## 1. Problem Statement

- Given a string s, find the number of substrings (not necessarily distinct) such that all characters inside the substring are distinct.
- A substring must be contiguous.
---

## 2. Problem Understanding

- You need to count all possible substrings where:
  * No character repeats inside the substring
  * Order matters
  * Substrings with same content but different positions are counted separately
- This is a counting problem, not a max/min length problem.
---

## 3. Constraints

- 1 <= |s| <= 100000
- Large input → brute force will not work
---

## 4. Edge Cases

- String with all same characters
- String with all unique characters
- Single character string
- Repeated patterns like "ababab"
---

## 5. Examples

```text
Example 1

Input:

gffg


Output:

6


Valid substrings:

"g", "gf", "f", "f", "fg", "g"

Example 2

Input:

acciojob


Output:

18
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Substrings)

**Idea:**
- Generate all substrings and check if each substring contains only distinct characters.

**Java Code:**
```java
public static int countDistinctSubstringBrute(String s) {
    int n = s.length();
    int count = 0;

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

            if (valid) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(n^3) — generating all substrings and validating each
- Space: O(1) — fixed-size frequency array
- Not feasible for large input.

### Approach 2: Sliding Window (Optimal ✅)

**Idea:**
- Use a sliding window to maintain a window of unique characters:
  * Expand right
  * If a duplicate appears, shrink from left until window becomes valid
  * At each position right, all substrings ending at right and starting between left and right are valid
- Number of valid substrings added at each step:
  * right - left + 1

**Java Code:**
```java
public static int countDistinctSubstring(String s) {
    int n = s.length();
    Set<Character> set = new HashSet<>();

    int left = 0;
    long count = 0;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);

        while (set.contains(c)) {
            set.remove(s.charAt(left));
            left++;
        }

        set.add(c);
        count += (right - left + 1);
    }
    return (int) count;
}
```

**💭 Intuition Behind the Approach:**
- A window with unique characters guarantees all its substrings are valid
- For each right, every substring starting from left to right is distinct
- Sliding window ensures no rechecking of characters
- This converts a nested problem into linear traversal

**Complexity (Time & Space):**
- Time: O(n) — each character is added and removed once
- Space: O(n) — set stores unique characters in current window

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible due to cubic complexity
- Sliding window avoids redundant checks
- Counting substrings using window length is the key optimization
- This is a standard counting pattern in sliding window problems
---

## 8. Variants / Follow-Ups

- Longest substring with unique characters
- Count substrings with at most k distinct characters
- Count substrings with exactly k distinct characters
- Distinct window problems
---

## 9. Tips & Observations

- This problem is about counting, not max length
- Use right - left + 1 carefully
- Sliding window is reusable across many substring problems
---

## 10. Pitfalls

- Treating this as a subsequence problem
- Forgetting to shrink window when duplicate appears
- Using nested loops instead of sliding window
- Missing the counting logic for each right
---

<!-- #endregion -->
