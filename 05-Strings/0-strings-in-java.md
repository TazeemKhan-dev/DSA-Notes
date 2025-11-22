<!-- #region Strings in Java -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: Strings in Java</h1>

## 1. Problem Understanding

- In Java, String is a class, not a primitive type.
- Strings are immutable — once created, their content cannot change.
- Any modification (like concatenation) creates a new String object.
- String literals are stored in the String Pool for memory efficiency.
  * Example:
    * String s1 = "hello";
    * String s2 = s1;
    * s1 = s1 + " world"; // creates new String

- **String Creation**
    - String s1 = "hello"; → stored in String Pool.
    - String s2 = new String("hello"); → stored in Heap memory.
    - Prefer literals for DSA — they are faster and reused automatically.

- **Key Properties**
    - Strings are immutable.
    - String literals are reused from the String Pool.
    - Implement Comparable for lexicographic comparison.
    - 0-indexed like arrays.
    - Supports index-based operations via charAt().

- **Common and Useful String Methods**
    - Length and Character Access
      * length() → returns number of characters.
      * charAt(i) → returns character at index i.
    - Substring and Searching
      * substring(i, j) → substring from index i to j-1.
      * indexOf(ch) → first occurrence of a character.
      * lastIndexOf(ch) → last occurrence of a character.
      * contains(str) → checks if a substring exists.
    - Comparison
      * equals(str) → compares values.
      * equalsIgnoreCase(str) → ignores case.
      * compareTo(str) → lexicographically compares (returns +, -, or 0).
    - Prefix and Suffix
      * startsWith(prefix) → checks prefix.
      * endsWith(suffix) → checks suffix.
    - Case and Spaces
      * toLowerCase() / toUpperCase() → converts case.
      * trim() → removes leading/trailing spaces.
    - Replacement and Split
      * replace(old, new) → replaces characters or substrings.
      * split(delimiter) → splits into array based on delimiter.
    - Conversion
      * toCharArray() → converts to char array.
      * String.valueOf(data) → converts other data types to string.

- **StringBuilder (Mutable Alternative)**
    - Strings are immutable; concatenation repeatedly is costly (O(n²)).
    - Use StringBuilder for modification-heavy tasks (like reversing or appending).
    - Example:
      * StringBuilder sb = new StringBuilder("abc");
      * sb.append("xyz");     // "abcxyz"
      * sb.insert(3, "123");  // "abc123xyz"
      * sb.delete(2, 4);      // deletes characters 2–3
      * sb.reverse();         // reverses string
      * String result = sb.toString();
    - StringBuilder is faster but not thread-safe.
    - StringBuffer is thread-safe (rarely needed in DSA).

- **DSA Operations with Strings**
    - Basic String Operations
      * Get Character at Index:
         * s.charAt(i)
      * Get Length of String:
        * s.length()
      * Check Equality:
        * Case-sensitive → s.equals(t)
        * Case-insensitive → s.equalsIgnoreCase(t)
      * Find Substring:
        * s.contains(sub) or s.indexOf(sub) → returns -1 if not found.
      * Get Substring:
        * s.substring(start, end) → from start (inclusive) to end (exclusive).
      * Replace Characters or Substrings:
        * Replace single character → s.replace('a', 'b')
        * Replace using regex → s.replaceAll("regex", "replacement")
      * Split String by Delimiter:
        * s.split(" ") or s.split(",")
      * Trim Whitespace:
         * s.trim()
      * Convert Between String and Integer:
         * String → Integer → Integer.parseInt(str)
         * Integer → String → String.valueOf(num)
    - Common DSA Operations with Strings
      * Reverse a String:
        * new StringBuilder(s).reverse().toString();
      * Check Palindrome:
        * Compare s with its reversed version.
      * Count Character Frequency:
        * Use an int[26] array for lowercase letters.
      * Convert Char to Int:
        * ch - '0'
      * Convert Int to Char:
        * (char)(num + '0')
      * Convert String to Char Array:
        * s.toCharArray()
      * Join Array of Strings:
        * String.join("", array)
      * Sort Characters in String:
        * char[] c = s.toCharArray();
        * Arrays.sort(c);
        * String sorted = new String(c);
      * Compare Substrings:
        * s.substring(i, j).equals(t.substring(x, y))
    - Character Case Operations
      * Check uppercase → Character.isUpperCase(ch)
      * Check lowercase → Character.isLowerCase(ch)
      * Convert to upper → Character.toUpperCase(ch)
      * Convert to lower → Character.toLowerCase(ch)
      * String → uppercase → str.toUpperCase()
      * String → lowercase → str.toLowerCase()

- **String vs char[] (Quick Comparison)**
    - String → immutable, more built-in methods, easy to use.
    - char[] → mutable, good for in-place edits like reversals.
    - Use char[] in problems requiring frequent modifications.

- **Time Complexity Notes**
    - charAt() → O(1)
    - equals() → O(n)
    - Concatenation (+) → O(n)
    - Append in StringBuilder → O(1) amortized
    - substring() → O(n)

- **Key Takeaways**
    - Use StringBuilder for concatenation-heavy logic.
    - Use equals() instead of == for value comparison.
    - Remember Strings are immutable.
    - Master substring(), charAt(), and toCharArray().
    - Understand compareTo() for lexicographic sorting and comparison.
---

<!-- #endregion -->
