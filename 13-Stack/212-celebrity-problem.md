<!-- #region 212-Celebrity Problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q212: Celebrity Problem</h1>

## 1. Problem Statement

- You are given a party of N people.
- A matrix M of size N x N represents who knows whom:
  * M[i][j] = 1 → person i knows person j
  * M[i][i] = 0 always
- A celebrity is a person who:
  * is known by everyone else
  * knows no one
- You must return the index (0-based) of the lowest-numbered celebrity, or -1 if no celebrity exists.
---

## 2. Problem Understanding

- For a person c to be a celebrity:
  * Row c must be all 0 → c knows no one
  * Column c must be all 1 except M[c][c] → everyone knows c
- Brute checking every person works but is inefficient.
- Key idea:
  * If person A knows person B, then A cannot be a celebrity.
---

## 3. Constraints

- 2 <= N <= 300
- M[i][j] ∈ {0,1}
- M[i][i] = 0
---

## 4. Edge Cases

- No celebrity exists
- Multiple possible candidates → return lowest index
- Everyone knows someone
- Only one valid candidate after elimination
---

## 5. Examples

```text
Input:
2
0 1
0 0

Output:
1
```

---

## 6. Approaches

### Approach 1: Brute Force (Check Every Person)

**Idea:**
- Assume each person as celebrity
- Verify row and column conditions

**Steps:**
- For each person i:
  * Check row i → all zeros
  * Check column i → all ones except diagonal
- If both satisfy → return i

**Java Code:**
```java
public int celebrityBrute(int[][] M, int n) {
    for (int i = 0; i < n; i++) {
        boolean celeb = true;

        for (int j = 0; j < n; j++) {
            if (i != j) {
                if (M[i][j] == 1 || M[j][i] == 0) {
                    celeb = false;
                    break;
                }
            }
        }

        if (celeb) return i;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Directly enforce celebrity definition

**Complexity (Time & Space):**
- Time: O(N^2) — checking row + column for each person
- Space: O(1) — no extra space

### Approach 2: Stack Elimination (Better)

**Idea:**
- Push all people into stack
- Compare two people at a time
- Eliminate one who cannot be celebrity

**Steps:**
- Push all indices into stack
- Pop two persons a, b
- If a knows b → a cannot be celebrity
- Else b cannot be celebrity
- Push the remaining candidate
- Verify final candidate

**Java Code:**
```java
public int celebrityStack(int[][] M, int n) {
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) st.push(i);

    while (st.size() > 1) {
        int a = st.pop();
        int b = st.pop();

        if (M[a][b] == 1) st.push(b);
        else st.push(a);
    }

    int cand = st.pop();

    for (int i = 0; i < n; i++) {
        if (i != cand) {
            if (M[cand][i] == 1 || M[i][cand] == 0) return -1;
        }
    }

    return cand;
}
```

**💭 Intuition Behind the Approach:**
- Every comparison removes one non-celebrity
- Only one candidate can survive

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N) — stack

### Approach 3: Two-Pointer Elimination (Optimal)

**Idea:**
- Use two pointers instead of stack
- Same elimination logic with O(1) space

**Steps:**
- Initialize a = 0, b = n - 1
- While a < b:
  * If a knows b → a++
  * Else b--
- Remaining index is candidate
- Verify candidate

**Java Code:**
```java
public int celebrity(int[][] M, int n) {
    int a = 0, b = n - 1;

    while (a < b) {
        if (M[a][b] == 1) a++;
        else b--;
    }

    int cand = a;

    for (int i = 0; i < n; i++) {
        if (i != cand) {
            if (M[cand][i] == 1 || M[i][cand] == 0) return -1;
        }
    }

    return cand;
}
```

**💭 Intuition Behind the Approach:**
- Elimination logic does not require stack
- Constant space version of stack solution

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(1)

---

## 7. Justification / Proof of Optimality

- At most one celebrity can exist
- Pairwise elimination guarantees removal of non-celebrities
- Final verification ensures correctness
---

## 8. Variants / Follow-Ups

- Find all celebrities
- Celebrity in directed graph
- Party with incomplete information
---

## 9. Tips & Observations

- Knowing someone immediately disqualifies a candidate
- Being unknown disqualifies a candidate
- Elimination is more efficient than checking everyone
---

## 10. Pitfalls

- Forgetting final verification
- Assuming survivor is always celebrity
- Checking M[i][i]
- Returning first candidate without validation
---

<!-- #endregion -->
