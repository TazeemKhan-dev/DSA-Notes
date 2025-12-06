<!-- #region Hashing -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: Hashing</h1>

## 1. Problem Understanding

- Hashing is a technique to map data (key) → a fixed-size array (hash table) using a hash function.
  * Used to quickly:
  * Insert
  * Delete
  * Search
  * — usually in O(1) average time.
  * A hash function converts keys into indexes in an array.
- Why Hashing?
  * Array search = O(n)
  * Binary Search = O(log n)
  * Hashing = O(1) average, O(n) worst-case (if many collisions)

- **Hash Function**
    - A formula that maps a key → index.
    - Example:
      * int index = key % size;
    - Should:
      * Distribute keys uniformly.
      * Avoid collisions.
      * Be deterministic (same key → same index).

- **Collisions**
    - Occur when two keys hash to the same index.
    - Handled by:
      * Chaining: Each array cell stores a LinkedList (or list) of keys with the same hash.
      * Open Addressing: Find the next available cell (Linear / Quadratic probing).

- **Common Hash-Based Data Structures (Java)**
    - HashMap<K,V> → key–value mapping
    - HashSet<E> → stores unique elements (backed by HashMap)
    - LinkedHashMap → maintains insertion order
    - TreeMap → sorted order (Red-Black Tree, not hash-based)

- **DSA Use Cases of Hashing**
    - Count frequencies (numbers or characters)
    - Check duplicates
    - Track seen prefix sums / subarrays
    - Map relations (like Roman numeral → value)
    - Fast lookup or existence check
    - String mappings (isomorphic, anagrams)
    - Unique collections (union, intersection)

- **Hash Arrays (Static Hashing)**
    - When elements are numeric and within range, use arrays as hash tables (common in CP / DSA problems).
    - Example:
      * int[] hash = new int[1000000]; // 10^6 size
      * int[] arr = {1,4,2,4,2};
      * for (int x : arr) hash[x]++;
      * System.out.println(hash[4]); // prints 2

- **Memory Constraints in Java (⚠️ Important for DSA)**
    - Inside main():
      * int[] up to 10^6
      * boolean[] up to 10^7
    - Outside main (static/global):
      * int[] up to 10^7
      * boolean[] up to 10^8
    - Reason:
      * Local variables are stored on stack (limited memory).
      * Static/global arrays are stored on heap (larger memory).
    - Exceeding limits causes:
      * java.lang.OutOfMemoryError: Java heap space

- **Types of Hashing in DSA**
    - Direct Address Table: Use the key directly as index (when range is small).
    - Hash Table: Use hash function to map large keys to smaller indexes.
    - Double Hashing: Use two hash functions to minimize clustering in open addressing

- **Implementation Snippets**
    - Frequency of characters:
          * String s = "aabbbc";
          * int[] freq = new int[26];
          * for (char c : s.toCharArray()) freq[c - 'a']++;
          * System.out.println(freq['b' - 'a']); // freq of 'b'\
    - Frequency using HashMap:
          * HashMap<Integer, Integer> map = new HashMap<>();
          * for (int x : arr)
              * map.put(x, map.getOrDefault(x, 0) + 1);

- **Time & Space Complexity**
    - Average Case:
      * Insert → O(1)
      * Search → O(1)
      * Delete → O(1)
    - Worst Case: O(n) (many collisions)
    - Space: O(N)

- **Real-World Analogy**
    - Like a library shelf system:
      * Hash function = shelf number
      * Book = key/value
      * You go directly to that shelf → fast retrieval.

- **Common Interview Problems (Hashing-Based)**
    - Two Sum – LeetCode #1
      * → Store complement in HashMap.
    - Subarray Sum Equals K – LeetCode #560
      * → Store prefix sums in HashMap.
    - Longest Consecutive Sequence – LeetCode #128
      * → Use HashSet for fast existence checks.
    - Group Anagrams – LeetCode #49
      * → HashMap with sorted string as key.
    - Top K Frequent Elements – LeetCode #347
      * → HashMap + MinHeap.
    - Valid Anagram – LeetCode #242
      * → Count frequency via array or HashMap.
    - Intersection of Two Arrays – HashSet for membership check.

- **Best Practices for Hashing in DSA**
    - For characters: use arrays (int[26] or int[256])
    - For numbers: use HashMap or static arrays.
    - Always offset negative numbers by +10⁵ or +10⁶ before indexing.
    - Avoid large arrays if range is unknown → use HashMap.
    - Clear hash structures between test cases in coding challenges.
    - Use long as key for larger integers (e.g. prefix sums).
    - Be aware of hash collisions — use .equals() and hashCode() properly for custom objects.

- **Internal Working of HashMap (Java Internals)**
    - 1. Structure
      * Backed by an array of buckets (Node<K,V>[]).
      * Each bucket holds a linked list (or tree) of key-value pairs.
    - 2. Node<K,V> Structure
      * static class Node<K,V> {
          * final int hash;
          * final K key;
          * V value;
          * Node<K,V> next;
      * }
    - 3. hashCode()
      * Every key has a hashCode() (integer value).
      * HashMap refines this using bit manipulation:
      * int hash = (key == null) ? 0 : (key.hashCode()) ^ (key.hashCode() >>> 16);
      * → spreads hash bits to avoid collisions.
    - 4. Index Calculation
      * Index in array = hash & (n - 1) (where n = capacity, always power of 2)
    - 5. Collision Handling
      * Before Java 8: LinkedList chaining.
      * After Java 8: Converts to balanced tree (TreeNode) if chain length > 8.
    - 6. Load Factor
      * Ratio of number of elements to array size (default = 0.75).
      * When load factor exceeds threshold → rehashing (array size doubles).
    - 7. equals() and hashCode()
      * Both must be correctly overridden for user-defined keys.
      * Two keys are equal if:
      * key1.hashCode() == key2.hashCode() && key1.equals(key2)
    - 8. Null Keys and Values
      * HashMap allows one null key and multiple null values.
      * Null key is stored in index 0.

- **Common Interview Questions on HashMap Internals**
    - How does HashMap achieve O(1) complexity?
    - What happens if two keys have same hashCode()?
    - How does Java 8 improve collision handling?
    - What is load factor and resizing?
    - Why is capacity a power of 2?
    - Why should hashCode() and equals() be consistent?

- **Optimization Tips**
    - Use HashSet for unique element lookups.
    - Use LinkedHashMap when insertion order matters.
    - Use TreeMap when you need sorted keys.
    - Avoid large primitive arrays if the key range is sparse.
    - For competitive programming:
      * Prefer static arrays (faster than HashMap).
      * Use HashMap when input constraints are large or negative.

- **Quick Recap (Key Takeaways)**
    - Hashing = fast lookup using key → index mapping.
    - Use arrays for small ranges, HashMap for generic data.
    - Memory matters:
      * Inside main: 10^6
      * Static/global: 10^7 (int), 10^8 (boolean)
    - Handle collisions smartly.
    - Understand hashCode() + equals() for interviews.
    - Practice problems: Two Sum, Subarray Sum, Anagrams.
---

<!-- #endregion -->
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
<!-- #region 166-Employees and Manager -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q166: Employees and Manager</h1>

## 1. Problem Understanding

- You are given a mapping employee → manager.
- For every person, you must compute how many employees they manage, including:
  * Direct reports
  * Indirect reports (reports of their reports, recursively)
- CEO is the person whose manager is himself.
- Finally, print in lexical order of employee names:
- employee total_reports
---

## 2. Constraints

- 1 <= N <= 100
- Single manager per employee
- CEO reports to himself
- Characters used to represent employees/managers
- Indirect reports included
---

## 3. Edge Cases

- Single employee (self-reporting) → output employee 0
- Multiple disconnected chains (rare but possible in input)
- Deep hierarchy (chain-like)
- Employee with zero reports
- Output must be lexically sorted
---

## 4. Examples

```text
Input

6
A C
B C
C F
D E
E F
F F


Output

A 0
B 0
C 2
D 0
E 1
F 5
```

---

## 5. Approaches

### Approach 1: Brute Force (DFS for each employee)

**Idea:**
- For each employee X, run a DFS/BFS to count all nodes reachable under him.

**Steps:**
- Build adjacency list: manager → list of direct reports
- For each employee:
  * DFS starting from them
  * Count reachable nodes (excluding themselves)

**Java Code:**
```java
public Map<Character, Integer> countEmployees(Map<Character, Character> emp) {
    Map<Character, List<Character>> tree = new HashMap<>();

    // Build manager → direct reports list
    for (char e : emp.keySet()) {
        char m = emp.get(e);
        if (e != m) {  // ignore CEO mapping to himself
            tree.computeIfAbsent(m, k -> new ArrayList<>()).add(e);
        }
    }

    Map<Character, Integer> ans = new HashMap<>();

    for (char person : emp.keySet()) {
        ans.put(person, dfs(person, tree));
    }

    return ans;
}

private int dfs(char manager, Map<Character, List<Character>> tree) {
    int count = 0;
    if (!tree.containsKey(manager)) return 0;

    for (char child : tree.get(manager)) {
        count += 1 + dfs(child, tree);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- You repeatedly compute subtrees independently.
- Simple but inefficient since same subtrees are computed multiple times.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * DFS for each employee → N * (N) = O(N^2)
  * Space for recursion tree → O(N)
- 💾 Why this complexity?
  * Each DFS can touch all nodes in worst-case chain-like hierarchy.

### Approach 2: Single DFS with Memoization

**Idea:**
- Use a tree + memoization so each subtree-count is computed once.

**Steps:**
- Build adjacency list
- Identify CEO (employee who reports to themselves)
- DFS from CEO, computing subtree sizes
- For employees with no subtree entry, their count = 0
- Print sorted

**Java Code:**
```java
public Map<Character, Integer> countEmployees(Map<Character, Character> emp) {
    Map<Character, List<Character>> tree = new HashMap<>();
    char ceo = '#';

    // build graph
    for (char e : emp.keySet()) {
        char m = emp.get(e);
        if (e == m) ceo = e; // CEO
        else tree.computeIfAbsent(m, k -> new ArrayList<>()).add(e);
    }

    Map<Character, Integer> memo = new HashMap<>();
    dfs(ceo, tree, memo);

    // for leaf nodes not managers
    for (char e : emp.keySet()) {
        memo.putIfAbsent(e, 0);
    }

    return memo;
}

private int dfs(char manager, Map<Character, List<Character>> tree,
                Map<Character, Integer> memo) {

    if (memo.containsKey(manager)) return memo.get(manager);
    int total = 0;

    if (tree.containsKey(manager)) {
        for (char c : tree.get(manager)) {
            total += 1 + dfs(c, tree, memo);
        }
    }

    memo.put(manager, total);
    return total;
}
```

**💭 Intuition Behind the Approach:**
- Each subtree is solved once and cached.
- This converts repeated work into constant-time lookups → optimal.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Building tree → O(N)
  * DFS traversal → O(N)
  * Overall → O(N)
- 💾 Why this complexity?
    * Each employee and each edge is processed a single time.

---

## 6. Justification / Proof of Optimality

- The memoized DFS approach is optimal because each report-chain is computed only once.
- Hierarchy is a directed tree → one DFS solves all subtree sizes efficiently.
---

## 7. Variants / Follow-Ups

- Print only manager nodes (those who have at least 1 report)
- Multi-level hierarchy with depth tracking
- Count only direct reports
- Print tree structure visually
---

## 8. Tips & Observations

- Always use adjacency list for hierarchy problems
- Memoization drastically improves performance
- CEO detection = employee whose manager = themselves
- Lexically sorting ensures deterministic output
---

<!-- #endregion -->
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
<!-- #region 171-Array Pairs Divisible By K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q171: Array Pairs Divisible By K</h1>

## 1. Problem Statement

- You are given an integer array arr of even length n and an integer k.
- You must split the entire array into n/2 disjoint pairs such that each pair’s sum is divisible by k.
- Return true if such a pairing is possible, otherwise return false.
---

## 2. Problem Understanding

- We must pair up all elements. No element can be left unused.
- Each pair (a, b) must satisfy:
- (a + b) % k == 0
- This means:
  * If a % k = r, then b % k must be k - r, except:
    * If r == 0, then both elements in the pair must also have remainder 0.
    * If r == k/2 (only when k is even), then both elements must have the same remainder k/2.
- We're essentially pairing based on remainders modulo k.
- This is a frequency-matching problem, not a sorting or two-pointer problem.
---

## 3. Constraints

- 1 <= n, k <= 100000
- n is always even
- 1 <= arr[i] <= 1000000
---

## 4. Edge Cases

- Elements divisible by k must appear in even count.
- If k is even, elements with remainder k/2 must also appear in even count.
- Remainder frequencies must match: freq[r] == freq[k-r].
- If any remainders mismatch → pairing impossible.
---

## 5. Examples

```text
Given:
arr = [1,2,3,4,5,10,6,7,8,9], k = 5

Pairs:
(1,9), (2,8), (3,7), (4,6), (5,10)
All sums divisible by 5.
```

---

## 6. Approaches

### Approach 1: Frequency of Remainders (Optimal & Standard)

**Idea:**
- Two numbers a and b form a valid pair if:
- (a + b) % k == 0
- → remainders must satisfy:
- (a % k) + (b % k) == k (or 0 for exact matches).
- So:
  * Count remainders.
  * For every remainder r, ensure freq[r] == freq[k-r].
  * Handle special remainders: 0 and k/2 separately.

**Steps:**
- Create an array freq[k] to store counts of remainders.
- Loop over array, compute arr[i] % k, increment frequency.
- Check rules:
  * If freq[0] is odd → return false.
  * If k % 2 == 0 and freq[k/2] is odd → return false.
  * For each r from 1 to k-1:
    * Check if freq[r] == freq[k-r]. If not → return false.
- Otherwise return true.

**Java Code:**
```java
public boolean canArrange(int[] arr, int k) {
    int[] freq = new int[k];

    for (int x : arr) {
        int r = x % k;
        if (r < 0) r += k; // handle negatives
        freq[r]++;
    }

    if (freq[0] % 2 != 0) return false;

    if (k % 2 == 0 && freq[k / 2] % 2 != 0) return false;

    for (int r = 1; r < k; r++) {
        if (r == k - r) continue;
        if (freq[r] != freq[k - r]) return false;
    }

    return true;
}
```

**💭 Intuition Behind the Approach:**
- The only thing that matters is remainders.
- If two numbers must sum to a multiple of k, their remainders must complement each other.
- So the whole problem reduces to validating if remainder frequencies match up properly.

**Complexity (Time & Space):**
- Time: O(n + k)
  * O(n) to build frequencies
  * O(k) to validate
- Space: O(k)
  * Frequency array

### Approach 2: HashMap instead of Array (Same Logic, Slower)

**Idea:**
- Same as above but using HashMap instead of fixed array.

**Java Code:**
```java
public boolean canArrange(int[] arr, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int x : arr) {
        int r = x % k;
        if (r < 0) r += k;
        map.put(r, map.getOrDefault(r, 0) + 1);
    }

    if (map.getOrDefault(0, 0) % 2 != 0) return false;

    for (int r : map.keySet()) {
        if (r == 0) continue;
        int complement = (k - r) % k;

        if (!map.get(r).equals(map.getOrDefault(complement, 0)))
            return false;
    }

    return true;
}
```

**💭 Intuition Behind the Approach:**
- Same remainder-complement rule, different storage.

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(k)

---

## 7. Justification / Proof of Optimality

- The remainder-pairing method is mathematically guaranteed to be correct because it's derived directly from modular arithmetic properties.
- If remainder frequencies satisfy all pairing rules, valid pairs must exist.
- No other approach is simpler or faster.
---

## 8. Variants / Follow-Ups

- Find actual pairs instead of boolean result.
- Count how many valid pairings exist.
- Return pairs with minimum difference but still divisible by k.
- Multi-set pairing with constraints on number of pairs.
---

## 9. Tips & Observations

- Always normalize negative remainders (r < 0 ? r + k).
- Remainder-based problems almost always use frequency buckets.
- Special remainders (0, k/2) must be even in count because they can pair only with themselves.
---

## 10. Pitfalls

- Forgetting to normalize negative numbers (-3 % 5 = -3, not valid as a remainder).
- Forgetting special cases: 0 and k/2.
- Directly trying to sort + two-pointer — this problem is not solvable with two pointers reliably.
---

<!-- #endregion -->
<!-- #region 172-Largest Subarray with 0 Sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q172: Largest Subarray with 0 Sum</h1>

## 1. Problem Statement

- Given an integer array arr of length N, find the length of the longest contiguous subarray whose sum is exactly 0.
- You must return/print the maximum length of any such subarray.
---

## 2. Problem Understanding

- We need the longest continuous segment inside the array that adds up to 0.
- Example:
  * 15 -2 2 -8 1 7 10 23
  * Subarray [-2, 2, -8, 1, 7] gives sum = 0 and length = 5.
- The brute-force way is to try all subarrays and compute sums, which is slow (O(N^2)).
- The optimal way uses prefix sums + hashmap:
  * If prefix_sum[j] == prefix_sum[i], then the subarray (i+1 to j) has sum 0.
  * Store the earliest index for each prefix sum in a map.
  * Whenever the same sum reappears, compute length.
- This avoids recomputing subarray sums repeatedly.
---

## 3. Constraints

- -100000 <= N <= 100000
- 0 <= arr[i] <= 100000
- Large N → must use O(N) solution.
---

## 4. Edge Cases

- Array with no 0-sum subarray → answer is 0.
- If the prefix sum is 0 at index i, then subarray from 0...i is valid.
- Multiple repeated prefix sums → only store earliest index.
---

## 5. Examples

```text
Example 1
Input:
15 -2 2 -8 1 7 10 23
Output: 5

Example 2
Input:
1 2 3
Output: 0
```

---

## 6. Approaches

### Approach 1: Brute Force (Too Slow)

**Idea:**
- Try every subarray and check if its sum is 0.

**Java Code:**
```java
public int longestZeroSum(int[] arr) {
    int n = arr.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum == 0) {
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(N^2)
- Space: O(1)

### Approach 2: Prefix Sum + HashMap (Optimal)

**Idea:**
- Use prefix sums:
- Let prefix[i] = sum of elements from 0 to i.
- If prefix[j] == prefix[i], the subarray from i+1 to j has sum 0.
- Use a HashMap to store the first time each prefix sum appears.

**Steps:**
- Create map: sum → earliest index.
- Maintain running sum currSum.
- If currSum == 0, update max length.
- If sum already exists in map, compute subarray length.
- Otherwise store current sum with index.

**Java Code:**
```java
public int longestZeroSum(int[] arr) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int currSum = 0;
    int maxLen = 0;

    for (int i = 0; i < arr.length; i++) {
        currSum += arr[i];

        if (currSum == 0) {
            maxLen = i + 1;
        }

        if (map.containsKey(currSum)) {
            int length = i - map.get(currSum);
            maxLen = Math.max(maxLen, length);
        } else {
            map.put(currSum, i);
        }
    }

    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- If a prefix sum repeats, the segment between the repeated indices cancels out to zero.
- Storing the first occurrence gives the longest possible subarray.

**Complexity (Time & Space):**
- Time: O(N)
  * Why: Each element processed once.
- Space: O(N)
  * Why: Worst case all prefix sums are unique.

---

## 7. Justification / Proof of Optimality

- Prefix sum repetition is the most efficient way to detect zero-sum subarrays.
- Only prefix sum logic guarantees a linear solution.
---

## 8. Variants / Follow-Ups

- Longest subarray with sum = K.
- Count number of subarrays with sum = 0.
- Shortest subarray with sum = 0.
---

## 9. Tips & Observations

- Always store prefix sums only once (earliest index).
- If prefix sum becomes zero at index i, full range 0..i is valid.
- Negative values do not break the prefix sum logic.
---

## 10. Pitfalls

- Storing repeated sums again → breaks longest logic.
- Forgetting case when prefix sum itself becomes 0.
- Using long if values might overflow (safe to use int for constraints here).
---

<!-- #endregion -->
<!-- #region 173-Group Anagrams -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q173: Group Anagrams</h1>

## 1. Problem Statement

- You are given an array of words.
- You must group all anagrams together and print them in a single line.
- Requirements:
  * Words that are anagrams must appear together.
  * Inside each anagram group, words must remain in input order.
  * Groups must appear in lexicographical order of the FIRST WORD that appeared in that group, not in order of anagram key.
  * Output should be a single line.
---

## 2. Problem Understanding

- Two words are anagrams if:
  * They have the same characters
  * In the same frequency
  * Order does not matter
- Example: "cat", "tac", "act" → all anagrams.
- But this problem has non-standard ordering rules:
- ❗ Critical Custom Rule
- You must sort the groups based on:
  * The lexicographical order of the first word that appeared in each group.
- NOT:
  * Lexicographical order of anagram keys
  * Input order
  * Length
  * Alphabetical group name
- Example:
  * eat tea tan ate nat bat
- Groups:
  * eat-group → first word = "eat"
  * tan-group → first word = "tan"
  * bat-group → first word = "bat"
- Sorted based on first words:
  * bat < eat < tan
- Final output:
  * bat eat tea ate tan nat
- This is the rule that causes many solutions to fail.
---

## 3. Constraints

- 1 <= n <= 200
- 0 <= word.length <= 100
- All words are lowercase English letters
---

## 4. Edge Cases

- Single-word groups remain as they are.
- Empty string "" must be grouped with other empty strings.
- Large characters (long strings) still must be sorted.
- Multiple groups may have different sizes.
---

## 5. Examples

```text
Example 1

Input:

5
cat dog tac god act


Groups:

cat-group: cat, tac, act (first word = cat)

dog-group: dog, god (first word = dog)

Lexicographic group order:

cat < dog


Output:

cat tac act dog god

Example 2

Input:

6
eat tea tan ate nat bat


Output:

bat eat tea ate tan nat
```

---

## 6. Approaches

### Approach 1: HashMap + Sorted Key + First-Word Sorting (Correct + Required)

**Idea:**
- Use sorted characters as the anagram key.
- For each anagram group, store the first word encountered that created the group.
- After forming groups, sort them based on this first word (not the key).
- Then print words in group-preserved order.

**Steps:**
- Loop through each word.
- Create key by sorting characters.
- Insert into map:
- key -> list of words
- Also store:
- key -> first word (only once)
- After forming map, create list of keys.
- Sort keys by their first-word value.
- Print words in sorted group order.

**Java Code:**
```java
static String sortString(String str) {
    char ch[] = str.toCharArray();
    Arrays.sort(ch);
    return String.valueOf(ch);
}

static void printAnagramsTogether(String arr[], int n) {

    HashMap<String, List<String>> groups = new HashMap<>();
    HashMap<String, String> firstWord = new HashMap<>();

    for (String word : arr) {
        String key = sortString(word);

        groups.putIfAbsent(key, new ArrayList<>());
        groups.get(key).add(word);

        // Store FIRST appearing word of this group
        firstWord.putIfAbsent(key, word);
    }

    // Sort groups based on FIRST word lexicographically
    List<String> keys = new ArrayList<>(groups.keySet());
    Collections.sort(keys, (a, b) -> firstWord.get(a).compareTo(firstWord.get(b)));

    // Print result
    for (String key : keys) {
        for (String w : groups.get(key)) {
            System.out.print(w + " ");
        }
    }
}
```

**💭 Intuition Behind the Approach:**
- Sorted-string representation uniquely identifies an anagram group.
- A problem-specific rule requires grouping based on first word—not sorted key.
- This solves ALL tricky test cases and behaves exactly how the judge expects.

**Complexity (Time & Space):**
- Time:
  * Sorting each string: O(Log L)
  * Doing this for N strings: O(N * L log L)
  * Sorting groups by first word: O(G log G)
  * (G = number of groups)
  * Overall: O(N * L log L) l
- Space:
  * Map storage: O(N * L)
  * Key strings + first-word map: O(G * L)

---

## 7. Justification / Proof of Optimality

- Sorted-string representation uniquely identifies an anagram group.
- A problem-specific rule requires grouping based on first word—not sorted key.
- This solves ALL tricky test cases and behaves exactly how the judge expects.
---

## 8. Variants / Follow-Ups

- Return list instead of print.
- Group anagrams ignoring case.
- Group anagrams with unicode characters.
- Group anagrams by frequency array instead of sorting (optimization).
---

## 9. Tips & Observations

- Using first-word sorting gives exactly the grouping order demanded by the problem.
- Use putIfAbsent() to cleanly build groups.
- Sorted strings as keys avoid collisions.
---

## 10. Pitfalls

- Sorting groups by sorted key is WRONG (fails test cases).
- Sorting groups by input index is WRONG.
- Sorting words inside the group is WRONG; maintain original input order.
- Not storing first word of the group → cannot sort correctly.
---

<!-- #endregion -->
<!-- #region 174-Subarray sum equals K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q174: Subarray sum equals K</h1>

## 1. Problem Statement

- You are given an integer array nums and an integer k.
- You need to find the total number of contiguous subarrays whose sum is exactly k.
- Return / print that count.
---

## 2. Problem Understanding

- We’re not just checking if any subarray equals k — we must count all such subarrays.
- A subarray is contiguous, i.e., elements must be side-by-side in the original array.
- Numbers can be negative, zero, or positive.
- Because of negatives, classic two-pointer sliding window won't always work (sum can go up and down).
- Example:
  * nums = [1, 1, 1], k = 2
  * Subarrays with sum 2:
  * nums[0..1] = [1,1]
  * nums[1..2] = [1,1]
  * Answer = 2.
- We need an approach that:
  * Handles negatives.
  * Counts all possible subarrays.
  * Works fast enough for up to ~200 elements (brute force is borderline okay but we’ll still do optimal).
---

## 3. Constraints

- 1 <= nums.length <= 2 * 10^8 (given as 208, likely typo, but we’ll treat logically)
- -1000 <= nums[i] <= 1000
- -10^7 <= k <= 10^7
- Given typical constraints for this problem, we should aim for O(N).
---

## 4. Edge Cases

- No subarray sums to k → return 0.
- All elements are zero, k = 0 → many subarrays contribute.
- Negative numbers present → sliding window fails.
- Single-element subarrays: if nums[i] == k, that’s a valid subarray.
- Large positive and negative mix → prefix sum can repeat.
---

## 5. Examples

```text
nums = [1, 1, 1], k = 2
Valid subarrays:

[1, 1] (indices 0–1)

[1, 1] (indices 1–2)
Answer: 2.

nums = [1, 2, 3], k = 3
Valid subarrays:

[1, 2]

[3]
Answer: 2.
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Subarrays)

**Idea:**
- Generate all possible subarrays, compute their sum, and count how many equal k.

**Steps:**
- Loop i from 0 to n-1:
  * sum = 0
  * Loop j from i to n-1:
    * Add nums[j] to sum
    * If sum == k → increment count

**Java Code:**
```java
public int subarraySumBruteForce(int[] nums, int k) {
    int n = nums.length;
    int count = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += nums[j];
            if (sum == k) {
                count++;
            }
        }
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- You literally test every possible subarray. Simple, straightforward, but not efficient for large N.

**Complexity (Time & Space):**
- Time:
  * O(N^2)
  * We check all N*(N+1)/2 subarrays and compute sums on the fly.
- Space:
  * O(1)
  * Only a few variables.

### Approach 2: Prefix Sum + HashMap (Optimal)

**Idea:**
- Use prefix sums and a frequency map.
- Let:
  * prefixSum[i] = sum of elements from index 0 to i.
- For any subarray nums[l..r]:
  * sum(l..r) = prefixSum[r] - prefixSum[l-1]
  * We want sum(l..r) == k
  * → prefixSum[r] - prefixSum[l-1] = k
  * → prefixSum[l-1] = prefixSum[r] - k
- So, at each index r, if we know how many times prefixSum[r] - k has appeared before, that count is the number of subarrays ending at r with sum k.
- We store frequencies of prefix sums in a HashMap<Integer, Integer>:
  * Key: prefix sum value
  * Value: how many times it has occurred
- We must also initialize:
  * map.put(0, 1) → meaning sum 0 has occurred once before starting (empty prefix), to correctly count subarrays starting at index 0.

**Steps:**
- Create map to store prefixSum → frequency.
- Initialize: map.put(0, 1) and currSum = 0, count = 0.
- Iterate over nums:
  * currSum += nums[i]
  * We want currSum - k to have appeared previously.
  * If map contains currSum - k, add map.get(currSum - k) to count.
  * Then, do map.put(currSum, map.getOrDefault(currSum, 0) + 1).
- Return count

**Java Code:**
```java
public int subarraySum(int[] nums, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();
    map.put(0, 1); // prefix sum 0 occurs once initially

    int currSum = 0;
    int count = 0;

    for (int x : nums) {
        currSum += x;

        int need = currSum - k;
        if (map.containsKey(need)) {
            count += map.get(need);
        }

        map.put(currSum, map.getOrDefault(currSum, 0) + 1);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Instead of recomputing sums for each subarray, we track the running sum and see how many previous positions would make a valid subarray ending here.
- The condition:
  * prefixSum[r] - prefixSum[l-1] = k
  * becomes:
  * “How many times have we seen prefixSum[r] - k before?”
- Each such occurrence represents a valid starting point l.

**Complexity (Time & Space):**
- Time:
  * O(N)
  * Single pass through array, each map operation is amortized O(1).
- Space:
  * O(N)
  * In worst case all prefix sums are distinct and stored.

---

## 7. Justification / Proof of Optimality

- Brute force is conceptually fine but O(N^2) and not scalable when N grows.
- Prefix sum + HashMap directly encodes the math relation between subarray sums and prefix sums.
- Works for:
  * Negative numbers
  * Positive numbers
  * Mixed arrays
- Sliding window fails because it assumes non-negative numbers; this problem explicitly allows negatives.
- So the prefix sum + HashMap is the right optimal solution.
---

## 8. Variants / Follow-Ups

- Count subarrays with sum less than k.
- Count subarrays with sum at least k.
- Longest subarray with sum exactly k.
- Subarray sum equals k but only non-negative numbers → sliding window possible
---

## 9. Tips & Observations

- Always initialize map.put(0, 1) for subarray-sum prefix problems; it handles subarrays starting from index 0.
- Prefix sum technique is a pattern; same trick appears in:
  * “Longest subarray with sum k”
  * “Count subarrays with sum divisible by k”
- Use int for prefix sums here because N * max(|nums[i]|) is safe within int range for constraints given. If constraints were bigger, use long.
---

## 10. Pitfalls

- Forgetting to add map.put(0, 1) → you miss subarrays starting at index 0.
- Using sliding window with negative numbers → wrong logic.
- Overwriting frequencies instead of incrementing them.
- Misunderstanding “subarray” as “subset” (non-contiguous).
---

<!-- #endregion -->
<!-- #region 175-Find Hinged Element -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q175: Find Hinged Element</h1>

## 1. Problem Statement

- Given an array arr of length N, find an index i such that:
  * All elements before i are strictly smaller than arr[i]
  * All elements after i are strictly greater than arr[i]
- Return the 0-based index if such an element exists, otherwise return -1.
- Notes:
  * The first element can never be a hinged element.
  * If the rightmost element is the largest element, then it can be the hinged element (only if all before it are smaller).
---

## 2. Problem Understanding

- We need an element that sits like a “hinge”:
  * left side: strictly smaller
  * current element
  * right side: strictly greater
- This is essentially:
  * For index i,
    * max(left[0..i-1]) < arr[i] < min(right[i+1..end])
- You cannot check this for all i in brute-force (O(N^2)) because N can be 100000.
- We must preprocess:
  * Left max array
  * Right min array
- Then find index satisfying the condition.
---

## 3. Constraints

- 0 <= N <= 100000
- 0 <= arr[i] <= 10000
- Must run in O(N) or O(N) memory
---

## 4. Edge Cases

- Array length < 3 → always return -1
- (because first cannot be hinge, and last must be > everything else)
- Duplicate values break strict comparison
- All increasing array → last element is hinged
- All decreasing array → no hinge
---

## 5. Examples

```text
Example 1
Input:
9
5 1 4 3 6 8 10 7 9

Output:
4


At index 4 → 6:
Left: 5,1,4,3 (all < 6)
Right: 8,10,7,9 (all > 6)

Example 2
Input:
4
5 1 4 4

Output:
-1


No valid hinge.
```

---

## 6. Approaches

### Approach 1: Prefix-Max + Suffix-Min (Optimal)

**Idea:**
- Precompute:
  * leftMax[i] = max element from 0 → i
  * rightMin[i] = min element from i → end
- For index i to be hinged:
  * leftMax[i-1] < arr[i] < rightMin[i+1]

**Steps:**
- Build leftMax[]
- Build rightMin[]
- Iterate from 1 to n-2:
  * Check if leftMax[i-1] < arr[i] < rightMin[i+1]
- Special case: last index — if arr[n-1] > leftMax[n-2], it’s hinged.

**Java Code:**
```java
public int findElement(int[] arr, int n) {

    // Special case: N = 1 -> return 0
    if (n == 1) return 0;

    int[] leftMax = new int[n];
    int[] rightMin = new int[n];

    leftMax[0] = arr[0];
    for (int i = 1; i < n; i++) {
        leftMax[i] = Math.max(leftMax[i - 1], arr[i]);
    }

    rightMin[n - 1] = arr[n - 1];
    for (int i = n - 2; i >= 0; i--) {
        rightMin[i] = Math.min(rightMin[i + 1], arr[i]);
    }

    // MUST check last element FIRST (as per problem statement)
    if (arr[n - 1] > leftMax[n - 2]) return n - 1;

    // Now check internal elements
    for (int i = 1; i < n - 1; i++) {
        if (leftMax[i - 1] < arr[i] && arr[i] < rightMin[i + 1]) {
            return i;
        }
    }

    return -1;
}

```

**💭 Intuition Behind the Approach:**
- We avoid re-checking left and right sides by precomputing:
- Maximum left element up to each index
- Minimum right element from each index
- This allows O(1) condition check per index.

**Complexity (Time & Space):**
- Time: O(N)
  * One pass for leftMax, one for rightMin, one for answer.
- Space: O(N)

### Approach 2: Optimized Memory Using Single Pass (Tricky)

**Idea:**
- Skip prefix arrays and compute rightMin on the fly using a second pass.
- Not worth the complexity; stick to Approach 1.

**Complexity (Time & Space):**
- Still O(N) time
- O(1) space (but implementation gets messy)

---

## 7. Justification / Proof of Optimality

- Prefix-max + suffix-min is the industry-standard way to solve hinge/pivot/index-middle problems cleanly and reliably.
---

## 8. Variants / Follow-Ups

- Find all hinged elements instead of one.
- Find pivot where left sum equals right sum.
- Find element greater than all to its left and smaller than all to its right.
---

## 9. Tips & Observations

- Strict inequality matters → duplicates break hinge.
- First element can't be hinge because no left values.
- Last element can be hinge if it’s the largest overall.
---

## 10. Pitfalls

- Forgetting strict comparison: use <, not <=
- Missing last element special condition
- Arrays of length < 3 should return -1
---

<!-- #endregion -->
<!-- #region 176-Count Number of Pairs With Absolute Difference K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q176: Count Number of Pairs With Absolute Difference K</h1>

## 1. Problem Statement

- You are given an integer array A of size n and a non-negative integer k.
- You must count distinct unordered pairs (A[i], A[j]) such that:
  * |A[i] - A[j]| = k   AND   i != j
- Unordered pair means:
  * (1, 2) is the same as (2, 1)
  * Count it once
- You must return the count of distinct pairs, not total occurrences.
---

## 2. Problem Understanding

- We only want unique value pairs, NOT index-based pairs.
- For example:
  * arr = [1, 2, 2, 1], k = 1
  * Valid pair value = (1, 2)
  * Even though many index pairs exist, we count only ONCE.
- Two cases:
- Case 1: k > 0
  * Pair values must satisfy:
    * A[j] = A[i] + k
  * So we can use a HashSet.
- Case 2: k == 0
  * We count distinct values that appear at least twice.
    * Example:
    * arr = [1, 5, 4, 1, 2], k = 0
    * Only value 1 appears twice → answer = 1
---

## 3. Constraints

- 1 ≤ n ≤ 100000
- 0 ≤ k ≤ 100000 (given as possibly negative, but logically k must be non-negative)
- 1 ≤ A[i] ≤ 100000
---

## 4. Edge Cases

- k = 0 → count duplicates only
- Repeated elements → must count only one unordered pair
- No valid pair → answer = 0
- Large n requires O(n) or O(n log n) solution
---

## 5. Examples

```text
Example 1

Input:

5 0
1 5 4 1 2


Output:

1


Because only (1,1) is valid.

Example 2

Input:

4 1
1 2 2 1


Output:

1


Because only (1,2) is valid.
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Pairs)

**Idea:**
- Try every pair (i,j) and check if:
  * |A[i] - A[j]| = k
  * store pair in a set as (min, max) to avoid duplicates.

**Steps:**
- Loop i from 0 to n-1
- Loop j from i+1 to n-1
- If difference = k → store pair in a HashSet
- Count size of set

**Java Code:**
```java
public int countPairsBrute(int[] A, int k) {
    HashSet<String> set = new HashSet<>();

    for (int i = 0; i < A.length; i++) {
        for (int j = i + 1; j < A.length; j++) {
            if (Math.abs(A[i] - A[j]) == k) {
                int a = Math.min(A[i], A[j]);
                int b = Math.max(A[i], A[j]);
                set.add(a + "," + b);
            }
        }
    }

    return set.size();
}
```

**💭 Intuition Behind the Approach:**
- Check everything → guaranteed correct but too slow.

**Complexity (Time & Space):**
- Time: O(n^2)
- Space: O(n) for storing unique pairs

### Approach 2: Sorting + Two Pointer (Better, O(n log n))

**Idea:**
- If array is sorted, then:
  * For each i, search for value A[i] + k using:
    * Two pointers OR
    * Binary search
- Store pair only once.

**Steps:**
- Sort array
- Use pointer i and pointer j
- If A[j] - A[i] == k → count pair, move both
- If smaller → move j
- If larger → move i
- Also skip duplicates.

**Java Code:**
```java
public int countPairsTwoPointer(int[] A, int k) {
    Arrays.sort(A);
    int i = 0, j = 1, count = 0;

    while (j < A.length) {
        if (i == j) { j++; continue; }

        int diff = A[j] - A[i];

        if (diff == k) {
            count++;
            int val = A[i];
            // Skip duplicates
            while (i < A.length && A[i] == val) i++;
        } 
        else if (diff < k) j++;
        else i++;
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Sorting organizes numbers, making difference checks easier and duplicate skipping possible.

**Complexity (Time & Space):**
- Time: O(n log n)
- Space: O(1) or O(n) depending on sort

### Approach 3: HashMap + HashSet (Optimal, O(n)) → Best Approach

**Idea:**
- Two cases:
- Case 1: k = 0
- Count numbers appearing at least twice.
- Each gives one pair.
- Case 2: k > 0
- For each unique number x:
- Check if x + k exists.
- Count each pair once.

**Steps:**
- Build frequency map
- If k == 0:
- count how many freq[x] ≥ 2
- If k > 0:
- for each unique key:
- check if key + k exists

**Java Code:**
```java
public int countPairs(int[] A, int k) {

    HashMap<Integer, Integer> freq = new HashMap<>();
    for (int x : A)
        freq.put(x, freq.getOrDefault(x, 0) + 1);

    int count = 0;

    if (k == 0) {
        for (int f : freq.values())
            if (f >= 2) count++;
        return count;
    }

    HashSet<Integer> set = new HashSet<>(freq.keySet());
    for (int x : set)
        if (set.contains(x + k))
            count++;

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Difference only concerns values, not positions.
- HashSet gives instant lookup → fastest solution.

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

---

## 7. Justification / Proof of Optimality

- Brute force is too slow for n = 100000
- Two pointer requires sorting → good but slower
- Hashing uses value-difference logic directly → optimal
---

## 8. Variants / Follow-Ups

- Count ordered pairs (i,j)
- Find all pairs, not just count
- Count pairs with sum K instead of difference K
- Return boolean instead of count
---

## 9. Tips & Observations

- Always separate the k = 0 case.
- For unordered pairs, use value pairs, not indices.
- HashSet simplifies pair uniqueness.
---

## 10. Pitfalls

- Counting the same value pair multiple times (e.g., duplicates → count only once).
- Forgetting the special case k = 0, where only numbers with freq ≥ 2 count.
- Treating (a, b) and (b, a) as different → unordered pair must be counted once.
- Checking both x + k AND x - k → leads to double counting; only check x + k.
- Failing to skip duplicates when using sorting + two pointers.
- Counting index pairs instead of distinct value pairs.
---

<!-- #endregion -->
<!-- #region 95-Longest Consecutive Sequence in an Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q95: Longest Consecutive Sequence in an Array</h1>

## 1. Problem Understanding

- You are given an integer array nums.
- You must find the length of the longest consecutive elements sequence, where the sequence numbers appear in any order.
- ➡️ A “consecutive sequence” means numbers that come one after another with a difference of 1 (like 1,2,3,4).
- Important: You only care about the length, not the sequence itself.
---

## 2. Constraints

- 1 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9
---

## 3. Edge Cases

- Empty array → return 0
- Array with duplicates → count only unique numbers
- Array already consecutive → return full length
- Negative and mixed values handled normally
---

## 4. Examples

```text
Input: nums = [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: Longest consecutive sequence is [1, 2, 3, 4].
```

---

## 5. Approaches

### Approach 1: Sorting-Based

**Idea:**
- Sort the array and then count consecutive elements. Skip duplicates and reset when the sequence breaks.

**Steps:**
- Sort the array.
- Use variables currLen and maxLen to track streaks.
- Compare consecutive elements and update counts.

**Java Code:**
```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;
    Arrays.sort(nums);
    int maxLen = 1, currLen = 1;
    
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1]) continue;
        if (nums[i] == nums[i - 1] + 1) currLen++;
        else currLen = 1;
        maxLen = Math.max(maxLen, currLen);
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Sorting: O(n log n)
  * Single traversal: O(n)
  * Total: O(n log n)
- 💾 Space Complexity
  * O(1) (ignoring sort overhead)

### Approach 2: Using HashSet (Optimal)

**Idea:**
- Use a HashSet to check existence of consecutive numbers quickly.
- Start counting only when the current number is the start of a sequence (i.e., num - 1 is not present).

**Steps:**
- Add all numbers to a HashSet.
- For each number, if num - 1 doesn’t exist, start a new sequence.
- Keep counting num + 1, num + 2, ... until break.
- Track the maximum streak length.

**Java Code:**
```java
public int longestConsecutive(int[] nums) {
    HashSet<Integer> set = new HashSet<>();
    for (int num : nums) set.add(num);

    int maxLen = 0;

    for (int num : set) {
        if (!set.contains(num - 1)) { // sequence start
            int curr = num;
            int len = 1;
            
            while (set.contains(curr + 1)) {
                curr++;
                len++;
            }
            maxLen = Math.max(maxLen, len);
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Insert all elements: O(n)
  * Traverse and count sequences: O(n) (each element checked once)
  * Total: O(n)
- 💾 Space Complexity
  * O(n) for HashSet storage

### Approach 3: Using HashMap (Alternate)

**Idea:**
- Use a HashMap to dynamically merge sequences.
- Each number connects to the length of its current sequence, and merging happens when neighboring sequences exist.

**Steps:**
- For each element, get lengths of left and right neighboring sequences.
- New sequence length = left + right + 1.
- Update boundaries (num - left and num + right) with new length.
- Keep track of the longest sequence.

**Java Code:**
```java
public int longestConsecutive(int[] nums) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int maxLen = 0;

    for (int num : nums) {
        if (map.containsKey(num)) continue;

        int left = map.getOrDefault(num - 1, 0);
        int right = map.getOrDefault(num + 1, 0);
        int len = left + right + 1;

        map.put(num, len);
        map.put(num - left, len);
        map.put(num + right, len);

        maxLen = Math.max(maxLen, len);
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Each element processed once: O(n)
  * HashMap operations: O(1) average
  * Total: O(n)
- 💾 Space Complexity
  * O(n) for HashMap

---

## 6. Justification / Proof of Optimality

- Sorting is simpler but slower (O(n log n)).
- HashSet is optimal and easy to reason about.
- HashMap approach is powerful for merging intervals (advanced variant).
---

## 7. Variants / Follow-Ups

- Longest consecutive subsequence in a sorted array.
- Longest sequence with given difference k.
- Used in problems like “Longest Band” or “Union of consecutive ranges.”
---

## 8. Tips & Observations

- Always check num - 1 to find the start of a sequence.
- HashSet avoids unnecessary duplicate counting.
- The logic applies for negative, positive, and unordered values.
- This is a great example of how to convert O(n log n) sorting logic into O(n) using hashing.
---

<!-- #endregion -->
<!-- #region 96-Longest Subarray with Sum K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q96: Longest Subarray with Sum K</h1>

## 1. Problem Understanding

- You are given an array nums of integers and a target sum k.
- You need to find the length of the longest contiguous subarray whose sum equals k.
- If no such subarray exists, return 0.
---

## 2. Constraints

- 1 <= n <= 10⁵
- -10⁵ <= nums[i] <= 10⁵
- -10⁹ <= k <= 10⁹
---

## 3. Edge Cases

- All numbers are positive → sliding window approach works.
- Array contains negative numbers → need prefix sum + HashMap.
- k = 0 (check for subarrays summing to zero).
- No subarray equals k → return 0.
---

## 4. Examples

```text
Input: nums = [10, 5, 2, 7, 1, 9], k = 15
Output: 4
Explanation: [5, 2, 7, 1] has sum = 15.
```

---

## 5. Approaches

### Approach 1: Brute Force (O(n²))

**Idea:**
- Try every possible subarray and compute its sum.
- Keep track of the longest one equal to k.

**Steps:**
- Use two loops — outer for start index, inner for end index.
- Compute sum for each subarray.
- If sum == k → update longest length.

**Java Code:**
```java
int longestSubarraySumK(int[] nums, int k) {
    int n = nums.length;
    int maxLen = 0;
    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += nums[j];
            if (sum == k)
                maxLen = Math.max(maxLen, j - i + 1);
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n²) → nested loops
  * For large n (10⁵), not efficient
- 💾 Space Complexity
  * O(1)

### Approach 2: Prefix Sum + HashMap (Optimized O(n))

**Idea:**
- Use a prefix sum to keep track of cumulative sums, and store the first occurrence of each prefix in a HashMap.
- If prefixSum - k exists in map → a subarray with sum k exists from that index to current.

**Steps:**
- Initialize sum = 0, maxLen = 0, and a HashMap<Integer, Integer> prefixMap.
- For each element:
  * Add it to sum.
  * If sum == k → update maxLen.
  * If sum - k exists in map → subarray found, update maxLen.
  * If sum not in map → store it with current index (only first occurrence).
- Return maxLen.

**Java Code:**
```java
int longestSubarraySumK(int[] nums, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int sum = 0, maxLen = 0;

    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];

        if (sum == k) maxLen = i + 1;

        if (map.containsKey(sum - k)) {
            maxLen = Math.max(maxLen, i - map.get(sum - k));
        }

        if (!map.containsKey(sum)) {
            map.put(sum, i);
        }
    }

    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) → single pass with O(1) HashMap lookups
- 💾 Space Complexity
  * O(n) → HashMap storing prefix sums

### Approach 3: Sliding Window (Only for Non-Negative Numbers)

**Idea:**
- When all elements are non-negative, we can use a sliding window to expand/shrink the subarray.

**Steps:**
- Maintain two pointers l and r, and a running sum.
- Expand r to increase sum.
- If sum > k → move l forward.
- If sum == k → update maxLen.

**Java Code:**
```java
int longestSubarraySumK(int[] nums, int k) {
    int left = 0, right = 0, sum = 0, maxLen = 0;
    int n = nums.length;

    while (right < n) {
        sum += nums[right];

        while (sum > k) {
            sum -= nums[left];
            left++;
        }

        if (sum == k) {
            maxLen = Math.max(maxLen, right - left + 1);
        }

        right++;
    }

    return maxLen;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) → each element added/removed once
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- The HashMap + Prefix Sum approach is the most optimal because:
  * It handles both positive and negative numbers.
  * It gives O(n) time complexity.
---

## 7. Variants / Follow-Ups

- Count of subarrays with sum K (instead of longest length).
- Subarray sum divisible by K
- Longest subarray with sum ≤ K
---

## 8. Tips & Observations

- Always store first occurrence of prefix sum — it gives longest subarray.
- Resetting map or overwriting sums can shorten valid subarrays.
- For strictly positive arrays → sliding window is even simpler and faster.
---

<!-- #endregion -->
<!-- #region 97-Count subarrays with given sum -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q97: Count subarrays with given sum</h1>

## 1. Problem Understanding

- You are given an integer array nums and an integer k.
- You need to count all subarrays whose sum equals k.
- Subarrays are contiguous sequences within the array.
---

## 2. Constraints

- 1 <= nums.length <= 10^5 → implies O(n²) brute force may TLE, we need O(n).
- -1000 <= nums[i] <= 1000 → elements can be negative.
- -10^7 <= k <= 10^7.
---

## 3. Edge Cases

- Array contains negative numbers → prefix sum with hashmap works.
- Array has all zeros, k = 0 → multiple subarrays sum to 0.
- Single element array equal to k.
---

## 4. Examples

```text
Input: [1, 1, 1], k = 2 → Output: 2 → [1,1], [1,1]

Input: [1, 2, 3], k = 3 → Output: 2 → [1,2], [3]

Input: [3, 1, 2, 4], k = 6 → Output: 1 → [2,4]
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check all possible subarrays and sum them.

**Steps:**
- Initialize count = 0
- Loop i from 0 to n-1
- Loop j from i to n-1
- Sum nums[i..j], if sum == k → count++

**Java Code:**
```java
public int subarraySum(int[] nums, int k) {
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        int sum = 0;
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
            if (sum == k) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n²)
- 💾 Space Complexity:
  * O(1)

### Approach 2: Prefix Sum + HashMap (Optimized) ✅

**Idea:**
- Use a prefix sum and store counts of prefix sums in a hashmap.
- For each new prefix sum currSum, check if (currSum - k) exists in the map → add its frequency to result.

**Steps:**
- Initialize map = {0:1}, sum = 0, count = 0
- Loop over each element in nums:
- sum += nums[i]
- If (sum - k) exists in map → count += map.get(sum - k)
- map[sum] = map.getOrDefault(sum, 0) + 1

**Java Code:**
```java
import java.util.HashMap;

public class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);  // base case
        int sum = 0, count = 0;
        for (int num : nums) {
            sum += num;
            if (map.containsKey(sum - k)) {
                count += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n) → single pass over array
- 💾 Space Complexity:
  * O(n) → hashmap storing prefix sums

---

## 6. Justification / Proof of Optimality

- The hashmap keeps track of how many times a prefix sum occurred.
- sum - k exists → there exists a previous prefix sum that forms a subarray summing to k.
---

## 7. Variants / Follow-Ups

- Count subarrays with sum ≤ k → need sliding window for positive numbers.
- Longest subarray with sum = k → similar prefix sum + hashmap, store first index.
---

## 8. Tips & Observations

- Always initialize map.put(0,1) → handles subarrays starting from index 0.
- Works for negative numbers too.
- For longest subarray, store first occurrence of each prefix sum.
---

<!-- #endregion -->
<!-- #region 98-Count subarrays with given xor K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q98: Count subarrays with given xor K</h1>

## 1. Problem Understanding

- You are given an integer array nums and an integer k.
- You need to count all subarrays whose XOR of elements equals k.
- XOR (^) is bitwise exclusive OR.
---

## 2. Constraints

- 1 <= nums.length <= 10^5 → O(n²) brute force may TLE, need O(n).
- 1 <= nums[i] <= 10^9 → elements are positive integers.
- 1 <= k <= 10^9.
---

## 3. Edge Cases

- Single element equal to k → counts as one subarray.
- Multiple subarrays with same XOR.
- Large numbers → ensure XOR calculations are correct (int is fine in Java).
---

## 4. Examples

```text
Input: [4, 2, 2, 6, 4], k = 6 → Output: 4 → [4,2], [4,2,2,6,4], [2,2,6], [6]

Input: [5, 6, 7, 8, 9], k = 5 → Output: 2 → [5], [5,6,7,8,9]

Input: [5, 2, 9], k = 7 → Output: 0
```

---

## 5. Approaches

### Approach 1: Brute Force

**Idea:**
- Check XOR for all subarrays.

**Steps:**
- Initialize count = 0
- Loop i from 0 to n-1
- Loop j from i to n-1 → xor = XOR(nums[i..j])
- If xor == k → count++

**Java Code:**
```java
public int subarrayXor(int[] nums, int k) {
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        int xor = 0;
        for (int j = i; j < nums.length; j++) {
            xor ^= nums[j];
            if (xor == k) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n²) → TLE for large n
- 💾 Space Complexity:
  * O(1)

### Approach 2: Prefix XOR + HashMap (Optimized) ✅

**Idea:**
- Let prefixXOR[i] = XOR of all elements from 0 to i.
- For subarray nums[l..i], XOR = prefixXOR[i] ^ prefixXOR[l-1]
- If prefixXOR[i] ^ k exists in map → there exists a subarray ending at i with XOR k.

**Steps:**
- Initialize map = {0:1}, xor = 0, count = 0
- Loop over each element in nums:
  * xor ^= nums[i]
  * If (xor ^ k) exists in map → count += map.get(xor ^ k)
  * map[xor] = map.getOrDefault(xor, 0) + 1

**Java Code:**
```java
import java.util.HashMap;

public class Solution {
    public int subarrayXor(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1); // base case
        int xor = 0, count = 0;
        for (int num : nums) {
            xor ^= num;
            count += map.getOrDefault(xor ^ k, 0);
            map.put(xor, map.getOrDefault(xor, 0) + 1);
        }
        return count;
    }
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity:
  * O(n) → single pass over array
- 💾 Space Complexity:
  * O(n) → hashmap storing prefix XOR counts

---

## 6. Justification / Proof of Optimality

- The hashmap stores frequency of prefix XORs.
- xor ^ k in map → there exists a previous prefix XOR that will form a subarray with XOR = k.
- Works for large positive integers.
---

## 7. Variants / Follow-Ups

- Count subarrays with XOR ≤ k → needs trie-based approach.
- Find longest subarray with XOR k → use first occurrence of prefix XOR in map.
---

## 8. Tips & Observations

- XOR is reversible: a ^ b = c → a = b ^ c
- Initialize map.put(0,1) → handles subarrays starting from index 0.
- Similar template to prefix sum with hashmap, just replace + with ^.
---

<!-- #endregion -->
