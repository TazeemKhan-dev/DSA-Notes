<!-- #region 170-Problem With Given Difference -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q170: Problem With Given Difference</h1>

## 1. Problem Statement

- You are given an unsorted array A of size N, and an integer B.
- Check whether there exists any pair (A[i], A[j]) such that:
- |A[i] - A[j]| = B
- Return/print 1 if such a pair exists, otherwise 0.
---

## 2. Problem Understanding

- The problem wants us to find two numbers whose difference equals exactly B.
- Key clarifications:
  * Difference means absolute difference, because examples show 80 - 2 = 78 and 20 - (-10) = 30.
  * We only need to check if such a pair exists, not to return the pair.
  * N is up to 100000, so brute force O(N^2) might be too slow.
- Input example:
- Array: [5,10,3,2,50,80], B = 78
- Pair (80,2) works → print 1.
---

## 3. Constraints

- 1 <= N <= 100000
- -10000 <= A[i] <= 10000
- -20000 <= B <= 20000
---

## 4. Edge Cases

- B = 0 → we must check if any duplicate number exists.
- Array with 1 element → always output 0.
- Negative numbers allowed.
- Large N → must use O(N) or O(N log N).
---

## 5. Examples

```text
Example:
A = [20, -10], B = 30
Difference = 20 - (-10) = 30 → print 1.
```

---

## 6. Approaches

### Approach 1: e Force (Check all pairs)

**Idea:**
- Try all pairs (i, j) and check if |A[i] - A[j]| == B.

**Steps:**
- Loop i from 0 to N-1
- Loop j from i+1 to N-1
- Compare difference

**Java Code:**
```java
public int solve(int[] A, int B) {
    int n = A.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (Math.abs(A[i] - A[j]) == B) return 1;
        }
    }
    return 0;
}
```

**💭 Intuition Behind the Approach:**
- You literally check all possible pairs. Works conceptually but too slow for large N.

**Complexity (Time & Space):**
- Time: O(N^2)
  * Because nested loops.
- Space: O(1)
  * No extra data.

### Approach 2: Sorting + Two Pointers (Optimal)

**Idea:**
- Sort array.
- Use two pointers i and j (j starts ahead).
- Check difference:
  * If A[j] - A[i] == B → pair found
  * If difference < B → move j (increase difference)
  * If difference > B → move i (decrease difference)

**Steps:**
- Sort array.
- Initialize i=0, j=1.
- While i < n and j < n:
  * If i==j, move j.
  * Compute diff.
  * Adjust i or j.

**Java Code:**
```java
public int solve(int[] A, int B) {
    Arrays.sort(A);
    int i = 0, j = 1;
    int n = A.length;

    while (i < n && j < n) {
        if (i == j) {
            j++;
            continue;
        }

        int diff = A[j] - A[i];

        if (diff == B) return 1;
        else if (diff < B) j++;
        else i++;
    }

    return 0;
}
```

**💭 Intuition Behind the Approach:**
- Sorting orders numbers so the difference changes predictably.
- Two pointers allow you to adjust difference efficiently.

**Complexity (Time & Space):**
- Time: O(N log N)
  * Sorting dominates.
- Space: O(1) or O(log N) depending on sort implementation

### Approach 3: Hashing (Best Time Complexity)

**Idea:**
- For each element x, check if:
  * x + B exists
  * OR
  * x - B exists

**Steps:**
- Insert all numbers into a HashSet.
- For each value:
  * If set contains value + B → return 1
  * If set contains value - B → return 1

**Java Code:**
```java
public int solve(int[] A, int B) {
    HashSet<Integer> set = new HashSet<>();

    for (int x : A) set.add(x);

    for (int x : A) {
        if (set.contains(x + B) || set.contains(x - B))
            return 1;
    }

    return 0;
}
```

**💭 Intuition Behind the Approach:**
- Instead of searching the whole array, hash lets you check the required matching number in O(1) average time.

**Complexity (Time & Space):**
- Time: O(N)
  * Because each lookup is averaged O(1).
- Space: O(N)
  * Storing values in HashSet.

---

## 7. Justification / Proof of Optimality

- Hashing is the fastest in terms of time.
- Two pointers is also optimal but slightly slower due to sorting.
- Brute force is too slow for N = 100000.
---

## 8. Variants / Follow-Ups

- Count all pairs with difference B.
- Return the pair instead of 1/0.
- Handle absolute vs exact signed difference.
---

## 9. Tips & Observations

- Always prefer hashing when the problem needs fast presence lookups.
- If duplicates matter and B == 0, check frequency.
---

## 10. Pitfalls

- Forgetting absolute difference.
- Forgetting that B can be zero.
- Using two pointers incorrectly with overlapping pointers.
---

<!-- #endregion -->
