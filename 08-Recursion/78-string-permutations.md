<!-- #region 78-String Permutations -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q78: String Permutations</h1>

## 1. Problem Understanding

- You are given a string str, and you need to print all possible permutations of its characters.
- Each character must appear exactly once per permutation, and all permutations should be unique and printed in lexicographic order.
---

## 2. Constraints

- 1 ≤ |str| ≤ 5
- Small enough for recursion (O(n!) total permutations).
---

## 3. Edge Cases

- Empty string → no output
- String with duplicates → ensure uniqueness (e.g., "aab")
- Lexicographic order → sort before generating permutations
---

## 4. Examples

```text
Input
abc
Output
abc
acb
bac
bca
cab
cba
```

---

## 5. Approaches

### Approach 1: Simple Recursion (No Duplicates)

**Idea:**
- At each step:
- Pick one character ch
- Recurse on the remaining string ros
- Add ch to the current answer ans

**Steps:**
- Base case: If str is empty → print ans
- For every character ch in str:
- Remove it → get ros
- Recurse with (ros, ans + ch)

**Java Code:**
```java
static void printPermutations(String str, String ans) {
    if (str.isEmpty()) {
        System.out.println(ans);
        return;
    }

    for (int i = 0; i < str.length(); i++) {
        char ch = str.charAt(i);
        String ros = str.substring(0, i) + str.substring(i + 1);
        printPermutations(ros, ans + ch);
    }
}
printPermutations("abc", "")

                 ("abc","")
             /         |          \
          a            b            c
     ("bc","a")   ("ac","b")   ("ab","c")
     /     \        /     \       /     \
  b        c      a       c     a       b
("c","ab")("b","ac")("c","ba")("a","bc")("b","ca")("a","cb")
     ↓         ↓        ↓        ↓        ↓        ↓
   "abc"     "acb"    "bac"    "bca"    "cab"    "cba"
```
### 💭 Intuition Behind Approach 1 — Simple Recursion (No Duplicates)

The permutation problem is fundamentally about **fixing one character at a time** and recursively finding all possible arrangements of the remaining ones.  
At each step, we choose a character `ch` from the string, append it to the current result `ans`, and recurse on the remaining substring (`ros`).  
This creates a branching structure where each branch represents one unique character choice, and by the time the string becomes empty, we’ve built one complete permutation.  
Since we explore all choices at every level, recursion naturally generates all `n!` possible permutations.  
It’s a pure “**choice and explore**” problem, making it a classic example of recursion over decision space.

---

**Complexity (Time & Space):**
- Time: O(n × n!) → Each level makes n choices, total n! permutations
- Space: O(n) → Recursion depth (stack)

### Approach 2: Handle Duplicates (Unique Permutations)

**Idea:**
- If string has duplicate characters (e.g., "aab"), naive recursion prints duplicates.
- To avoid that:
- Sort the string first.
- Use a boolean array or Set at each recursion level to skip duplicate choices.

**Java Code:**
```java
static void printUniquePermutations(String str, String ans) {
    if (str.isEmpty()) {
        System.out.println(ans);
        return;
    }

    HashSet<Character> used = new HashSet<>();

    for (int i = 0; i < str.length(); i++) {
        char ch = str.charAt(i);
        if (used.contains(ch)) continue; // skip duplicate char at this level
        used.add(ch);

        String ros = str.substring(0, i) + str.substring(i + 1);
        printUniquePermutations(ros, ans + ch);
    }
}
                ("aab","")
             /             \
         a("ab","a")       b("aa","b")
        /     \              |
    a("b","aa") b("a","ab")  a("a","ba")
       ↓           ↓            ↓
     "aab"        "aba"        "baa"
```

### 💭 Intuition Behind Approach 2 — Handle Duplicates (Unique Permutations)

When a string contains duplicate characters (like `"aab"`), naive recursion generates duplicate permutations because identical choices lead to the same results.  
To prevent this, we use a **Set** (or boolean array) at each recursion level to **remember which characters have already been chosen** in that position.  
Before recursing, we check if the character was already used — if yes, we skip it.  
This pruning ensures that even though recursion still explores all branches, it never revisits identical states.  
Sorting the string before recursion further guarantees **lexicographic order**, so results appear in sorted sequence.  
Together, sorting + the Set transform the brute-force recursive search into a **systematic, duplicate-free generation** of all unique permutations.


**Complexity (Time & Space):**
- Time: O(n × n!) worst case
- Space: O(n) recursion depth + O(n) set storage
- But avoids repeated permutations → more efficient on duplicate inputs

---

## 6. Justification / Proof of Optimality

- Approach 1
  * This approach explores all possible character arrangements recursively.
  * Every level "fixes" one character while permuting the rest.
  * It’s a direct application of recursion for branching problems (each branch = choice of one character).
- Approach 2
  * Sorting ensures lexicographic order.
  * Using a Set ensures no duplicate character is chosen in the same recursion level.
  * Hence, all generated permutations are unique and ordered.
---

## 7. Variants / Follow-Ups

- Return instead of Print
  * Return List<String> instead of printing directly.
- Count Total Permutations
  * Instead of printing, return count → n! / (freq of each duplicate)!
- Lexicographically Next Permutation
  * Given a string, find its next permutation in sorted order.
- Kth Permutation Sequence
  * Find the k-th permutation directly (e.g., LeetCode #60).
- Permutations of Array / List
  * Apply same recursion logic on integer arrays.
- Backtracking (In-place Swap) Version
  * Instead of building substrings, swap characters in an array and backtrack.
---

## 8. Tips & Observations

- Recursive permutation generation is a classic backtracking pattern.
- Base case is always when all choices are made (empty string or full selection).
- Lexicographic order needs sorted input.
- Always visualize recursion as a tree of choices (branch = one pick).
- For large n, recursion becomes infeasible (factorial explosion).
---

<!-- #endregion -->
