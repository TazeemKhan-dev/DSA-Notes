<!-- #region 0-Recursion on Strings -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Recursion on Strings</h1>

## 1. Problem Understanding

- Recursion on strings involves processing one character at a time, reducing the string in each call.
- Each recursive step handles a smaller substring (like s.substring(1) or index increment).
- Common operations:
  * Traversal / Printing
  * Reversal
  * Character search or count
  * Subsequence / Subset generation
  * Permutations
  * Encoding or decoding patterns
---

## 2. Constraints

- String length = n
- String is immutable → each operation may create new substrings
- Base case: when i == n or s.length() == 0
- Avoid excessive substring creation for efficiency
---

## 3. Edge Cases

- Empty string ""
- Single character strings
- Repeated characters (for permutations)
- Case sensitivity (e.g., ‘A’ vs ‘a’)
- Palindromic strings
---

## 4. Examples

```text
Input: "abc" → Output (print): a b c

Input: "abc" → Reverse: "cba"

Input: "abc" → Subsets: ["", "a", "b", "c", "ab", "ac", "bc", "abc"]

Input: "abc" → Permutations: ["abc", "acb", "bac", "bca", "cab", "cba"]
```

---

## 5. Approaches

### Approach 1: Character Processing (Traversal)

**Idea:**
- Process one character and move forward.

**Java Code:**
```java
void process(String s, int i) {
    if (i == s.length()) return;
    System.out.print(s.charAt(i) + " ");
    process(s, i + 1);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 2: Reverse a String

**Idea:**
- Process from end to start recursively.

**Java Code:**
```java
String reverse(String s) {
    if (s.length() == 0) return "";
    return reverse(s.substring(1)) + s.charAt(0);
}
```

**Complexity (Time & Space):**
- Time: O(n²) → due to substring creation
- Space: O(n)
- ✅ Optimization: use character array instead of substring for O(n).

### Approach 3: Count Characters

**Idea:**
- Count characters recursively using index.

**Java Code:**
```java
int count(String s, int i) {
    if (i == s.length()) return 0;
    return 1 + count(s, i + 1);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 4: Count Specific Character Occurrences

**Idea:**
- Increment count when a match is found.

**Java Code:**
```java
int countChar(String s, int i, char ch) {
    if (i == s.length()) return 0;
    int count = (s.charAt(i) == ch) ? 1 : 0;
    return count + countChar(s, i + 1, ch);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 5: Remove Specific Character

**Idea:**
- Build new string excluding given character.

**Java Code:**
```java
String removeChar(String s, char ch, int i) {
    if (i == s.length()) return "";
    if (s.charAt(i) == ch) return removeChar(s, ch, i + 1);
    return s.charAt(i) + removeChar(s, ch, i + 1);
}
```

**Complexity (Time & Space):**
- Time: O(n²) due to string concatenation
- Space: O(n)
- ✅ Use StringBuilder for O(n) optimized version.

### Approach 6: Check Palindrome

**Idea:**
- Compare start and end characters recursively.

**Java Code:**
```java
boolean isPalindrome(String s, int l, int r) {
    if (l >= r) return true;
    if (s.charAt(l) != s.charAt(r)) return false;
    return isPalindrome(s, l + 1, r - 1);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 7: Subsequence Generation

**Idea:**
- For each character → include or exclude.

**Java Code:**
```java
void subsequences(String s, String ans, int i) {
    if (i == s.length()) {
        System.out.println(ans);
        return;
    }
    subsequences(s, ans + s.charAt(i), i + 1);  // include
    subsequences(s, ans, i + 1);                // exclude
}
```

**Complexity (Time & Space):**
- Time: O(2ⁿ)
- Space: O(n)

### Approach 8: Subsets (Same as subsequences but conceptual)

**Idea:**
- Treat each character as a choice (include/exclude).

**Steps:**
- void subsets(String s, String curr, int i) {
    * if (i == s.length()) {
        * System.out.println(curr);
        * return;
    * }
    * subsets(s, curr + s.charAt(i), i + 1);
    * subsets(s, curr, i + 1);
- }

**Complexity (Time & Space):**
- Time: O(2ⁿ)
- Space: O(n)

### Approach 9: Permutations

**Idea:**
- Fix one character and recursively permute the rest.

**Java Code:**
```java
void permutations(String s, String ans) {
    if (s.length() == 0) {
        System.out.println(ans);
        return;
    }
    for (int i = 0; i < s.length(); i++) {
        char ch = s.charAt(i);
        String ros = s.substring(0, i) + s.substring(i + 1);
        permutations(ros, ans + ch);
    }
}
```

**Complexity (Time & Space):**
- Time: O(n × n!)
- Space: O(n)

### Approach 10: Replace Character

**Idea:**
- Replace all occurrences of a target character.

**Java Code:**
```java
String replaceChar(String s, char oldChar, char newChar, int i) {
    if (i == s.length()) return "";
    char curr = s.charAt(i);
    if (curr == oldChar) curr = newChar;
    return curr + replaceChar(s, oldChar, newChar, i + 1);
}
```

**Complexity (Time & Space):**
- Time: O(n²) (string concatenation)
- Space: O(n)

### Approach 11: Remove Consecutive Duplicates

**Idea:**
- Skip same consecutive characters.

**Java Code:**
```java
String removeDuplicates(String s, int i) {
    if (i == s.length() - 1) return s.charAt(i) + "";
    String next = removeDuplicates(s, i + 1);
    if (s.charAt(i) == s.charAt(i + 1)) return next;
    return s.charAt(i) + next;
}
```

**Complexity (Time & Space):**
- Time: O(n²) (string ops)
- Space: O(n)

### Approach 12: String to Integer (Parse Recursively)

**Idea:**
- Convert each char to number from left to right.

**Java Code:**
```java
int stringToInt(String s, int i) {
    if (i == s.length()) return 0;
    int num = s.charAt(i) - '0';
    return num * (int)Math.pow(10, s.length() - i - 1) + stringToInt(s, i + 1);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 13: Encoding / Decoding (Advanced)

**Idea:**
- Example: “1 → a, 2 → b …” → Decode numeric strings to alphabets.

**Java Code:**
```java
void encode(String s, String ans) {
    if (s.length() == 0) {
        System.out.println(ans);
        return;
    }
    char ch1 = (char) ('a' + (s.charAt(0) - '1'));
    encode(s.substring(1), ans + ch1);
    if (s.length() > 1) {
        int num = Integer.parseInt(s.substring(0, 2));
        if (num <= 26) {
            char ch2 = (char) ('a' + num - 1);
            encode(s.substring(2), ans + ch2);
        }
    }
}
```

**Complexity (Time & Space):**
- Time: O(2ⁿ) worst
- Space: O(n)

---

## 6. Justification / Proof of Optimality

- Recursion simplifies complex string manipulations.
- Reduces looping logic into smaller subproblems.
- Backbone for backtracking & combinatorial string problems.
---

## 7. Variants / Follow-Ups

- String recursion with multiple strings (comparisons)
- Pattern-based recursion (decoding, parentheses matching)
- Using character arrays to optimize performance
---

## 8. Tips & Observations

- Always use base case like i == s.length() or s.length() == 0.
- Be careful with string immutability (avoid excessive concatenation).
- For large inputs, prefer StringBuilder to optimize performance.
- Visualize recursive stack for better debugging.
- Backtracking problems on strings often follow include/exclude pattern.
---

<!-- #endregion -->
