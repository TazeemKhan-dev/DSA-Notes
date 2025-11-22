<!-- #region 90-Coins Permutations - 1 -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q90: Coins Permutations - 1</h1>

## 1. Problem Understanding

- You are given a list of coin denominations and a total amount amt.
- You need to print all permutations (different orders) of coins that sum up to amt.
- Unlike combinations, here order matters — e.g., [2,3,7] and [3,2,7] are considered different permutations.
---

## 2. Constraints

- 1 <= n <= 30
- 1 <= coin[i] <= 20
- 0 <= amt <= 50
---

## 3. Edge Cases

- If amt == 0, print the permutation and return.
- If amt < 0, stop exploring that path.
- If no valid permutation exists, no output.
- Duplicates in coins array should not produce duplicate permutations.
---

## 4. Examples

```text
Input:

5
2 3 5 6 7
12


Output:

2 3 7
2 7 3
3 2 7
3 7 2
5 7
7 2 3
7 3 2
7 5


Explanation:
These are all distinct permutations of coins that sum to 12.
```

---

## 5. Approaches

### Approach 1: Recursive Backtracking (Pick Each Coin Every Time)

**Idea:**
- Explore each coin as the first choice.
- Subtract its value from the amount.
- Recurse to find the remaining amount.
- When amt == 0, print the current permutation.

**Steps:**
- Start with an empty permutation string and given amt.
- For each coin:
  * If coin ≤ amt → choose it and recurse with reduced amount.
  * If amt == 0 → print the permutation.
- Backtrack to explore other choices.

**Java Code:**
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scn = new Scanner(System.in);
        int n = scn.nextInt();
        int[] coins = new int[n];
        for (int i = 0; i < n; i++) coins[i] = scn.nextInt();
        int amt = scn.nextInt();
        coinPermutations(coins, amt, "");
    }

    public static void coinPermutations(int[] coins, int amt, String asf) {
        if (amt == 0) {
            System.out.println(asf.trim());
            return;
        }
        if (amt < 0) return;

        for (int coin : coins) {
            if (coin <= amt)
                coinPermutations(coins, amt - coin, asf + coin + " ");
        }
    }
}

🕸 Recursion Tree (Example)
For coins = [2, 3, 7] and amt = 5:

coinPermutations([], 5)
│
├── choose 2 → coinPermutations([2], 3)
│     ├── choose 2 → coinPermutations([2,2], 1)
│     └── choose 3 → coinPermutations([2,3], 0) ✅ print "2 3"
│
├── choose 3 → coinPermutations([3], 2)
│     ├── choose 2 → coinPermutations([3,2], 0) ✅ print "3 2"
│
└── choose 7 → skipped (7 > 5)


✅ Output:

2 3
3 2


Solution by sir
   public static void helper(int[] coins,String prem, int amt,boolean used[]){
        //Write your code here
        if(amt==0) {
            System.out.println(prem);
            return;
        }
        if(amt<0) {
            return;
        }
        for(int i=0;i<coins.length;i++){
            if(!used[i]){
                used[i]=true;
            helper(coins,prem+coins[i]+" ",amt-coins[i],used);
                used[i]=false; // remove this to sollve Coins Permutations - 2
            }
        }
    }


```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(kⁿ) where n = number of coins and k ≈ (amt / smallest coin value).
  * Each recursive path explores multiple branches → exponential.
- 💾 Space Complexity
  * O(amt) (maximum recursion depth = number of coins used to make amt).

---

## 6. Justification / Proof of Optimality

- Every permutation is explored using recursive DFS.
- Backtracking ensures no partial permutation persists after printing.
- Suitable for small n and amt values.
---

## 7. Variants / Follow-Ups

- Coin Combinations - 1: Order doesn’t matter (use index-based recursion).
- Coin Change (DP): Count ways, not print them.
- Coin Permutations - 2: Each coin can be used only once.
---

## 8. Tips & Observations

- Always reset asf(answer so far.) on backtracking.
- Be careful between “permutation” (order matters) and “combination” (order doesn’t).
- Avoid global variables — pass data in parameters.
---

<!-- #endregion -->
