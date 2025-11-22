<!-- #region 59-Camel Case Word Separator -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q59: Camel Case Word Separator</h1>

## 1. Problem Understanding

- You are given a string S written in Camel Case format (e.g., IAmAJavaProgrammer).
- Each new word starts with an uppercase letter.
- You must separate and print each word on a new line.
- The goal is to enhance readability by splitting the Camel Case string into individual words.
---

## 2. Constraints

- 1≤∣S∣≤10^4
- S contains only English letters (a-z, A-Z).
---

## 3. Edge Cases

- String has only one word → "Java" → Output: Java
- String has all uppercase letters → "ABC" → Output:
- A
- B
- C
- String starts with lowercase (non-standard CamelCase) → handle gracefully.
---

## 4. Examples

```text
Example 1

Input:  IAmAJavaProgrammer
Output:
I
Am
A
Java
Programmer
```

---

## 5. Approaches

### Approach 1: Index-Based Substring (Mathematical Traversal)

**Idea:**
- Use two pointers (start, i) to mark word boundaries.
- Every time you encounter an uppercase character, treat it as the beginning of a new word.
- Use substring extraction to print words.

**Steps:**
- Initialize start = 0
- Traverse string from index 1 to n-1
- If str.charAt(i) is uppercase:
  * Print substring from start to i
  * Update start = i
- After loop, print substring from start to end

**Java Code:**
```java
public static void solution(String str) {
    int n = str.length();
    int start = 0;

    for (int i = 1; i < n; i++) {
        if (Character.isUpperCase(str.charAt(i))) {
            System.out.println(str.substring(start, i));
            start = i;
        }
    }
    System.out.println(str.substring(start));
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(1)

### Approach 2: Manual Traversal (Using StringBuilder)

**Idea:**
- Build words character by character.
- When an uppercase letter is found (and a word already exists), print the accumulated word.
- Continue building until the next uppercase boundary.

**Steps:**
- Initialize an empty StringBuilder
- For each character:
  * If uppercase and builder not empty → print word and reset builder
  * Append current character
- Print the final word after loop

**Java Code:**
```java
public static void solution(String str) {
    StringBuilder word = new StringBuilder();

    for (int i = 0; i < str.length(); i++) {
        char ch = str.charAt(i);
        if (Character.isUpperCase(ch) && word.length() > 0) {
            System.out.println(word.toString());
            word.setLength(0);
        }
        word.append(ch);
    }

    if (word.length() > 0) {
        System.out.println(word.toString());
    }
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(1)

### Approach 3: Regex Split (String Utility)

**Idea:**
- Use a regex pattern to split before every uppercase letter.
- Pattern (?=[A-Z]) means “split before uppercase characters.”

**Steps:**
- Use split("(?=[A-Z])") to divide the string.
- Print each resulting substring.

**Java Code:**
```java
public static void solution(String str) {
    String[] words = str.split("(?=[A-Z])");
    for (String w : words) {
        System.out.println(w);
    }
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(k) (where k = number of words)

---

## 6. Justification / Proof of Optimality

- The index-based approach is the most memory-efficient and logical (O(1) space).
- The StringBuilder approach offers clarity and flexibility for modifications.
- The regex approach is concise but involves slightly higher runtime overhead due to regex processing.
---

## 7. Variants / Follow-Ups

- Convert CamelCase → snake_case (IAmAJavaProgrammer → i_am_a_java_programmer)
- Count the number of words instead of printing them
- Handle PascalCase or mixedCase strings
---

## 8. Tips & Observations

- Character.isUpperCase(ch) helps detect word starts safely.
- Avoid concatenating strings directly inside loops — use StringBuilder or substring slicing.
- Regex (?=[A-Z]) is a powerful pattern to handle CamelCase splits elegantly.
- The index-based traversal is the most intuitive and optimal for large inputs.
---

<!-- #endregion -->
