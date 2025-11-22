<!-- #region 55-Pangrams -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q55: Pangrams</h1>

## 1. Problem Understanding

- You are given a string s, and you need to determine whether it is a pangram — a sentence that contains every letter of the English alphabet at least once.
- Ignore case sensitivity and spaces.
- Return "pangram" if all 26 letters (a–z) are present, otherwise return "not pangram".
---

## 2. Constraints

- 0<∣s∣≤1000
- Each character s[i] ∈ {a–z, A–Z, space}
---

## 3. Edge Cases

- Empty string → "not pangram"
- String with only spaces → "not pangram"
- Mixed case letters should count as same letter
- Long strings with missing one or two letters
- Punctuation is not included here, only spaces and letters
---

## 4. Examples

```text
Example 1:

Input: "We promptly judged antique ivory buckles for the next prize"

Output: pangram

✅ Explanation: Every letter from ‘a’ to ‘z’ is present.

Example 2:

Input: "We promptly judged antique ivory buckles for the prize"

Output: not pangram

❌ Explanation: The string is missing the letter 'x'.
```

---

## 5. Approaches

### Approach 1: Using a Set

**Idea:**
- Convert the string to lowercase.
- Add every character to a set only if it’s a letter.
- If the set’s size becomes 26, it’s a pangram.

**Steps:**
- Initialize an empty set.
- Traverse through each character of the string.
- If the character is an alphabet, add it to the set.
- Finally, check if the size of the set == 26.

**Java Code:**
```java
class Solution {
    public static String isPangram(String s) {
        s = s.toLowerCase();
        Set<Character> letters = new HashSet<>();
        
        for (char c : s.toCharArray()) {
            if (c >= 'a' && c <= 'z')
                letters.add(c);
        }
        
        return letters.size() == 26 ? "pangram" : "not pangram";
    }
}
```

**Complexity (Time & Space):**
- Time: O(n) — single traversal of the string
- Space: O(1) — at most 26 characters stored

### Approach 2: Using Boolean Array

**Idea:**
- Maintain a boolean array of size 26 for each letter.
- Mark each letter encountered as true.
- In the end, check if all are true.

**Steps:**
- Initialize a boolean array of size 26 with false.
- For each letter, mark the corresponding index (c - 'a') as true.
- After processing, check if all entries are true.

**Java Code:**
```java
class Solution {
    public static String isPangram(String s) {
        boolean[] seen = new boolean[26];
        s = s.toLowerCase();
        
        for (char c : s.toCharArray()) {
            if (c >= 'a' && c <= 'z')
                seen[c - 'a'] = true;
        }
        
        for (boolean b : seen) {
            if (!b) return "not pangram";
        }
        return "pangram";
    }
}
```

**Complexity (Time & Space):**
- Time: O(n + 26) ≈ O(n)
- Space: O(1)

### Approach 3: Using Bitmask (Optimized Space)

**Idea:**
- Represent each letter’s presence with a bit in an integer.
- Toggle the bit for each letter found.
- If the final mask has all 26 bits set, it’s a pangram.

**Steps:**
- Initialize mask = 0.
- For each letter, set the bit (1 << (c - 'a')).
- After iteration, compare mask with (1 << 26) - 1.

**Java Code:**
```java
class Solution {
    public static String isPangram(String s) {
        int mask = 0;
        s = s.toLowerCase();
        
        for (char c : s.toCharArray()) {
            if (c >= 'a' && c <= 'z')
                mask |= (1 << (c - 'a'));
        }
        
        return mask == (1 << 26) - 1 ? "pangram" : "not pangram";
    }
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- Using a set is simplest and intuitive.
- The boolean array is slightly faster.
- The bitmask approach is optimal in space and elegant for interview answers.
---

## 7. Variants / Follow-Ups

- ✅ Check pangrammatic lipogram → a sentence missing exactly one letter.
- ✅ Count how many letters are missing to form a pangram.
- ✅ Apply to Unicode alphabets or other languages.
---

## 8. Tips & Observations

- ✅ You can safely ignore spaces and case — always convert to lowercase first.
- ✅ 26 is constant, so any alphabet-based structure (set/boolean array) uses O(1) space.
- ✅ Bitmasking is a smart trick often reused in similar problems like “Check if two strings are anagrams” or “Find unique characters”.
- ✅ If you’re asked to find missing letters, you can easily list them using a simple check on unmarked positions.
- ✅ Common interview follow-up: “Can you do this for non-English alphabets (like Unicode)?” — leads into set-based or dictionary-based solutions.
---

<!-- #endregion -->
