<!-- #region 64-Ptice -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q64: Ptice</h1>

## 1. Problem Understanding

- You are given the correct answers to an exam consisting of n questions (each being A, B, or C).
- Three people — Adrian, Bruno, and Goran — guess answers according to their fixed repeating patterns:
  * Adrian: A, B, C, A, B, C, ...
  * Bruno: B, A, B, C, B, A, B, C, ...
  * Goran: C, C, A, A, B, B, C, C, A, A, B, B, ...
- You must find:
  * Who got the maximum number of correct answers.
  * Print the highest score.
  * Then, print the names of all who achieved that score (in alphabetical order).
---

## 2. Constraints

- 1 ≤ n ≤ 100
- Each character in input string ∈ {A, B, C}
---

## 3. Edge Cases

- All get the same number of correct answers.
- Only one person gets the highest score.
- n smaller than length of any pattern (needs modular repetition).
- All answers are same (e.g. "AAAAAA").
---

## 4. Examples

```text
Example 1
Input:
5
BAACC
Output:
3
Bruno
Explanation:
Bruno’s sequence best matches the correct answers — 3 matches.

Example 2
Input:
9
AAAABBBBB
Output:
4
Adrian
Bruno
Goran
Explanation:
All three get 4 correct answers.
```

---

## 5. Approaches

### Approach 1: Brute Force Simulation

**Idea:**
- Generate the repeating pattern for each person up to length n.
- Compare each answer in s to the corresponding answer in each pattern.
- Count matches for each person.
- Find the maximum score and print all who achieved it.

**Steps:**
- Read n and the string answers.
- Define patterns:
  * String adrian = "ABC";
  * String bruno = "BABC";
  * String goran = "CCAABB";
- For each i in 0 to n-1:
  * Compare answers[i] with pattern[i % pattern.length()].
  * Increment count for matching ones.
- Compute maxScore = max(countA, countB, countG).
- Print maxScore and names (alphabetically) of all with that score.

**Java Code:**
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String answers = sc.next();
        
        String adrian = "ABC";
        String bruno = "BABC";
        String goran = "CCAABB";
        
        int a = 0, b = 0, g = 0;
        
        for (int i = 0; i < n; i++) {
            char ans = answers.charAt(i);
            if (ans == adrian.charAt(i % adrian.length())) a++;
            if (ans == bruno.charAt(i % bruno.length())) b++;
            if (ans == goran.charAt(i % goran.length())) g++;
        }
        
        int max = Math.max(a, Math.max(b, g));
        System.out.println(max);
        
        if (a == max) System.out.println("Adrian");
        if (b == max) System.out.println("Bruno");
        if (g == max) System.out.println("Goran");
    }
}
```

**Complexity (Time & Space):**
- Time Complexity: O(n) — single pass comparison for each of the three sequences.
- Space Complexity: O(1) — only uses counters.

---

## 6. Justification / Proof of Optimality

- This approach is optimal since it directly compares each answer with the pre-determined pattern in O(n).
- No extra data structures are required, and repeating patterns are handled neatly via modulo indexing.
---

## 7. Variants / Follow-Ups

- Similar problems can involve:
  * Customizable patterns.
  * Weighted answers (e.g., +2 for correct, -1 for wrong).
  * More players with unique repeating patterns.
---

## 8. Tips & Observations

- Use modulo to repeat patterns efficiently instead of generating full-length strings.
- Sorting names alphabetically can be skipped here since the order “Adrian, Bruno, Goran” is already lexicographical.
- A good problem to practice pattern repetition and string indexing with modulo logic.
---

<!-- #endregion -->
