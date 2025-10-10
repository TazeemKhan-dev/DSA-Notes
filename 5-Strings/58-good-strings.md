<!-- #region 58-Good Strings -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q58: Good Strings</h1>

## 1. Problem Understanding

- You are given a set of distinct characters S and an array of strings A.
- A string in A is called “good” if all its characters are present in S.
- We must count how many strings in A are good.
---

## 2. Constraints

- 1 ≤ T ≤ 10
- 1 ≤ |S| ≤ 26
- 1 ≤ N ≤ 1000
- 1 ≤ |A[i]| ≤ 1000
- Characters are lowercase English letters.
---

## 3. Edge Cases

- All strings are good (when all characters in A exist in S).
- No string is good (when none of the characters match).
- Empty S → all counts = 0.
- Single-character strings (should still be valid).
- Large test cases (N = 1000, |A[i]| = 1000) → efficiency matters.
---

## 4. Examples

```text
Example 1:
Input:

1
abc
4
ab
abd
cacb
cabef


Output:

2


Explanation:
✅ “ab”, “cacb” are good — all letters are within {a, b, c}.
❌ “abd”, “cabef” contain disallowed characters (d, e, f).

Example 2:
Input
1
pq
3
pqqqpp
abc
rst
Output:
1
Explanation:
✅ “pqqqpp” is good.
❌ “abc” and “rst” have letters not in {p, q}.
```

---

## 5. Approaches

### Approach 1: Brute Force (Using String.contains())

**Idea:**
- For each string in A, check every character using s.contains(char) to verify if it exists in the set string S.

**Steps:**
- Loop through all strings in A.
- For each string, loop through its characters.
- If any character is not in S, mark it as invalid.

**Java Code:**
```java
static int goodStrings(String s, String[] A, int n) {
    int c = 0;
    for (int i = 0; i < A.length; i++) {
        String z = A[i];
        boolean f = true;
        for (int j = 0; j < z.length(); j++) {
            if (!s.contains(String.valueOf(z.charAt(j)))) {
                f = false;
                break;
            }
        }
        if (f) c++;
    }
    return c;
}
```

**Complexity (Time & Space):**
- Time: O(N × |A[i]| × |S|)
- Space: O(1)
- ⚠️ Drawback:
- Repeated .contains() makes it inefficient for large strings.

### Approach 2: Using HashSet (Optimized O(1) Lookup)

**Idea:**
- Store characters of S in a HashSet for O(1) lookup per character.

**Steps:**
- Convert all characters of S into a HashSet.
- For each string in A, check every character’s existence using the HashSet.
- Count if all characters are found.

**Java Code:**
```java
static int goodStrings(String s, String[] A, int n) {
    Set<Character> set = new HashSet<>();
    for (char ch : s.toCharArray()) set.add(ch);

    int count = 0;
    for (String word : A) {
        boolean good = true;
        for (char ch : word.toCharArray()) {
            if (!set.contains(ch)) {
                good = false;
                break;
            }
        }
        if (good) count++;
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N × |A[i]|)
- Space: O(|S|)
- ✅ Advantages:
  * O(1) membership check per character.
  * Simple and very readable.
  * Great practical balance between simplicity and efficiency.

### Approach 3: Bitmasking (Most Optimal)

**Idea:**
- Represent the set S and each word as a bitmask of 26 bits (for each lowercase letter).
- If all bits of a word exist in S, the word is “good”.

**Steps:**
- Create a bitmask for S → set bits for each letter.
- For each word:
  * Create its bitmask.
  * Check (maskWord & ~maskS) == 0.
- If true, increment count.

**Java Code:**
```java
static int goodStrings(String s, String[] A, int n) {
    int maskS = 0;
    for (char ch : s.toCharArray()) {
        maskS |= (1 << (ch - 'a'));
    }

    int count = 0;
    for (String word : A) {
        int maskW = 0;
        for (char ch : word.toCharArray()) {
            maskW |= (1 << (ch - 'a'));
        }

        if ((maskW & ~maskS) == 0) // all chars in S
            count++;
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N × |A[i]|)
- Space: O(1)
- ✅ Advantages:
- Extremely fast (bitwise operations = O(1)).
- Ideal when only lowercase letters are used.
- No extra data structures.

---

## 6. Justification / Proof of Optimality

- Approach 1: Easiest to understand but slow due to O(n²) checks.
- Approach 2: Best balance between clarity and performance — widely used in real-world DSA problems.
- Approach 3: Most efficient (constant space and O(1) bit-level checks), perfect for large test cases or competitive programming.
---

## 7. Variants / Follow-Ups

- Count bad strings instead of good ones.
- Return the list of good strings instead of count.
- Handle mixed-case alphabets or Unicode (extend bitmask to HashSet).
- Similar problems:
  * “Check if two strings are anagrams”
  * “Find common characters across all strings”
  * “Pangram Check”
---

## 8. Tips & Observations

- Prefer HashSet for flexible character sets.
- Use Bitmasking for only lowercase alphabets → super fast O(1).
- Precompute bitmasks when handling multiple test cases.
- Avoid using String.contains() in nested loops — it’s O(n²).
- This problem teaches character presence checking — a fundamental subskill for substring, anagram, and frequency-based problems.
---

<!-- #endregion -->
