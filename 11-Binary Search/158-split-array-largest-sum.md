<!-- #region 158-Split array - largest sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q158: Split array - largest sum</h1>

## 1. Problem Understanding

- You are given an array a of size n.
- You must split it into k non-empty, consecutive subarrays.
- Each split produces subarrays like:
  * [a[0] ... a[x]],
  * [a[x+1] ... a[y]],
  * ...
- For each subarray, compute its sum.
- Among all these sums, take the maximum.
- Your goal:
  * Split array such that the maximum subarray sum is as small as possible.
  * Return this minimized maximum sum.
- This is Minimize Maximum → a perfect Binary Search on Answer (BSOA) problem.
---

## 2. Constraints

- 1 ≤ n ≤ 10^4
- 1 ≤ k ≤ n
- 1 ≤ a[i] ≤ 10^4
---

## 3. Edge Cases

- k = 1 → answer = sum of whole array
- k = n → answer = max element
- Very large numbers → use long for sums
- Array in strictly increasing order → largest splits affect
- All elements equal → simple equal distribution
---

## 4. Examples

```text
Example 1

a = [1,2,3,4,5], k = 3  
Best split = [1,2,3], [4], [5]  
Largest sum = 6


Example 2

a = [3,5,1], k = 3  
Split = [3], [5], [1]  
Largest sum = 5
```

---

## 5. Approaches

### Approach 1: Brute Force (Try all partitions)

**Idea:**
- Try all ways to split array into k subarrays, calculate maximum subarray sum each time, return minimum of all.
- Why it's impossible
  * Number of ways to place k-1 dividers between n-1 gaps:
  * C(n-1, k-1) → exponential for n=10^4
  * Not feasible.

**💭 Intuition Behind the Approach:**
- Brute = check all partitions, but search space too huge.

**Complexity (Time & Space):**
- Time: O(2^n) (unusable)
- Space: O(n)

### Approach 2: Greedy Check + Linear Search (Better but still slow)

**Idea:**
- Try a candidate largest sum S, simulate partition:
  * Keep adding numbers until sum > S
  * When greater → make a cut
  * Count number of cuts (subarrays)
- But instead of binary search, you linearly test S from max(a[i]) to sum(a).
- Why this is bad
  * Worst case sum ~ 10^8 leads to O(sum) checks → impossible.

**💭 Intuition Behind the Approach:**
- Correct checking logic, but wrong outer loop.

**Complexity (Time & Space):**
- Time: O(sum * n) → too slow

### Approach 3: Optimal): Binary Search on Answer

**Idea:**
- We binary search on maximum subarray sum possible (call it mid).
- Range:
  * low = max(a)      (min possible max sum)
  * high = sum(a)     (max possible max sum)
- For each mid:
  * Check how many subarrays we need if no subarray sum exceeds mid.
  * If we need more than k subarrays → mid too small → increase low
  * Else → mid possible → try smaller mid

**Steps:**
- Compute:
  * low = max(a[i])
  * high = sum(a[i])
- Binary search:
  * mid = (low + high) / 2
- Use greedy to count how many subarrays required with max sum ≤ mid.
- Adjust search space.

**Java Code:**
```java
public class Solution {

    public int splitArray(int[] a, int k) {
        int n = a.length;

        int low = 0, high = 0;

        for(int x : a){
            low = Math.max(low, x);  // at least largest single element
            high += x;               // at most sum of all
        }

        while(low <= high){
            int mid = low + (high - low) / 2;

            if(canSplit(a, k, mid)){
                high = mid - 1;  // try smaller largest sum
            } else {
                low = mid + 1;   // mid too small → increase
            }
        }

        return low; // the minimized maximum sum
    }

    private boolean canSplit(int[] a, int k, int maxAllowed){
        int subarrays = 1;
        int currSum = 0;

        for(int x : a){
            if(currSum + x <= maxAllowed){
                currSum += x;
            } else {
                subarrays++;
                currSum = x;
                if(subarrays > k) return false;
            }
        }
        return true;
    }
}
```

**💭 Intuition Behind the Approach:**
- Why binary search works?
- Because:
  * If a max subarray sum S is possible,
  * then ANY sum > S is also possible.
  * If S is NOT possible,
  * then ANY sum < S is also not possible.
- This monotonic behavior makes BSOA perfect.
- Why greedy checking?
  * Because to minimize the maximum subarray sum:
  * We must pack as many elements as possible into each subarray without exceeding mid.
  * If we exceed, we cut immediately.
- This greedy is always optimal.

**Complexity (Time & Space):**
- Time
- O(n log(sum(a))) because:
  * Each mid-check = O(n)
  * Binary search range log(sum)
  * You get ~30–40 iterations for integers → very fast.
- Space
  * O(1) because only counters and current sum are stored.

---

## 6. Justification / Proof of Optimality

- Binary search correct due to monotonic property of possibility function.
- Greedy splitting is optimal:
- If sum exceeds mid, we MUST split (forced cut).
- After split, best choice is to start new subarray with current element.
- Together they guarantee minimum maximum sum.
---

## 7. Variants / Follow-Ups

- Painters Partition
- Allocate minimum number of pages
- Split array to minimize max OR maximize min
- Minimize largest bag size (LeetCode “Minimum Limit of Balls”)
---

## 8. Tips & Observations

- Always identify the pattern “Minimize the Maximum”
- low = max(a[i]) ensures subarray exists
- high = sum(a[i]) ensures all numbers in one subarray possible
- Greedy split logic must never exceed mid
- Use integer binary search (no double)

- **⚠️ Pitfalls**
    - Using mid incorrectly
    - Forgetting low = max(a)
    - Splitting incorrectly (splits must be consecutive)
    - Mistaking this for DP (DP is too slow: O(n*k))
    - Missing edge case: k = n → answer = max element
    - Off-by-one in low/high answer return
---

<!-- #endregion -->
