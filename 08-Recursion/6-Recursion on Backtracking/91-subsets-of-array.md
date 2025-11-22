<!-- #region 91-Subsets of Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q91: Subsets of Array</h1>

## 1. Problem Understanding

- You are given an array of distinct positive integers.
- Your task is to generate all possible subsets (the power set) of this array and print them in lexicographically sorted order.
- Each subset should contain elements in the same relative order as the original array.
---

## 2. Constraints

- 1 ≤ N ≤ 10
- 1 ≤ A[i] ≤ 20
---

## 3. Edge Cases

- Array with only one element — only one subset possible.
- Empty subset is not printed (based on the given examples).
- The array is distinct, so no duplicate subsets.
---

## 4. Examples

```text
Example 1

Input:

3
10 15 20


Output:

10
10 15
10 15 20
10 20
15
15 20
20
```

---

## 5. Approaches

### Approach 1: Recursive Backtracking (Inclusion-Exclusion method)

**Idea:**
- At each index, you have two choices:
  * Include the current element in the subset.
  * Exclude it and move to the next.
- This generates all possible subsets recursively.

**Steps:**
- Start with an empty list (current subset).
- At each step, include arr[i] and recurse.
- Then, exclude arr[i] and recurse.
- Once you reach the end of the array, print the subset (if not empty).
- Sort subsets lexicographically before printing.

**Java Code:**
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();
        
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(arr);
        helper(arr, 0, new ArrayList<>(), ans);

        // Print subsets (excluding empty subset)
        for (List<Integer> subset : ans) {
            if (!subset.isEmpty()) {
                for (int num : subset) System.out.print(num + " ");
                System.out.println();
            }
        }
    }

    public static void helper(int[] arr, int idx, List<Integer> current, List<List<Integer>> ans) {
        if (idx == arr.length) {
            ans.add(new ArrayList<>(current));
            return;
        }

        // Include arr[idx]
        current.add(arr[idx]);
        helper(arr, idx + 1, current, ans);

        // Backtrack (remove last element)
        current.remove(current.size() - 1);

        // Exclude arr[idx]
        helper(arr, idx + 1, current, ans);
    }
}


🌳 Recursion Tree (for arr = [10, 15, 20])
helper([], 0)
├── Include 10 → helper([10], 1)
│   ├── Include 15 → helper([10,15], 2)
│   │   ├── Include 20 → helper([10,15,20], 3) → print [10,15,20]
│   │   └── Exclude 20 → helper([10,15], 3) → print [10,15]
│   └── Exclude 15 → helper([10], 2)
│       ├── Include 20 → helper([10,20], 3) → print [10,20]
│       └── Exclude 20 → helper([10], 3) → print [10]
└── Exclude 10 → helper([], 1)
    ├── Include 15 → helper([15], 2)
    │   ├── Include 20 → helper([15,20], 3) → print [15,20]
    │   └── Exclude 20 → helper([15], 3) → print [15]
    └── Exclude 15 → helper([], 2)
        ├── Include 20 → helper([20], 3) → print [20]
        └── Exclude 20 → helper([], 3) → print []


(We skip printing the empty subset in final output)
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(2^N) subsets possible.
  * For each subset, up to O(N) work (copying/printing).
  * ✅ Overall: O(N × 2^N)
- 💾 Space Complexity
  * Recursion stack: O(N)
  * Storage for subsets: O(N × 2^N)

---

## 6. Justification / Proof of Optimality

- This approach explores every combination via inclusion/exclusion recursion — the standard power set generation pattern. Sorting ensures lexicographic output.
---

## 7. Variants / Follow-Ups

- Subsets including empty subset (LeetCode “Subsets” problem).
- Subsets with duplicates (use a set to avoid repetition).
- Iterative (bitmask) approach to generate all subsets.
---

## 8. Tips & Observations

- For lexicographic output → sort input first.
- Each level in recursion represents one element’s inclusion/exclusion.
- Useful recursion template for many subset/combinations problems.
---

<!-- #endregion -->
