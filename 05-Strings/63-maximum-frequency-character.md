<!-- #region 63-Maximum Frequency Character -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q63: Maximum Frequency Character</h1>

## 1. Problem Understanding

- Input: A string s containing only lowercase letters.
- Task: Return the character that occurs most frequently in s.
- Tie-breaker: If multiple characters have the same maximum frequency, return the lexicographically smallest one.
---

## 2. Constraints

- 1 < |s| < 100
- Only lowercase English letters (a to z)
---

## 3. Edge Cases

- All characters occur once → return the smallest (a if present)
- Single character string → return that character
- Multiple characters with same max frequency → choose lexicographically smallest
---

## 4. Examples

```text
Example 1
Input: abbbc
Output: b

Example 2
Input: aabbbccc
Output: b

Example 3
Input: xyzxyz
Output: x
Explanation: x, y, z all appear 2 times → return smallest lexicographically → x
```

---

## 5. Approaches

### Approach 1: Frequency Array (Optimal for lowercase letters)

**Idea:**
- Count occurrences of each character using an array of size 26.
- Traverse array to find max frequency.
- Traverse in order 'a' → 'z' to get lexicographically smallest in case of tie.

**Steps:**
- Initialize freq[26] = 0.
- Iterate through string s: freq[s.charAt(i) - 'a']++.
- Initialize maxFreq = 0 and maxChar = 'a'.
- Traverse freq array from 0 to 25:
  * If freq[i] > maxFreq: update maxFreq and maxChar = 'a'+i.
- Return maxChar.

**Java Code:**
```java
static char MaximumFrequencyChar(String s) {
    int[] freq = new int[26];
    for (char ch : s.toCharArray()) {
        freq[ch - 'a']++;
    }

    int maxFreq = 0;
    char maxChar = 'a';
    for (int i = 0; i < 26; i++) {
        if (freq[i] > maxFreq) {
            maxFreq = freq[i];
            maxChar = (char) (i + 'a');
        }
    }

    return maxChar;
}
```

**Complexity (Time & Space):**
- Time: O(n + 26) → O(n), n = length of string
- Space: O(26) → constant space

### Approach 2: HashMap (General approach for any characters)

**Idea:**
- Use a HashMap to store frequency of each character.
- Traverse keys to find max frequency and lexicographically smallest key in case of tie.

**Java Code:**
```java
static char MaximumFrequencyChar(String s) {
    Map<Character, Integer> freqMap = new HashMap<>();
    for (char ch : s.toCharArray()) {
        freqMap.put(ch, freqMap.getOrDefault(ch, 0) + 1);
    }

    char maxChar = s.charAt(0);
    int maxFreq = freqMap.get(maxChar);

    for (char ch : freqMap.keySet()) {
        int f = freqMap.get(ch);
        if (f > maxFreq || (f == maxFreq && ch < maxChar)) {
            maxFreq = f;
            maxChar = ch;
        }
    }

    return maxChar;
}
Works for any characters (not just lowercase)
```

**Complexity (Time & Space):**
- Time: O(n + k), k = number of distinct characters
- Space: O(k)

---

## 6. Justification / Proof of Optimality

- Frequency array is optimal for lowercase letters → constant space + linear time.
- HashMap approach generalizes for any input but slightly slower due to hashing.
---

## 7. Variants / Follow-Ups

- Return all characters with max frequency in lexicographical order.
- Handle uppercase letters or mixed characters.
- Find second most frequent character.
---

## 8. Tips & Observations

- Always traverse array or keys in lexicographical order to handle ties.
- Frequency array is more efficient than sorting the string.
- Avoid counting consecutive characters only — total frequency matters.
---

<!-- #endregion -->
