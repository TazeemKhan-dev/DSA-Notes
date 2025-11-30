<!-- #region 165-Missing Numbers -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q165: Missing Numbers</h1>

## 1. Problem Understanding

- You are given two arrays:
  * arr (smaller or equal)
  * brr (bigger one)
- Your task is to find which numbers in brr are missing or have different frequencies in arr.
- A number should be included in the result if:
  * It appears more times in brr than in arr, or
  * It appears in brr but does not exist in arr.
- Return the numbers sorted, and only once per number.
- If no such number exists → return -1.
---

## 2. Constraints

- 1 <= n, m <= 2*10^5
- 1 <= arr[i], brr[i] <= 10^4
- Large input → requires O(n + m) or better.
---

## 3. Edge Cases

- All frequencies match → output -1
- All elements of brr missing in arr
- Duplicate frequencies mismatch
- arr is subset of brr
- Arrays contain same numbers but different counts
- Single element arrays
---

## 4. Examples

```text
Input:
arr = [203 204 205 206 207 208 203 204 205 206]
brr = [203 204 204 205 206 207 205 208 203 206 205 206 204]

Output:
204 205 206

Example 2

arr = [1 1 2 5]
brr = [1 2 3 4]

Output:
1 3 4
```

---

## 5. Approaches

### Approach 1: Frequency HashMaps (Optimal & Recommended)

**Idea:**
- Count frequencies of each number in both arrays.
- Compare the frequencies: if mismatch → number is missing.
- Sort the result and print.

**Steps:**
- Build freqArr map
- Build freqBrr map
- Iterate through keys of freqBrr
- If freq differs or key missing → add to result
- Sort result
- Print

**Java Code:**
```java
static void missingNumbers(int n, int arr[], int m, int brr[]) {
    HashMap<Integer,Integer> freqArr = new HashMap<>();
    HashMap<Integer,Integer> freqBrr = new HashMap<>();

    for (int x : arr) freqArr.put(x, freqArr.getOrDefault(x, 0) + 1);
    for (int x : brr) freqBrr.put(x, freqBrr.getOrDefault(x, 0) + 1);

    List<Integer> ans = new ArrayList<>();

    for (int key : freqBrr.keySet()) {
        if (!freqArr.containsKey(key) || !freqArr.get(key).equals(freqBrr.get(key))) {
            ans.add(key);
        }
    }

    if (ans.size() == 0) {
        System.out.print(-1);
        return;
    }

    Collections.sort(ans);
    for (int x : ans) System.out.print(x + " ");
}
```

**💭 Intuition Behind the Approach:**
- Frequencies directly reveal mismatches.
- If a number appears more times in brr, it’s missing in arr.
- Since values ≤ 10^4, maps stay small.
- Sorting at the end ensures clean ascending output.

**Complexity (Time & Space):**
- Time:
  * Building maps → O(n + m)
  * Iterating over distinct values → O(k)
  * Sorting → O(k log k)
  * (k ≤ 10⁴, so very fast)
  * Why:
    * You must read every element → O(n + m) is the theoretical lower bound.
- Space:
  * O(k) for frequency maps
  * Because you only store distinct numbers

### Approach 2: Frequency Arrays (More memory-efficient)

**Idea:**
- Since constraints are arr[i] ≤ 10⁴, use fixed-size arrays instead of HashMaps.

**Java Code:**
```java
static void missingNumbers(int n, int arr[], int m, int brr[]) {
    int[] fa = new int[10001];
    int[] fb = new int[10001];

    for (int x : arr) fa[x]++;
    for (int x : brr) fb[x]++;

    List<Integer> ans = new ArrayList<>();

    for (int i = 1; i <= 10000; i++) {
        if (fb[i] > 0 && fa[i] != fb[i]) {
            ans.add(i);
        }
    }

    if (ans.isEmpty()) {
        System.out.print(-1);
        return;
    }

    for (int x : ans) System.out.print(x + " ");
}
```

**💭 Intuition Behind the Approach:**
- Array indexing gives instant O(1) frequency access.
- No hashing overhead → faster in practice.
- Useful when value constraints are small.

**Complexity (Time & Space):**
- Time: O(n + m + maxValue)
- Space: O(maxValue) → 10⁴ only
- Why: direct frequency arrays eliminate hashing cost.

---

## 6. Justification / Proof of Optimality

- Comparing frequencies is the only correct way to detect missing numbers when duplicates exist.
- Sorting ensures the required ascending output.
- Since values are ≤10⁴, the solution is efficient with both maps and arrays.
---

## 7. Variants / Follow-Ups

- Find extra elements in one array compared to another
- Compare difference between two multisets
- Count mismatched frequencies between lists
- Find missing numbers within range 1…N
---

## 8. Tips & Observations

- Always handle null checks when using HashMaps
- For bounded integer values, prefer arrays over Maps
- Sorting only distinct keys keeps complexity low
- Maps are naturally useful when values aren't bounded
---

<!-- #endregion -->
