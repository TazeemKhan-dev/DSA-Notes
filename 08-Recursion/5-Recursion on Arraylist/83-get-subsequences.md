<!-- #region 83-Get Subsequences -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q83: Get Subsequences</h1>

## 1. Problem Understanding

- Find all subsequences of a given string.
- A subsequence is formed by deleting some characters without changing the order.
- Return all subsequences in lexicographical order.
- Ignore empty string.
---

## 2. Constraints

- 1 <= s.length <= 100
---

## 3. Edge Cases

- Single character string → only 1 subsequence (itself).
- Repeated characters → subsequences may repeat; use a Set to ensure uniqueness.
---

## 4. Examples

```text
Input: "abc"

Output:

a
ab
abc
ac
b
bc
c


Explanation: All non-empty subsequences in sorted order.
```

---

## 5. Approaches

### Approach 1: Recursion

**Idea:**
- For each character, choose to include or exclude it recursively.
- Base case: empty string → return list containing empty string (later filter it out).
- Combine all subsequences from including and excluding current character.
- Sort the final list lexicographically.

**Steps:**
- If string empty → return list containing "".
- Take first character ch.
- Recurse on rest of string → get list restSubs.
- For each string in restSubs:
  * Add it to result (exclude ch).
  * Add ch + string to result (include ch).
- Remove empty string and sort the list.
- Return the list.

**Java Code:**
```java
static List<String> generateSubsequence(String s) {
    if (s.length() == 0) {
        List<String> base = new ArrayList<>();
        base.add("");
        return base;
    }
    
    char ch = s.charAt(0);
    List<String> restSubs = generateSubsequence(s.substring(1));
    List<String> result = new ArrayList<>();
    
    for (String str : restSubs) {
        result.add(str);           // exclude current character
        result.add(ch + str);      // include current character
    }
    
    result.remove(""); // remove empty string
    Collections.sort(result); // lexicographical order
    return result;
}
generateSubsequence("ab")
           /       \
exclude 'a'           include 'a'
      "b"                   "a" + "b"
     /   \                   /   \
exclude 'b' include 'b'  exclude 'b' include 'b'
  ""       "b"                "a"     "ab"
Result before removing empty: ["", "b", "a", "ab"]

After removing empty and sorting: ["a", "ab", "b"]

another way

public static ArrayList<String> generateSubsequences(String str) {
    ArrayList<String> l = new ArrayList<>();
    helper(str, 0, "", l);
    Collections.sort(l);
    return l;
}

public static void helper(String str, int i, String ans, ArrayList<String> l) {
    if (i >= str.length()) {
        if (ans.length() > 0) {  
            l.add(ans);
        }
        return;
    }
    helper(str, i + 1, ans + str.charAt(i), l);
    helper(str, i + 1, ans, l);
}
```

**Complexity (Time & Space):**
- Time: O(2^n * n) → 2^n subsequences, each of length up to n for sorting
- Space: O(2^n * n) → recursion stack + storing subsequences

### Approach 2: Iterative using Bitmasking

**Idea:**
- A string of length n has 2^n subsequences.
- Each subsequence corresponds to a bitmask of length n:
- 1 → include the character
- 0 → exclude the character
- Loop through all bitmasks from 1 to 2^n - 1 (skip 0 to ignore empty string).
- For each bitmask, build the subsequence by including characters where bit = 1.

**Steps:**
- Let n = s.length().
- Loop mask from 1 to (1 << n) - 1:
- Initialize empty string subseq.
- For each bit i from 0 to n-1:
- If (mask & (1 << i)) != 0 → include s.charAt(i) in subseq.
- Add subseq to list of subsequences.
- Sort the list lexicographically.
- Return the list.

**Java Code:**
```java
static List<String> generateSubsequenceBitmask(String s) {
    int n = s.length();
    List<String> result = new ArrayList<>();
    
    for (int mask = 1; mask < (1 << n); mask++) { // skip 0 to avoid empty string
        StringBuilder subseq = new StringBuilder();
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) != 0) {
                subseq.append(s.charAt(i));
            }
        }
        result.add(subseq.toString());
    }
    
    Collections.sort(result); // lexicographical order
    return result;
}
```

**Complexity (Time & Space):**
- Time: O(2^n * n) → 2^n masks, each can take up to n operations to build string
- Space: O(2^n * n) → storing subsequences

---

## 6. Justification / Proof of Optimality

- Approach 2
  * Correct because it explores all inclusion/exclusion choices for every character.
  * Ensures all subsequences are generated.
  * Lexicographical order achieved by sorting at the end.
- Approach 2
  * Generates all possible subsequences by representing choices using bits.
  * Excludes empty string by starting mask from 1.
  * Sorting ensures lexicographical order.
---

## 7. Variants / Follow-Ups

- Return subsequences as Set to remove duplicates.
- Return only subsequences of length k.
- Generate subsequences iteratively using bitmasking.
---

## 8. Tips & Observations

- Approach 1
  * Maximum number of subsequences for string of length n = 2^n.
  * Use recursion template:
    * Include first character + recurse on rest
    * Exclude first character + recurse on rest
  * Removing empty string ensures only valid subsequences are returned.
  * Sorting can be done using Collections.sort() for lexicographic order.
- Approach 2
  * Useful for iterative solution when recursion depth might be an issue.
  * Works best for n <= 20, since 2^n grows quickly.
  * Can combine with Set to avoid duplicates if the string has repeated characters.
---

<!-- #endregion -->
