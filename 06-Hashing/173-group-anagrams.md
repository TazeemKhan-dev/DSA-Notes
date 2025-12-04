<!-- #region 173-Group Anagrams -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q173: Group Anagrams</h1>

## 1. Problem Statement

- You are given an array of words.
- You must group all anagrams together and print them in the order groups appear lexicographically, but within each group, the words must appear in the order they appeared in the input.
- Anagrams → words that contain the same characters with the same frequencies (e.g., “cat”, “tac”, “act”).
- Example:
  * Input: cat dog tac god act
  * Output: cat tac act dog god
---

## 2. Problem Understanding

- You must:
  * Group words that are anagrams.
  * Print groups sorted lexicographically by their key (sorted characters).
  * Within a group, preserve original input order.
  * Print all words in a single line.
- This is not a normal "group anagrams" problem.
- There are two ordering constraints:
  * Group order: Based on lexicographical order of sorted key ("act" comes before "dgo").
  * Word order inside group: Must follow original input order.
---

## 3. Constraints

- 1 ≤ N ≤ 200
- 0 ≤ len(word) ≤ 100
- All words lowercase English letters.
---

## 4. Edge Cases

- Single-word groups should appear alone.
- Words with different lengths cannot be anagrams.
- Empty string "" should group with other empty strings.
- Large words → sorting cost acceptable due to low constraints.
---

## 5. Examples

```text
Input
eat tea tan ate nat bat
Output
bat eat tea ate tan nat

Reason:
Sorted keys:

bat → "abt"

eat/tea/ate → "aet"

tan/nat → "ant"

Order: "abt" < "aet" < "ant"
```

---

## 6. Approaches

### Approach 1: HashMap (Key = Sorted String) + Preserve Order (Optimal)

**Idea:**
- Convert each word to a sorted-character key.
- Use a LinkedHashMap<String, List<String>> to store groups in insertion order.
- But groups must be output in lexicographical order of the key, NOT insertion order.
- → So we must store groups first, then sort the keys.

**Steps:**
- For each word:
  * Compute key = sorted(word).
  * Put in map (key → list of words).
- Extract all keys.
- Sort keys lexicographically.
- Print words group by group, preserving original insertion order inside each group.

**Java Code:**
```java
public void groupAnagrams(String[] arr) {
    // Map: sortedKey -> list of original words (in input order)
    HashMap<String, List<String>> map = new HashMap<>();

    for (String word : arr) {
        char[] ch = word.toCharArray();
        Arrays.sort(ch);
        String key = new String(ch);

        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(word);
    }

    // Sort keys lexicographically
    List<String> keys = new ArrayList<>(map.keySet());
    Collections.sort(keys);

    // Print result
    StringBuilder sb = new StringBuilder();
    for (String key : keys) {
        for (String w : map.get(key)) {
            sb.append(w).append(" ");
        }
    }

    System.out.print(sb.toString().trim());
}
```

**💭 Intuition Behind the Approach:**
- Anagrams always share the same sorted string representation, so grouping by sorted key ensures correctness.
- Sorting keys ensures lexicographical group ordering.
- Preserving insertion order inside the lists ensures the group outputs match input order.

**Complexity (Time & Space):**
- Time:
  * Sorting each word: O(L log L)
  * For N words: O(N * L log L)
  * Sorting keys: negligible (~200)
  * Overall: O(N * L log L)
- Space:
  * Map of all words: O(N * L)

### Approach 2: Character Frequency Array Key (Faster but Wordy)

**Idea:**
- Instead of sorting each word, you create a 26-length frequency signature.
- Example:
- "eat" → [1,0,0,0,...,1,...,1]
- But lexicographical ordering using freq arrays becomes messy, so sorting keys requires converting them to string anyway.

**💭 Intuition Behind the Approach:**
- Frequency arrays uniquely identify anagrams, but not worth extra complexity for ordering.

**Complexity (Time & Space):**
- Time: O(N * L)
- Space: O(N * 26)

---

## 7. Justification / Proof of Optimality

- Sorted-string keys are simpler, easy to lexicographically order, and guarantee correctness.
- Given N ≤ 200, sorting cost is negligible.
---

## 8. Variants / Follow-Ups

- Return list of lists instead of printing.
- Group anagrams ignoring case.
- Group anagrams of sentences (multi-word).
- Group by character frequency without sorting.
---

## 9. Tips & Observations

- Sorted keys give natural lexicographical grouping.
- Always use StringBuilder for printing large outputs.
- HashMap + sorting keys is standard approach for ordering groups.
---

## 10. Pitfalls

- Using LinkedHashMap alone will NOT satisfy lexicographical group order.
- Forgetting to trim the final space.
- Sorting words themselves instead of sorting keys → breaks input-order rule.
---

<!-- #endregion -->
