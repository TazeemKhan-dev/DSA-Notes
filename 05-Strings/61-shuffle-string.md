<!-- #region 61-Shuffle String -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q61: Shuffle String</h1>

## 1. Problem Understanding

- Given a string s and an integer array indices of the same length, shuffle the string such that the character at the ith position moves to indices[i] in the shuffled string.
- Print the shuffled string.
---

## 2. Constraints

- 1 <= n <= 500 (length of string)
- indices contains all integers from 0 to n-1 without repetition
---

## 3. Edge Cases

- No shuffling needed (indices = [0,1,2,...]) → string stays same
- Single-character string → no change
- Maximum length string → solution must be O(n)
---

## 4. Examples

```text
Example 1
s = "acciojob"
indices = [4,5,6,7,0,2,1,3]
Output: oojbacci

Example 2
s = "abc"
indices = [0,1,2]
abc
```

---

## 5. Approaches

### Approach 1: Direct Mapping (Extra Array)

**Idea:**
- Create a new char array of the same length as s.
- Place each character at its target index given by indices.

**Steps:**
- Initialize char[] result = new char[n].
- Loop i = 0 to n-1:
  * result[indices[i]] = s.charAt(i)
- Convert result to a string and print

**Java Code:**
```java
static void shuffleString(int[] indices, String s) {
    char[] result = new char[s.length()];
    for (int i = 0; i < s.length(); i++) {
        result[indices[i]] = s.charAt(i);
    }
    System.out.print(new String(result));
}
```

**Complexity (Time & Space):**
- Time: O(n) → single pass through string
- Space: O(n) → extra array for result

### Approach 2: In-Place Swapping (Space Optimized)

**Idea:**
- Swap characters in the original array to their correct positions using indices.
- Avoid extra space for result array.

**Steps:**
- Convert s to char[] sArr.
- Loop i = 0 to n-1:
- While indices[i] != i:
  * Swap sArr[i] with sArr[indices[i]]
  * Swap indices[i] with indices[indices[i]]
- Print sArr

**Java Code:**
```java
static void shuffleStringInPlace(int[] indices, String s) {
    char[] sArr = s.toCharArray();
    for (int i = 0; i < sArr.length; i++) {
        while (indices[i] != i) {
            int target = indices[i];
            // swap characters
            char tempChar = sArr[i];
            sArr[i] = sArr[target];
            sArr[target] = tempChar;
            // swap indices
            int tempIndex = indices[i];
            indices[i] = indices[target];
            indices[target] = tempIndex;
        }
    }
    System.out.print(new String(sArr));
}
```

**Complexity (Time & Space):**
- Time: O(n) → each element visited at most once
- Space: O(1) → in-place, no extra array

---

## 6. Justification / Proof of Optimality

- Approach 1: Simple, intuitive, safe for small n (≤500)
- Approach 2: More space-efficient, optimal for large arrays
---

## 7. Variants / Follow-Ups

- Shuffle only a substring
- Shuffle multiple strings using the same indices
- Handle repeated indices → characters may overwrite each other
---

## 8. Tips & Observations

- Always distinguish pick vs place: this problem is about placing characters at indices, not picking from indices.
- In-place swapping avoids extra memory but modifies input indices.
- For small n, using extra array is simplest and less error-prone.
---

<!-- #endregion -->
