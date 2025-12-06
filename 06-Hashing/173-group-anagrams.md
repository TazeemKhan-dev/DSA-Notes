<!-- #region 173-Group Anagrams -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q173: Group Anagrams</h1>

## 1. Problem Statement

- You are given an array of words.
- You must group all anagrams together and print them in a single line.
- Requirements:
  * Words that are anagrams must appear together.
  * Inside each anagram group, words must remain in input order.
  * Groups must appear in lexicographical order of the FIRST WORD that appeared in that group, not in order of anagram key.
  * Output should be a single line.
---

## 2. Problem Understanding

- Two words are anagrams if:
  * They have the same characters
  * In the same frequency
  * Order does not matter
- Example: "cat", "tac", "act" → all anagrams.
- But this problem has non-standard ordering rules:
- ❗ Critical Custom Rule
- You must sort the groups based on:
  * The lexicographical order of the first word that appeared in each group.
- NOT:
  * Lexicographical order of anagram keys
  * Input order
  * Length
  * Alphabetical group name
- Example:
  * eat tea tan ate nat bat
- Groups:
  * eat-group → first word = "eat"
  * tan-group → first word = "tan"
  * bat-group → first word = "bat"
- Sorted based on first words:
  * bat < eat < tan
- Final output:
  * bat eat tea ate tan nat
- This is the rule that causes many solutions to fail.
---

## 3. Constraints

- 1 <= n <= 200
- 0 <= word.length <= 100
- All words are lowercase English letters
---

## 4. Edge Cases

- Single-word groups remain as they are.
- Empty string "" must be grouped with other empty strings.
- Large characters (long strings) still must be sorted.
- Multiple groups may have different sizes.
---

## 5. Examples

```text
Example 1

Input:

5
cat dog tac god act


Groups:

cat-group: cat, tac, act (first word = cat)

dog-group: dog, god (first word = dog)

Lexicographic group order:

cat < dog


Output:

cat tac act dog god

Example 2

Input:

6
eat tea tan ate nat bat


Output:

bat eat tea ate tan nat
```

---

## 6. Approaches

### Approach 1: HashMap + Sorted Key + First-Word Sorting (Correct + Required)

**Idea:**
- Use sorted characters as the anagram key.
- For each anagram group, store the first word encountered that created the group.
- After forming groups, sort them based on this first word (not the key).
- Then print words in group-preserved order.

**Steps:**
- Loop through each word.
- Create key by sorting characters.
- Insert into map:
- key -> list of words
- Also store:
- key -> first word (only once)
- After forming map, create list of keys.
- Sort keys by their first-word value.
- Print words in sorted group order.

**Java Code:**
```java
static String sortString(String str) {
    char ch[] = str.toCharArray();
    Arrays.sort(ch);
    return String.valueOf(ch);
}

static void printAnagramsTogether(String arr[], int n) {

    HashMap<String, List<String>> groups = new HashMap<>();
    HashMap<String, String> firstWord = new HashMap<>();

    for (String word : arr) {
        String key = sortString(word);

        groups.putIfAbsent(key, new ArrayList<>());
        groups.get(key).add(word);

        // Store FIRST appearing word of this group
        firstWord.putIfAbsent(key, word);
    }

    // Sort groups based on FIRST word lexicographically
    List<String> keys = new ArrayList<>(groups.keySet());
    Collections.sort(keys, (a, b) -> firstWord.get(a).compareTo(firstWord.get(b)));

    // Print result
    for (String key : keys) {
        for (String w : groups.get(key)) {
            System.out.print(w + " ");
        }
    }
}
```

**💭 Intuition Behind the Approach:**
- Sorted-string representation uniquely identifies an anagram group.
- A problem-specific rule requires grouping based on first word—not sorted key.
- This solves ALL tricky test cases and behaves exactly how the judge expects.

**Complexity (Time & Space):**
- Time:
  * Sorting each string: O(Log L)
  * Doing this for N strings: O(N * L log L)
  * Sorting groups by first word: O(G log G)
  * (G = number of groups)
  * Overall: O(N * L log L) l
- Space:
  * Map storage: O(N * L)
  * Key strings + first-word map: O(G * L)

---

## 7. Justification / Proof of Optimality

- Sorted-string representation uniquely identifies an anagram group.
- A problem-specific rule requires grouping based on first word—not sorted key.
- This solves ALL tricky test cases and behaves exactly how the judge expects.
---

## 8. Variants / Follow-Ups

- Return list instead of print.
- Group anagrams ignoring case.
- Group anagrams with unicode characters.
- Group anagrams by frequency array instead of sorting (optimization).
---

## 9. Tips & Observations

- Using first-word sorting gives exactly the grouping order demanded by the problem.
- Use putIfAbsent() to cleanly build groups.
- Sorted strings as keys avoid collisions.
---

## 10. Pitfalls

- Sorting groups by sorted key is WRONG (fails test cases).
- Sorting groups by input index is WRONG.
- Sorting words inside the group is WRONG; maintain original input order.
- Not storing first word of the group → cannot sort correctly.
---

<!-- #endregion -->
