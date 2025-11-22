<!-- #region Regex -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: Regex</h1>

## 1. Problem Understanding

- Regex (Regular Expression) is a pattern that describes a set of strings.
- Used for searching, matching, replacing, or splitting strings efficiently.
- Extremely useful in string validation, filtering, and transformation problems.

- **Common Patterns and Usage**
    - Match only letters
      * Expression: [a-zA-Z]
      * Purpose: Matches any single uppercase or lowercase letter.
      * DSA Example: Check if a string contains only letters before applying transformations.
        * String s = "HelloWorld";
        * boolean valid = s.matches("[a-zA-Z]+"); // true
      * Explanation:
        * matches returns true if the entire string consists of letters a-z or A-Z. The + ensures at least one letter.
    - Match only digits
      * Expression: [0-9] or \\d
      * Purpose: Matches any single digit.
      * DSA Example: Count digits in a string or validate numeric input.
        * String s = "123abc";
        * int countDigits = s.replaceAll("\\D", "").length(); // removes non-digits, count = 3
      * Explanation:
        * \\D matches non-digits. replaceAll removes them. length() counts remaining digits.
    - Match word characters
      * Expression: \\w → matches [a-zA-Z0-9_]
      * Purpose: Matches letters, digits, or underscore.
      * DSA Example: Validate variable names (alphanumeric + underscore).
        * String s = "var_1";
        * boolean valid = s.matches("\\w+"); // true
      * Explanation:
        * \\w+ ensures all characters are letters, digits, or underscores.
    - Match non-word characters
      * Expression: \\W → matches anything except [a-zA-Z0-9_]
      * DSA Example: Remove special characters from a string.
        * String s = "hello@world!";
        * String clean = s.replaceAll("\\W", ""); // "helloworld"
      * Explanation:
        * \\W matches special characters. replaceAll removes them, leaving only letters/digits/underscore.
    - Match whitespace
      * Expression: \\s → space, tab, newline
      * DSA Example: Split a sentence into words.
        * String sentence = "We are learning";
        * String[] words = sentence.split("\\s"); // ["We","are","learning"]
      * Explanation:
        * split("\\s") breaks the string wherever whitespace occurs.
    - Match non-whitespace
      * Expression: \\S → any character except whitespace
      * DSA Example: Count all non-space characters in a string.
        * String s = "Hi there";
        * int count = s.replaceAll("\\S", "").length(); // count spaces
      * Explanation:
        * \\S matches non-space characters. replaceAll removes them. Remaining length = spaces count.

- **Quantifiers**
    - " * " → 0 or more occurrences
    - " + "  → 1 or more occurrences
    - ? → 0 or 1 occurrence
    - {n} → exactly n occurrences
    - {n,m} → between n and m occurrences
    - DSA Example: Validate repeated patterns in strings.
      * String s = "aaab";
      * boolean match = s.matches("a{3}b"); // true
    - Explanation:
      * a{3}b matches exactly three 'a's followed by 'b'. Returns true if string matches.

- **Character Classes**
    - [abc] → match a, b, or c
    - [^abc] → match any character except a, b, or c
    - DSA Example: Remove vowels from a string.
      * String s = "leetcode";
      * String noVowels = s.replaceAll("[aeiou]", ""); // "ltcd"
    - Explanation:
      * [aeiou] matches vowels. replaceAll removes them.

- **Anchors**
    - ^ → start of string
    - $ → end of string
    - DSA Example: Check if string starts with a letter.
      * String s = "Hello123";
      * boolean valid = s.matches("^[a-zA-Z].*"); // true
    - Explanation:
      * ^ ensures the first character is a letter, .* matches the rest of the string.

- **Groups & Capturing**
    - (pattern) → captures group
    - DSA Example: Convert snake_case to CamelCase.
      * String s = "hello_world";
      * String camel = s.replaceAll("_(.)", m -> m.group(1).toUpperCase()); // "helloWorld"
    - Explanation:
      * _(.) captures the character after _. group(1).toUpperCase() converts it to uppercase.

- **Negation / Exclusion**
    - [^...] → exclude characters inside brackets
    - DSA Example: Keep only letters from string.
      * String s = "Hello123!";
      * String lettersOnly = s.replaceAll("[^a-zA-Z]", ""); // "Hello"
    - Explanation:
      * [^a-zA-Z] matches anything except letters. replaceAll removes them.

- **Predefined Classes**
    - \\d → digit [0-9]
    - \\D → non-digit [^0-9]
    - \\w → word [a-zA-Z0-9_]
    - \\W → non-word
    - \\s → whitespace
    - \\S → non-whitespace
    - DSA Tip: Combine these for filtering strings efficiently instead of manual iteration.

- **Useful Methods in Java**
    - matches(regex) → check if full string matches pattern
    - replaceAll(regex, replacement) → replace all matches
    - replaceFirst(regex, replacement) → replace first match
    - split(regex) → split string by pattern
    - Pattern.compile(regex) & Matcher → advanced matching with groups
    - Explanation:
    - These methods allow you to search, modify, and split strings in one line instead of loops.

- **DSA Usage Examples**
    - Remove non-alphabetic characters: [a-zA-Z]
      * String s = "a1b2c3!";
      * String letters = s.replaceAll("[^a-zA-Z]", ""); // "abc"
    - Check Pangram / unique letters: [a-z] with bitmask or regex filtering
    - CamelCase conversion: _(.) with replaceAll
    - Validate input strings: ^[a-zA-Z0-9]+$
    - Extract numbers from string: \\d+
    - Explanation:
      * Regex simplifies data cleaning and pattern checks commonly used in string DSA problems.

- **Tips & Observations**
    - Regex is fast for matching/cleaning strings in O(n) time.
    - Use precompiled Pattern for repeated regex to reduce overhead.
    - Remember escaping characters (\ → \\ in Java).
    - For DSA prep, focus on:
    - Cleaning strings [^\p{L}] / [^\w]
    - Splitting strings by whitespace \\s+
    - Group capture for transformations (.)
---

<!-- #endregion -->
