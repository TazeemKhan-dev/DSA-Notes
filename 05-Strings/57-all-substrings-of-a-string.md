<!-- #region 57-All Substrings of a String -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q57: All Substrings of a String</h1>

## 1. Problem Understanding

- Given a string str, print all possible non-empty substrings of it, in order of occurrence.
- A substring is a contiguous sequence of characters within a string.
---

## 2. Constraints

- 0≤∣str∣≤280
- String contains only lowercase English letters (typically).
- The output may have up to
- n×(n+1)/2 substrings → be cautious for large inputs.
---

## 3. Edge Cases

- Empty string ("") → no output.
- Single character ("a") → only "a".
- Repeated characters (e.g. "aaa") → substrings will repeat but are considered valid
---

## 4. Examples

```text
Example 1:
Input:abc
Output:
a  
ab  
abc  
b  
bc  
c
Explanation: All possible non-empty substrings of "abc" printed in order.

Example 2:
Input: abcd
Output:
a  
ab  
abc  
abcd  
b  
bc  
bcd  
c  
cd  
d
```

---

## 5. Approaches

### Approach 1: Brute Force using Nested Loops (Simple & Clear)

**Idea:**
- Generate all substrings using two loops — start index i and end index j.
- For every (i, j) pair, print the substring str.substring(i, j).

**Steps:**
- Loop i from 0 to n-1 (start index).
- For each i, loop j from i+1 to n (end index exclusive).
- Print the substring str.substring(i, j).

**Java Code:**
```java
public static void printSubstrings(String str) {
    int n = str.length();
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j <= n; j++) {
            System.out.println(str.substring(i, j));
        }
    }
}
```

**Complexity (Time & Space):**
- Time: O(n^2) → total substrings = n(n+1)/2
- Space: O(1) → no extra space used

### Approach 2: Using StringBuilder (More Efficient for Printing)

**Idea:**
- Use a StringBuilder to build substrings incrementally, avoiding repeated string creation.

**Steps:**
- Initialize a StringBuilder for each i.
- Append characters one by one to build and print substrings.
- This avoids creating multiple intermediate string objects.

**Java Code:**
```java
public static void printSubstrings(String str) {
    int n = str.length();
    for (int i = 0; i < n; i++) {
        StringBuilder sb = new StringBuilder();
        for (int j = i; j < n; j++) {
            sb.append(str.charAt(j));
            System.out.println(sb.toString());
        }
    }
}
```

**Complexity (Time & Space):**
- Time: O(n^2)  (same as before)
- Space: O(n) for StringBuilder buffer
- Why Better:
- Avoids repeatedly creating new string objects in memory — more efficient for large strings.

### Approach 3: Recursive (Less Common, Educational)

**Idea:**
- Use recursion to print all substrings by reducing the problem — fix a start index and recursively print the rest.

**Steps:**
- Start from index i=0, recursively print substrings starting at i.
- Increment start index and repeat until end.

**Java Code:**
```java
public static void printSubstrings(String str, int start, int end) {
    if (start == str.length()) return;
    if (end == str.length() + 1) {
        printSubstrings(str, start + 1, start + 1);
        return;
    }
    System.out.println(str.substring(start, end));
    printSubstrings(str, start, end + 1);
}
Call:

printSubstrings("abc", 0, 1);
```

**Complexity (Time & Space):**
- Time: O(n^2)
- Space: O(n) recursion stack

---

## 6. Variants / Follow-Ups

- Count Distinct Substrings
  * Use Trie or Suffix Array to count unique substrings efficiently.
- Longest Palindromic Substring
   * Similar substring enumeration, but with palindrome checking (O(n²) or Manacher’s Algorithm O(n)).
- Substring Search / Matching
   * Problems like “Find substring in another string” use similar substring concepts + pattern matching.
- Count Substrings with Given Property
   * Example: count substrings containing equal 0s and 1s, or at most k distinct characters.
- Generate Substrings for Pattern Problems
   * Used in DP for string segmentation or word break problems.
---

## 7. Tips & Observations

- Total non-empty substrings = n(n+1)/2
- Using StringBuilder avoids memory waste
- Substrings differ from subsequences (substrings are contiguous)
- Avoid using recursion for large strings (stack overflow risk)
- For competitive programming, prefer Approach 2
---

<!-- #endregion -->
