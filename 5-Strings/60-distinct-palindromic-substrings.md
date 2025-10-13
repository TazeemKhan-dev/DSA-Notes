<!-- #region 60-Distinct Palindromic Substrings -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q60: Distinct Palindromic Substrings</h1>

## 1. Problem Understanding

- Given a string s, find all substrings that are palindromic.
- Print these substrings in lexicographical (alphabetical) order.
- Palindrome: A string that reads the same forward and backward.
- Substring: A contiguous sequence of characters within a string.
---

## 2. Constraints

- 1≤∣s∣≤1000
- String contains only English letters (a-z, A-Z).
---

## 3. Edge Cases

- Single-character strings → the single character is a palindrome.
- All characters identical → every substring is a palindrome.
- No repeated palindromes should be printed more than once.
---

## 4. Examples

```text
Example 1
Input:  abc
Output:
a
b
c

Example 2
Input:  abccbc
Output:
a
b
bccb
c
cbc
cc
```

---

## 5. Approaches

### Approach 1: Brute Force (Generate All Substrings)

**Idea:**
- Generate all possible substrings of s.
- Check each substring for palindrome property.
- Use a Set to store unique palindromic substrings.
- Sort the set lexicographically.

**Steps:**
- Initialize a Set<String> for unique palindromes.
- For all pairs (i, j) where 0 ≤ i ≤ j < n:
  * Extract substring s[i...j].
  * Check if it is a palindrome.
  * If yes, add to the set.
- Convert the set to a list and sort lexicographically.
- Print each substring.

**Java Code:**
```java
public static void solution(String s) {
    Set<String> palSet = new HashSet<>();

    int n = s.length();
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            String sub = s.substring(i, j + 1);
            if (isPalindrome(sub)) {
                palSet.add(sub);
            }
        }
    }

    List<String> palList = new ArrayList<>(palSet);
    Collections.sort(palList);
    for (String p : palList) {
        System.out.println(p);
    }
}

private static boolean isPalindrome(String str) {
    int l = 0, r = str.length() - 1;
    while (l < r) {
        if (str.charAt(l) != str.charAt(r)) return false;
        l++; r--;
    }
    return true;
}
```

**Complexity (Time & Space):**
- Time: O(n³) → O(n²) substrings × O(n) palindrome check
- Space: O(k) → number of distinct palindromes

### Approach 2: Expand Around Centers

**Idea:**
- Each palindrome is symmetric around a center.
- A palindrome can have one center (odd length) or two centers (even length).
- Expand around every possible center and collect palindromes.

**Steps:**
- Initialize a Set<String> for unique palindromes.
- For each index i in the string:
  * Expand around center i (odd-length palindrome)
  * Expand around center (i, i+1) (even-length palindrome)
  * Add each found substring to the set
- Sort and print the set lexicographically.

**Java Code:**
```java
public static void solution(String s) {
    Set<String> palSet = new HashSet<>();
    int n = s.length();

    for (int i = 0; i < n; i++) {
        expand(s, i, i, palSet);     // odd length
        expand(s, i, i + 1, palSet); // even length
    }

    List<String> palList = new ArrayList<>(palSet);
    Collections.sort(palList);
    for (String p : palList) {
        System.out.println(p);
    }
}

private static void expand(String s, int left, int right, Set<String> palSet) {
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        palSet.add(s.substring(left, right + 1));
        left--; right++;
    }
}
```

**Complexity (Time & Space):**
- Time: O(n²) → expanding from each center takes O(n) in total
- Space: O(k) → distinct palindromic substrings

### Approach 3: Dynamic Programming (DP Table)

**Idea:**
- Use a DP table dp[i][j] to store whether substring s[i..j] is a palindrome.
- Build table for increasing lengths.
- Store palindromic substrings in a set.

**Steps:**
- Initialize boolean dp[n][n]
- For all i: dp[i][i] = true → single letters
- For all pairs (i, j) in increasing length:
  * dp[i][j] = true if s[i] == s[j] and (j-i < 2 || dp[i+1][j-1])
- Add s[i..j] to set if dp[i][j] == true
- Sort and print

**Java Code:**
```java
public static void solution(String s) {
    int n = s.length();
    boolean[][] dp = new boolean[n][n];
    Set<String> palSet = new HashSet<>();

    for (int len = 1; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s.charAt(i) == s.charAt(j) && (len <= 2 || dp[i+1][j-1])) {
                dp[i][j] = true;
                palSet.add(s.substring(i, j + 1));
            }
        }
    }

    List<String> palList = new ArrayList<>(palSet);
    Collections.sort(palList);
    for (String p : palList) {
        System.out.println(p);
    }
}
```

**Complexity (Time & Space):**
- Time: O(n²)
- Space: O(n² + k) → DP table + set of palindromes

---

## 6. Justification / Proof of Optimality

- Brute force: simple but inefficient (O(n³))
- Expand around center: optimal O(n²) solution, intuitive
- DP: also O(n²), useful if you need extra information about palindrome positions
---

## 7. Variants / Follow-Ups

- Count number of distinct palindromes instead of printing
- Find the longest palindromic substring
- Modify to return palindromes of length ≥ k
- Lexicographically smallest/largest palindrome
---

## 8. Tips & Observations

- Use a Set to remove duplicates automatically.
- Expanding around centers is often simpler than DP for substring printing.
- Sorting lexicographically can be done via Collections.sort() in Java.
- For large strings, avoid brute force (O(n³)) due to time constraints.
---

<!-- #endregion -->
