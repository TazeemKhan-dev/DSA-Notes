<!-- #region 66-Keyboard Row Words -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q66: Keyboard Row Words</h1>

## 1. Problem Understanding

- You are given an array of lowercase strings.
- You must print all words that can be typed using only one row of the QWERTY keyboard.
- Keyboard layout:
  * Row 1: qwertyuiop
  * Row 2: asdfghjkl
  * Row 3: zxcvbnm
- A word qualifies if all its characters belong to only one of the above rows.
---

## 2. Constraints

- 1 ≤ n ≤ 1000
- 1 ≤ |s| ≤ 100
- Each word contains only lowercase English letters.
---

## 3. Edge Cases

- Word with characters from multiple rows → ❌
- Single-character word (always valid ✅).
- Duplicate words → print as they appear.
- No valid words → print nothing.
---

## 4. Examples

```text
Example 1
Input:
2
glad
monkey
Output:
glad
Explanation:
glad uses only row 2 characters (asdfghjkl).

Example 2
Input:
3
horse
peter
jass
Output:
peter
jass
Explanation:
Both peter and jass can be typed using characters from a single keyboard row.
```

---

## 5. Approaches

### Approach 1: Set Comparison

**Idea:**
- Represent each keyboard row as a set of characters.
- For each word:
  * Convert it into a set of its characters.
  * If that set is a subset of any keyboard row set, print the word.

**Steps:**
- Read integer n.
- Initialize:
  * Set<Character> row1 = Set.of('q','w','e','r','t','y','u','i','o','p');
  * Set<Character> row2 = Set.of('a','s','d','f','g','h','j','k','l');
  * Set<Character> row3 = Set.of('z','x','c','v','b','n','m');
- For each word:
  * Create a character set from it.
  * Check if the word’s characters are all in row1 or row2 or row3.
- Print valid words.

**Java Code:**
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        sc.nextLine(); // consume newline

        Set<Character> row1 = Set.of('q','w','e','r','t','y','u','i','o','p');
        Set<Character> row2 = Set.of('a','s','d','f','g','h','j','k','l');
        Set<Character> row3 = Set.of('z','x','c','v','b','n','m');

        for (int i = 0; i < n; i++) {
            String word = sc.nextLine().toLowerCase();
            Set<Character> chars = new HashSet<>();
            for (char c : word.toCharArray()) chars.add(c);

            if (row1.containsAll(chars) || row2.containsAll(chars) || row3.containsAll(chars)) {
                System.out.println(word);
            }
        }
    }
}
```

**Complexity (Time & Space):**
- Time Complexity: O(n * |s|)
- → Each word’s characters are checked once.
- Space Complexity: O(1)
- → Only small fixed-size sets are used.

---

## 6. Justification / Proof of Optimality

- Using sets makes subset checking (containsAll) efficient and concise.
- This avoids manually checking each character row by row.
---

## 7. Variants / Follow-Ups

- Handle uppercase letters (just convert input to lowercase).
- Find which row number each valid word belongs to.
- Modify for custom keyboard layouts (input-driven).
---

## 8. Tips & Observations

- Using Set.containsAll() simplifies row validation greatly.
- Converting all words to lowercase avoids case-mismatch issues.
- Could also be done using string membership checks with allMatch() in Java streams.
---

<!-- #endregion -->
