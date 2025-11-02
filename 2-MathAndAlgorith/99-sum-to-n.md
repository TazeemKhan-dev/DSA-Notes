<!-- #region 99-Sum to N -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q99: Sum to N</h1>

## 1. Problem Understanding

- You need to find how many combinations of distinct digits (1–9) of size k have a sum equal to n.
- Each combination must use distinct numbers, and the order doesn’t matter (i.e., {1,2,4} and {2,1,4} are the same).
---

## 2. Constraints

- 1 <= k <= 9
- 1 <= n <= 45
- Digits available = {1, 2, 3, 4, 5, 6, 7, 8, 9}
---

## 3. Edge Cases

- n < smallest possible sum → return 0
- n > largest possible sum → return 0
- If no valid combinations exist, return 0
---

## 4. Examples

```text
Input:
9 3
Output:
3
Valid combinations:
{1, 2, 6}, {1, 3, 5}, {2, 3, 4}
```

---

## 5. Approaches

### Approach 1: Recursive Backtracking

**Idea:**
- Use recursion to explore all combinations of numbers from 1–9.
- At each step, choose a number, reduce n by that number, and decrease k by 1.
- Stop when n == 0 and k == 0 (a valid combination found).

**Steps:**
- Define a helper function countCombinations(start, n, k)
  * start: current number to consider (ensures distinct + ascending order)
- If n == 0 and k == 0 → found a valid combination → return 1
- If n < 0 or k == 0 → invalid path → return 0
- Loop i from start to 9
  * Include i → call recursively with n - i, k - 1, i + 1
- Sum all valid recursive counts

**Java Code:**
```java
public class Main {
    public static int sumOfN(int n, int k) {
        return helper(1, n, k);
    }

    private static int helper(int start, int n, int k) {
        // Base case: found valid combination
        if (n == 0 && k == 0) return 1;

        // Base case: invalid state
        if (n < 0 || k == 0) return 0;

        int count = 0;
        for (int i = start; i <= 9; i++) {
            count += helper(i + 1, n - i, k - 1);
        }
        return count;
    }

    public static void main(String[] args) {
        java.util.Scanner sc = new java.util.Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        System.out.println(sumOfN(n, k));
    }
}

🌳 Recursion Tree (Example: n = 7, k = 3)
helper(1,7,3)
 ├─ i=1 → helper(2,6,2)
 │     ├─ i=2 → helper(3,4,1)
 │     │     ├─ i=3 → helper(4,1,0) ❌
 │     │     ├─ i=4 → helper(5,0,0) ✅ (found 1)
 │     │     └─ i>4 → exceeds 9 ❌
 │     ├─ i>2 → other branches ❌
 └─ i>1 → other branches ❌


✅ Found {1,2,4} → total = 1
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Roughly O(2⁹) → since we explore include/exclude for 9 digits
  * Practically much less due to pruning (n < 0, k == 0)
- 💾 Space Complexity
  * O(k) → recursion depth (stack space)

---

## 6. Justification / Proof of Optimality

- This approach ensures:
- Distinct numbers (due to start parameter)
- No duplicates (combination order ignored)
- Checks all possible valid subsets efficiently
---

## 7. Variants / Follow-Ups

- Find actual combinations (store in list instead of counting)
- Allow repeated numbers (remove distinctness constraint)
- Use digits from 1–n instead of 1–9
---

## 8. Tips & Observations

- Use backtracking for combination-type problems
- Always control distinctness via the start parameter
- Recursion naturally handles combination depth (k here)
---

<!-- #endregion -->
