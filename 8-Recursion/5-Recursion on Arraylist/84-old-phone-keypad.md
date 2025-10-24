<!-- #region 84-Old Phone Keypad -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q84: Old Phone Keypad</h1>

## 1. Problem Understanding

- Given a sequence of key presses on an old phone keypad, generate all possible letter combinations.
- Keys map to multiple letters:
  * 1 -> ABC
  * 2 -> DEF
  * 3 -> GHI
  * 4 -> JKL
  * 5 -> MNO
  * 6 -> PQRS
  * 7 -> TU
  * 8 -> VWX
  * 9 -> YZ
  * 0, *, # -> ignored
- Output must be lexicographically sorted.
- The order of key presses must be maintained.
---

## 2. Constraints

- 1 ≤ n ≤ 10 → number of key presses
- 1 ≤ key[i] ≤ 9 → keys pressed
- Output must contain uppercase letters only
---

## 3. Edge Cases

- Single key press → just return letters of that key
- Repeated key → allow repetition in combinations
- Keys 0, *, # → ignored (no letters assigned)
---

## 4. Examples

```text
Input:

2
2 5


Output:

DM DN DO EM EN EO FM FN FO
```

---

## 5. Approaches

### Approach 1: Recursion (Backtracking)

**Idea:**
- For each key, iterate over its letters and recursively generate combinations for the remaining keys.

**Steps:**
- Map each key to its corresponding letters in an array.
- Start recursion with index = 0 and empty string ans.
- For each character corresponding to keys[index]:
  * Append to ans
  * Recurse for index + 1
- Base case: if index == n → add ans to result list

**Java Code:**
```java
public static ArrayList<String> keypadCombinations(int[] keys) {
    String[] map = {"", "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TU", "VWX", "YZ"};
    ArrayList<String> res = new ArrayList<>();
    helper(keys, 0, "", map, res);
    Collections.sort(res);
    return res;
}

static void helper(int[] keys, int index, String ans, String[] map, ArrayList<String> res) {
    if (index == keys.length) {
        res.add(ans);
        return;
    }
    String letters = map[keys[index]];
    for (char ch : letters.toCharArray()) {
        helper(keys, index + 1, ans + ch, map, res);
    }
}
Recursion Tree for keys = [2,5] (2 -> DEF, 5 -> MNO):

("",0)
   /   |   \
"D",1   "E",1   "F",1
 /|\     /|\     /|\
"M N O" "M N O" "M N O"


Produces: DM, DN, DO, EM, EN, EO, FM, FN, FO
```

**Complexity (Time & Space):**
- Time: O(letters_per_key ^ n) → For worst-case key 6 → 4 letters, max n = 10 → O(4^10)
- Space: O(n) recursion depth + O(total combinations) storage

---

## 6. Justification / Proof of Optimality

- Generates all possible combinations by recursion maintaining order of key presses.
- Sorted after generation to ensure lexicographical order.
---

## 7. Variants / Follow-Ups

- Can print directly instead of storing if memory is tight.
- Can adapt to lowercase letters easily by changing mapping.
---

## 8. Tips & Observations

- Each key press branches into multiple letters → classic recursion tree problem
- Lexicographic order is ensured by sorting at the end or by storing letters in order in mapping
- Works best for small number of key presses; for large n, iterative combinatorial generation may be
---

<!-- #endregion -->
