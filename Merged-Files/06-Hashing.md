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

## 1. Problem Statement

- Given a mapping of employee → manager, compute the number of employees under each person, including:
  - Direct reports
  - Indirect reports (recursive)
- CEO is the one who manages himself.
- You must print:
  - employee total_reports
- For every employee, sorted lexicographically.

---

## 2. Problem Understanding

- We are given a hierarchy:
  - Each employee has exactly one manager
  - CEO reports to himself
  - We must count all people working under each employee
  - “Under” includes:
    - Direct reports
    - Reports of reports
    - Multi-level chain
- This is effectively subtree size computation in a tree/forest.

---

## 3. Constraints

- 1 <= N <= 100
- Single manager per employee
- CEO reports to himself
- Characters used to represent employees/managers
- Indirect reports included

---

## 4. Edge Cases

- Single employee (self-reporting) → output employee 0
- Multiple disconnected chains (rare but possible in input)
- Deep hierarchy (chain-like)
- Employee with zero reports
- Output must be lexically sorted

---

## 5. Examples

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

## 6. Approaches

### Approach 1: Brute Force (DFS for each employee)

**Idea:**

- For each employee X, run a DFS/BFS to count all nodes reachable under him.

**Steps:**

- Build adjacency list: manager → list of direct reports
- For each employee:
  - DFS starting from them
  - Count reachable nodes (excluding themselves)

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
  - DFS for each employee → N \* (N) = O(N^2)
  - Space for recursion tree → O(N)
- 💾 Why this complexity?
  - Each DFS can touch all nodes in worst-case chain-like hierarchy.

### Approach 2: CEO-based DFS (Clean Version of Your Logic)

**Idea:**

- Identify CEO
- Build a map:
- manager → list of direct employees
- Run one DFS starting from CEO
- DFS returns:
- total employees under manager + 1

**Steps:**

- Parse input
- Build adjacency list of manager → employees
- Detect CEO
- DFS from CEO
- Store each manager’s direct+indirect employee count
- Print all employees (sorted)

**Java Code:**

```java
public Map<String, Integer> countEmployees(Map<String, String> emp) {

    // manager → list of direct reports
    Map<String, List<String>> tree = new HashMap<>();
    String ceo = "";

    // Build tree + identify CEO
    for (String e : emp.keySet()) {
        String m = emp.get(e);
        if (e.equals(m)) {
            ceo = e;  // CEO
        } else {
            tree.computeIfAbsent(m, k -> new ArrayList<>()).add(e);
        }
    }

    // Store results
    Map<String, Integer> ans = new HashMap<>();

    // DFS from CEO
    dfs(ceo, tree, ans);
    return ans;
}

private int dfs(String manager, Map<String, List<String>> tree,
                Map<String, Integer> ans) {

    // If manager has no reports → leaf
    if (!tree.containsKey(manager)) {
        ans.put(manager, 0);
        return 1; // count itself for parent
    }

    int total = 0;

    // Count all direct + indirect reports
    for (String emp : tree.get(manager)) {
        total += dfs(emp, tree, ans);
    }

    ans.put(manager, total);    // store counts excluding itself
    return total + 1;           // return subtree size including itself
}



Input Used for Dry Run


6
A C
B C
C F
D E
E F
F F


Meaning:

A → C
B → C
C → F
D → E
E → F
F → F (CEO)

✅ 1. Build the tree

We loop through emp:

Employee	Manager
A	C
B	C
C	F
D	E
E	F
F	F

CEO = "F" (because F = F)

Tree becomes:

F → [C, E]
C → [A, B]
E → [D]


Employees (leaves): A, B, D.

✅ 2. Start DFS from CEO “F”

We call:

dfs("F")


Important:
Your DFS returns:

return subtree_size_including_self

but stores ans[manager] = subtree_size_excluding_self

So we track both.

🚀 3. Step-by-step Dry Run
⭐ DFS("F")

F has children → ["C", "E"]

total = dfs("C") + dfs("E")


Let’s compute each.

🟦 DFS("C")

C has children → ["A", "B"]

total = dfs("A") + dfs("B")

➤ DFS("A")

A has no children → leaf

So:

ans["A"] = 0
return 1   // counts itself (1 node in subtree)

➤ DFS("B")

Same:

ans["B"] = 0
return 1


Now C computes:

total = 1 + 1 = 2
ans["C"] = 2
return 3       // including C itself → 2 employees + C

🟧 DFS("E")

E → ["D"]

So:

total = dfs("D")

➤ DFS("D")

Leaf

ans["D"] = 0
return 1


Now E computes:

total = 1
ans["E"] = 1
return 2     // including E itself → 1 employee + E

🟥 Back to DFS("F")

Now we have:

dfs("C") returned 3
dfs("E") returned 2


So:

total = 3 + 2 = 5
ans["F"] = 5       // meaning F has 5 employees under it
return 6           // including itself

🎯 4. Final ans before filling missing entries

After DFS:

A = 0
B = 0
C = 2
D = 0
E = 1
F = 5


Nothing missing (all employees covered), so final answer stays same.

🌳 5. Full Recursion Tree (Annotated)
                             dfs(F)
                     returns 6  (stores 5 in ans)
                     /                      \
            dfs(C)                          dfs(E)
     returns 3 (stores 2)            returns 2 (stores 1)
           /       \                        |
     dfs(A)       dfs(B)                dfs(D)
returns 1 (0)  returns 1 (0)       returns 1 (0)


Legend:

returns X → subtree size including self

stores Y → ans[manager] = number of employees under manager

So:

Node	dfs returns	ans stores
A	1	0
B	1	0
D	1	0
C	3	2
E	2	1
F	6	5
🧠 6. Why does ans["F"] = 5?

Because:

Employees under F → C, A, B, E, D = 5 employees

DFS returns 6 because that includes F itself.

✅ Final Output
A = 0
B = 0
C = 2
D = 0
E = 1
F = 5

```

**💭 Intuition Behind the Approach:**

- Think of each employee as a node in a tree.
- CEO is the root
- Manager–employee relationships form edges
- The number of employees under a manager = size of its subtree − 1
- Your DFS returns:
  - size of subtree = total reports + 1 (for the node itself)
- We store:
  - total reports = subtree size - 1

**Complexity (Time & Space):**

- Time Complexity: O(N)
- Space Complexity: O(N)

---

### Approach 3: Single DFS with Memoization

**Idea:**

- Use a tree + memoization so each subtree-count is computed once.
- this solution will count the number below manager as u can see it in dry run so ignore this for this q, just for refrence if we donot have to count the manager itself

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

We will dry run using the example:

6
A C
B C
C F
D E
E F
F F


So relationships:

F → C, E
C → A, B
E → D

✅ 1. Build the Tree (Adjacency List)

Input mapping:

A → C
B → C
C → F
D → E
E → F
F → F (CEO)


Adjacency list becomes:

tree = {
    F : [C, E],
    C : [A, B],
    E : [D]
}


CEO = F

✅ 2. Now DFS Starts at F

We call:

dfs(F)


Let’s dry run function by function.

🚀 3. Dry Run Step-by-Step
dfs(F)

memo does not contain F

tree.containsKey(F) → yes

F has children [C, E]

So:

total = 1 + dfs(C)
       + 1 + dfs(E)


Let’s compute dfs(C) first.

🟦 dfs(C)

Children of C = [A, B]

total = (1 + dfs(A)) + (1 + dfs(B))

➤ dfs(A)

A has no children

tree.containsKey(A) = false
→ total = 0
→ memo[A] = 0
Return 0

So dfs(A) contributes: 1 + 0 = 1

➤ dfs(B)

Same logic as A
→ memo[B] = 0
Return 0

dfs(B) contributes: 1 + 0 = 1

Summarize dfs(C)
total = 1 (from A) + 1 (from B)
      = 2
memo[C] = 2
Return 2


So → C has 2 employees under it: A and B

🟩 Back to dfs(F)

So far:

total = 1 + dfs(C)
      = 1 + 2
      = 3


Now compute dfs(E).

🟧 dfs(E)

Children of E = [D]

total = 1 + dfs(D)

➤ dfs(D)

No children
→ memo[D] = 0
Return 0

So dfs(E) total:

total = 1 + 0 = 1
memo[E] = 1
Return 1

🟥 Back to dfs(F)

Now total becomes:

total = 3 (from C) + 1 (from E)
      = 4
memo[F] = 4
Return 4


But remember:
This result is subtree size excluding itself.
So:

F has 5 employees under him?
No → 4 is correct because subtree size - 1 = total reports.

(Although in some constraints the CEO output is 5 if counting itself, but your code returns direct reports only.)

🎄 4. Full Recursion Tree Diagram (Visual)
 here is the exact CUSTOM recursion tree:

                           dfs(F)
                 /                           \
            dfs(C)                          dfs(E)
          /       \                           |
     dfs(A)      dfs(B)                    dfs(D)
       |           |                        |
     return 0    return 0                return 0


Now annotate it with returned totals:

                           dfs(F)
                             |
                         returns 4
                 /                           \
            dfs(C) (returns 2)             dfs(E) (returns 1)
          /       \                           |
     dfs(A)=0   dfs(B)=0                 dfs(D)=0

🎯 5. Final memo Map After DFS
A → 0
B → 0
C → 2
D → 0
E → 1
F → 4


That matches:

A 0
B 0
C 2
D 0
E 1
F 5 (if counting itself)


Your version outputs 4 for F since you're storing only subtree without self.
```

**💭 Intuition Behind the Approach:**

- Each subtree is solved once and cached.
- This converts repeated work into constant-time lookups → optimal.

**Complexity (Time & Space):**

- ⏱️ Time Complexity
  - Building tree → O(N)
  - DFS traversal → O(N)
  - Overall → O(N)
- 💾 Why this complexity?
  - Each employee and each edge is processed a single time.

## 7. Justification / Proof of Optimality

- The memoized DFS approach is optimal because each report-chain is computed only once.
- Hierarchy is a directed tree → one DFS solves all subtree sizes efficiently.

---

## 8. Variants / Follow-Ups

- Print only manager nodes (those who have at least 1 report)
- Multi-level hierarchy with depth tracking
- Count only direct reports
- Print tree structure visually

---

## 9. Tips & Observations

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

### Approach 1: Brute Force (Check all pairs)

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

Example 2
Input

5 10
1 2 3 4 5 6
Output

false
Explanation

there is no way to divide arr into 3 pairs each with sum divisible by 10.


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
<!-- #region 177-Roll number problem -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q177: Roll number problem</h1>

## 1. Problem Statement

- You are given an array nums of size N containing numbers from 1 to N.
- Exactly one number is missing and one number appears twice.
- Find the repeating and missing numbers.
---

## 2. Problem Understanding

- The problem gives us numbers from 1 to N, but:
  * One number is repeated
  * One number is missing
- Example:
  * [1, 4, 2, 5, 2]
  * Here:
  * 2 occurs twice → repeating
  * 3 never appears → missing
- We must find both.
---

## 3. Constraints

- 2 <= N <= 100000
- 1 <= nums[i] <= N
- Exactly one missing and one repeating number.
---

## 4. Edge Cases

- Repeating number at the beginning: [2, 2]
- Missing number at the beginning: [2, 3, 4, 4]
- Large N → efficiency matters.
---

## 5. Examples

```text
Example 1
Input: 5
1 4 2 5 2
Output: 2 3

Example 2
Input: 2
2 2
Output: 2 1
```

---

## 6. Approaches

### Approach 1: HashMap Counting (Brute but Efficient)

**Idea:**
- Count frequency of each number.
  * The one with frequency 2 → repeating
  * The one with frequency 0 → missing

**Steps:**
- Build a frequency map.
- Traverse from 1 to N.
- Detect repeating and missing

**Java Code:**
```java
static int[] findRepeatingAndMissingNumbers(int[] nums) {
    HashMap<Integer, Integer> h = new HashMap<>();
    int repeating = -1, missing = -1;

    for (int x : nums)
        h.put(x, h.getOrDefault(x, 0) + 1);

    for (int i = 1; i <= nums.length; i++) {
        if (!h.containsKey(i)) missing = i;
        else if (h.get(i) == 2) repeating = i;
    }

    return new int[]{repeating, missing};
}
```

**💭 Intuition Behind the Approach:**
- We know each number from 1 to N should appear exactly once.
- By counting occurrences:
  * A number appearing twice → the repeated
  * A number not appearing → the missing
- Simple and clear.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) because we traverse array + numbers from 1..N.
- 💾 Space Complexity
  * O(N) because of HashMap.

### Approach 2: Math Formula (Optimal, O(1) Space)

**Idea:**
- Let:
  * S = sum(nums)
  * S_actual = N*(N+1)/2
  * sq = sum of squares(nums)
  * sq_actual = N*(N+1)*(2N+1)/6
- Let:
  * x = repeating
  * y = missing
- We get:
  * x - y = S - S_actual
  * x^2 - y^2 = sq - sq_actual
  * => (x - y)(x + y) = ...
- Solve equations to get x and y.

**Java Code:**
```java
static int[] findRepeatingAndMissingNumbers(int[] nums) {
    long n = nums.length;
    long S = 0, sq = 0;

    for (int x : nums) {
        S += x;
        sq += (long)x * x;
    }

    long S_actual = n * (n + 1) / 2;
    long sq_actual = n * (n + 1) * (2 * n + 1) / 6;

    long diff = S - S_actual;           // x - y
    long sqDiff = sq - sq_actual;       // x^2 - y^2 = (x-y)(x+y)

    long sum = sqDiff / diff;           // x + y

    long x = (diff + sum) / 2;          // repeating
    long y = sum - x;                   // missing

    return new int[]{(int)x, (int)y};
}
```

**💭 Intuition Behind the Approach:**
- We use the fact that with perfect numbers 1..N, the formulas for sum and square sum are known.
- Deviations from actual → tell us:
  * One number added twice
  * One number removed
- This gives two equations to solve for repeating & missing.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N)
- 💾 Space Complexity
  * O(1) (optimal)

### Approach 3: XOR Method (Also O(1) Space)

**Idea:**
- If we XOR everything:
- X = (1 ⊕ 2 ⊕ 3 ⊕ ... ⊕ N) ⊕ (all elements in array)
- We will get:
- X = missing ⊕ repeating   (because every normal number cancels out)
- Now we have one combined XOR result.
- We must separate missing and repeating.
- We use the rightmost set bit of X to divide numbers into 2 groups:
- Group A → numbers with that bit set
- Group B → numbers without that bit set
- Because missing and repeating differ at that bit,
- they fall into different groups → letting us isolate them.
- Then we XOR again within groups to get individual values.
- After that, we check which one occurs twice to know which is repeating.

**Steps:**
- Compute XOR of:
  * All numbers from 1..N
  * All numbers in array
  * This gives: xor = repeating ⊕ missing
- Extract rightmost set bit:
  * rsb = xor & -xor
- Split numbers into 2 groups:
  * Group 1: numbers having this rsb bit
  * Group 2: numbers NOT having this rsb bit
- XOR inside both groups separately to find two values:
- val1 and val2
- Check which value actually appears twice → repeating
- The other is missing.

**Java Code:**
```java
static int[] findRepeatingAndMissingNumbers(int[] nums) {

    int n = nums.length;
    int xor = 0;

    // Step 1: XOR all nums and numbers from 1..N
    for (int x : nums) xor ^= x;
    for (int i = 1; i <= n; i++) xor ^= i;

    // xor = repeating ^ missing

    // Step 2: Get rightmost set bit
    int rsb = xor & -xor;

    int bucket1 = 0, bucket2 = 0;

    // Step 3: Divide numbers based on rsb
    for (int x : nums) {
        if ((x & rsb) != 0) bucket1 ^= x;
        else bucket2 ^= x;
    }

    for (int i = 1; i <= n; i++) {
        if ((i & rsb) != 0) bucket1 ^= i;
        else bucket2 ^= i;
    }

    // Now bucket1 and bucket2 contain missing and repeating in some order
    int repeating = 0, missing = 0;

    // Step 4: Detect which is repeating
    for (int x : nums) {
        if (x == bucket1) {
            repeating = bucket1;
            missing = bucket2;
            return new int[]{repeating, missing};
        }
    }

    // Else bucket2 is repeating
    repeating = bucket2;
    missing = bucket1;

    return new int[]{repeating, missing};
}
```

**💭 Intuition Behind the Approach:**
- XOR has a unique property:
    * a ⊕ a = 0
    * a ⊕ 0 = a
- So when all correct numbers cancel each other out,
- we are left with:
  * repeating ⊕ missing
- The rightmost set bit ensures the two target numbers lie in different buckets,
- allowing us to isolate them—like splitting data based on a unique fingerprint bit.
- This avoids extra memory, unlike HashMap.
- Pitfalls
  * Forgetting to check which bucket is repeating → wrong answer
  * Not using the rightmost set bit — needed for separation
  * Using overflow-prone math formulas — XOR avoids that

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N) (single pass XOR operations)
- 💾 Space Complexity
  * O(1) (no extra arrays / maps)
- Why?
  * We use only a few integers (xor buckets).
  * XOR cancels extra values automatically — very memory-efficient.

---

## 7. Justification / Proof of Optimality

- We covered all valid approaches:
- HashMap → simple, intuitive
- Math → optimal, constant space
- XOR → optimal, no overflow issues
- Given constraints (N <= 1e5), all approaches run in time.
---

## 8. Variants / Follow-Ups

- Find missing numbers when k numbers missing
- Find only duplicate numbers
- Find missing ranges
- Detect missing + repeating when array starts from 0
---

## 9. Tips & Observations

- Simple sum formula often works best.
- Be careful: sum might overflow → use long.
- HashMap is the most readable but not optimal for memory.
---

## 10. Pitfalls

- Doing missing = sum_expected - sum_array is WRONG when duplicates exist.
- Not using long → overflow.
- Forgetting that exactly one number repeats twice.
---

<!-- #endregion -->
<!-- #region 178-Subarray sum divisible by k -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q178: Subarray sum divisible by k</h1>

## 1. Problem Statement

- You are given an integer array nums and an integer k.
- Find the count of non-empty subarrays whose sum is divisible by k.
- A subarray is a contiguous portion of the array.
---

## 2. Problem Understanding

- We need to count how many subarrays (i…j) satisfy:
  * (sum from i to j) % k == 0
- A direct brute force would check all possible (i, j) pairs, but that is too slow for n = 5000 (≈12.5 million checks).
- We need an efficient way using prefix sums + remainder frequency.
---

## 3. Constraints

- n ≤ 5000
- nums[i] can be negative
- k ≥ 2
- Must run faster than O(n²) ideally.
---

## 4. Edge Cases

- k divides 0 → a prefix sum being exactly divisible by k counts a subarray.
- Negative numbers → remainders can be negative → must normalize.
- Single element divisible by k.
---

## 5. Examples

```text
Example 1

Input:

6 5
4 5 0 -2 -3 1


Output: 7

Example 2

Input:

4 2
4 5 0 -2


Output: 4
```

---

## 6. Approaches

### Approach 1: Brute Force (O(n³))

**Idea:**
- Enumerate every (i, j) pair.
- Compute sum for each subarray.
- Check if divisible by k.

**Steps:**
- For each start index i
- For each end index j >= i
- Compute sum inside inner loop
- Check divisibility

**Java Code:**
```java
public int subarraysDivByK(int[] nums, int k) {
    int n = nums.length;
    int count = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int sum = 0;
            for (int t = i; t <= j; t++) {
                sum += nums[t];
            }
            if (sum % k == 0) count++;
        }
    }
    return count;
}
```

**💭 Intuition Behind the Approach:**
- Very direct: try all subarrays and compute each sum manually.
- But extremely slow.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n³)
  * Sum recomputed many times.
- 💾 Space Complexity
  * O(1)

### Approach 2: Prefix Sum Optimization (O(n²))

**Idea:**
- Instead of recomputing sums, use prefix sum:
  * sum(i…j) = prefix[j] - prefix[i-1]
- Then check if divisible by k.

**Steps:**
- Build prefix-sum array
- Enumerate (i, j) pairs
- Compute sum = prefix[j] − prefix[i-1]
- Check divisibility

**Java Code:**
```java
public int subarraysDivByK(int[] nums, int k) {
    int n = nums.length;
    int[] pref = new int[n];
    pref[0] = nums[0];

    for (int i = 1; i < n; i++) {
        pref[i] = pref[i - 1] + nums[i];
    }

    int count = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {

            int sum = pref[j] - (i > 0 ? pref[i - 1] : 0);

            if (sum % k == 0) count++;
        }
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Avoid repeated sum calculation by caching sums up to each index.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n²)
  * Why?
  * Every (i, j) pair is checked once.
- 💾 Space Complexity
  * O(n) for prefix array.

### Approach 3: Prefix Remainder Frequency (Optimal — O(n))

**Idea:**
- If two prefix sums have the same remainder mod k, the subarray between them has sum divisible by k.
- We maintain:
  * freq[remainder] = count of prefix sums with that remainder
- When a remainder repeats, new valid subarrays form.

**Steps:**
- Initialize:
  * freq[0] = 1 (handles subarrays starting at index 0)
  * sum = 0, count = 0
- For each number:
  * sum += num
  * rem = (sum % k + k) % k (handle negatives)
  * Add freq[rem] to answer (those many subarrays end here)
  * Increment freq[rem]

**Java Code:**
```java
public int subarraysDivByK(int[] nums, int k) {
    HashMap<Integer, Integer> freq = new HashMap<>();
    freq.put(0, 1);

    int sum = 0;
    int count = 0;

    for (int num : nums) {
        sum += num;

        int rem = sum % k;
        if (rem < 0) rem += k;

        if (freq.containsKey(rem)) {
            count += freq.get(rem);
        }

        freq.put(rem, freq.getOrDefault(rem, 0) + 1);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- If:
  * prefix[j] % k == prefix[i-1] % k
- Then:
  * (prefix[j] - prefix[i-1]) % k == 0
- Meaning the subarray (i…j) is divisible by k.
- We only need to count matching remainders.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Why?
    * Single pass, hash lookups are O(1).
- 💾 Space Complexity
  * O(k) or O(n) (depending on distribution of remainders).

---

## 7. Justification / Proof of Optimality

- The optimal approach is correct due to the mathematical fact:
  * (a % k == b % k)  →  (a - b) % k == 0
- Prefix sums convert subarray sum checking to matching remainders.
- Remainder frequency allows counting all valid subarrays ending at the current index in constant time.
- This reduces the problem from checking O(n²) subarrays to processing remainders in O(n).
---

## 8. Variants / Follow-Ups

- Variant 1 — Longest subarray with sum divisible by k
  * Track first occurrence of each remainder to compute max length.
- Variant 2 — Count subarrays with sum equal to k
  * Use:
  * sum - k = existing prefix sum
- Variant 3 — Count subarrays with sum divisible by k but also bounded
  * Use sliding window + mod tricks.
- Variant 4 — Subarray sums divisible by k with negative numbers
  * Same technique but requires remainder normalization.
- Variant 5 — Count subarrays divisible by multiple mod values
  * Maintain maps for each k.
---

## 9. Tips & Observations

- Normalizing remainder with:
  * rem = (sum % k + k) % k
- is extremely important with negative numbers.
- Always initialize:
  * freq[0] = 1
- to allow subarrays starting at index 0.
- Remainder frequency tricks show up in many problems (equal 0s & 1s, sums divisible by k, etc.).
- HashMap works better than array when k is large.
- If all nums are multiples of k → every subarray works.
---

## 10. Pitfalls

- Forgetting remainder normalization → WRONG answers.
- Missing initialization of freq[0].
- Using brute force on large n → will timeout.
- Treating subsequences like subarrays (order matters!).
- Using array remainder frequency when k is very large → memory issues.
---

<!-- #endregion -->
<!-- #region 179-Minimum Window Substring -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q179: Minimum Window Substring</h1>

## 1. Problem Statement

- Given two strings s and t, return the minimum window substring of s such that every character in t (including duplicates) is included in the window.
- If no such window exists, return an empty string "".
- The answer is guaranteed to be unique.
---

## 2. Problem Understanding

- You are given:
  * A source string s
  * A target string t
- Your task is to find the smallest contiguous substring of s that:
  * Contains all characters of t
  * Respects frequency (if t has 2 'A', window must also have 2 'A')
- This is not about subsequences — the window must be continuous.
---

## 3. Constraints

- 1 <= s.length, t.length <= 100000
- Characters can repeat
- Large input size → brute force will TLE
---

## 4. Edge Cases

- t longer than s → return ""
- s == t → answer is s
- Characters in t not present in s
- Duplicate characters in t
- Single-character strings
---

## 5. Examples

```text
Example 1

Input:
s = "ADOBECODEBANC"
t = "ABC"

Output:
"BANC"

Example 2

Input:
s = "a"
t = "a"

Output:
"a"
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Check all substrings of s and verify if each substring contains all characters of t with correct frequency.

**Steps:**
- Generate all substrings
- For each substring, count frequency
- Compare with frequency of t
- Track minimum valid substring
- ❌ Why Not Used
  * Substrings: O(n^2)
  * Frequency check: O(n)
  * Total: O(n^3) → TLE

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Use a sliding window with two pointers:
  * Expand the window until it becomes valid
  * Shrink it to make it minimum
- Use a HashMap to track:
  * Required character counts (t)
  * Current window counts (s)

**Steps:**
- Build frequency map for t
- Initialize two pointers left = 0, right = 0
- Expand right and include characters in window
- When all required characters are matched:
  * Try shrinking from left
  * Update minimum window
- Continue until right reaches end

**Java Code:**
```java
public static String minWindow(String s, String t) {
    if (s.length() < t.length()) return "";

    Map<Character, Integer> need = new HashMap<>();
    for (char c : t.toCharArray())
        need.put(c, need.getOrDefault(c, 0) + 1);

    int left = 0, count = 0;
    int minLen = Integer.MAX_VALUE, start = 0;
    Map<Character, Integer> window = new HashMap<>();

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);

        if (need.containsKey(c) && window.get(c) <= need.get(c))
            count++;

        while (count == t.length()) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }

            char lChar = s.charAt(left);
            window.put(lChar, window.get(lChar) - 1);
            if (need.containsKey(lChar) && window.get(lChar) < need.get(lChar))
                count--;

            left++;
        }
    }

    return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
}
```

**💭 Intuition Behind the Approach:**
- Expanding ensures all required characters are present
- Shrinking ensures the window is minimal
- count ensures we only shrink when the window is valid
- HashMaps help manage duplicates correctly

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
    * Each character enters and leaves the window once
- 💾 Space Complexity
  * O(k)
    * k = number of unique characters in t

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible due to high complexity
- Sliding window ensures linear traversal
- HashMap correctly handles duplicate character requirements
- This is the optimal and interview-accepted solution
---

## 8. Variants / Follow-Ups

- Longest substring with at most k distinct characters
- Minimum window with distinct characters
- Substring containing all vowels
- Sliding window with frequency constraints
---

## 9. Tips & Observations

- Always compare frequency, not just presence
- Use count == t.length() instead of checking maps repeatedly
- Sliding Window problems often follow expand → validate → shrink
---

## 10. Pitfalls

- ❌ Ignoring character frequency
  * Checking only presence instead of required counts (duplicates in t matter).
- ❌ Shrinking window too early
  * Shrinking before the window becomes valid leads to missing required characters.
- ❌ Incorrect validity check
  * Rechecking the whole frequency map instead of maintaining a count slows down the solution.
- ❌ Forgetting edge case t.length() > s.length()
  * Should return "" immediately.
- ❌ Off-by-one errors in window size
  * Always calculate window size as (right - left + 1).
- ❌ Not updating window count correctly while shrinking
  * Missing the condition where frequency falls below required count.
- ❌ Using == instead of <= in count logic
  * Can break handling of duplicates.
---

<!-- #endregion -->
<!-- #region 180-Distinct Window -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q180: Distinct Window</h1>

## 1. Problem Statement

- Given a string s, find the smallest substring (window) that contains all distinct characters present in the entire string s.
---

## 2. Problem Understanding

- You are given one string only.
- Steps to understand the problem:
  * First, determine how many distinct characters exist in the whole string s.
  * Then, find the smallest contiguous substring of s that contains all those distinct characters at least once.
- This is not about unique characters inside the window, but about covering all unique characters of the full string.
---

## 3. Constraints

- 1 <= |s| <= 10000
- Characters can repeat
- Need an efficient solution → brute force will TLE
---

## 4. Edge Cases

- String with all same characters → answer length = 1
- String already has all distinct characters → answer is full string
- Single character string
- Repeated patterns like "aaaaab"
---

## 5. Examples

```text
Example 1

Input:
s = "aabcbcdbca"

Output:
"dbca"

Explanation:
Distinct characters in s = {a, b, c, d}
Smallest substring covering all = "dbca"

Example 2

Input:
s = "aaab"

Output:
"ab"

Explanation:
Distinct characters = {a, b}
Smallest valid window = "ab"
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Generate all substrings
- Check if each substring contains all distinct characters of s
- Track the minimum length substring
- ❌ Why Not Used
  * Substrings → O(n^2)
  * Checking each substring → O(n)
  * Total → O(n^3) → TLE

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Count number of distinct characters in entire string → required
- Use sliding window with two pointers
- Expand window until it contains all required characters
- Shrink window to minimize length

**Steps:**
- Build a set or map to count distinct characters in s
- Initialize two pointers left = 0, right = 0
- Expand right, add characters to window map
- When window contains all distinct characters:
  * Try shrinking from left
  * Update minimum window
- Continue until end of string

**Java Code:**
```java
public static String distinctWindow(String s) {
    int n = s.length();

    Set<Character> set = new HashSet<>();
    for (char c : s.toCharArray()) set.add(c);
    int required = set.size();

    Map<Character, Integer> window = new HashMap<>();
    int left = 0, formed = 0;
    int minLen = Integer.MAX_VALUE, start = 0;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);

        if (window.get(c) == 1) formed++;

        while (formed == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }

            char lChar = s.charAt(left);
            window.put(lChar, window.get(lChar) - 1);
            if (window.get(lChar) == 0) formed--;

            left++;
        }
    }

    return s.substring(start, start + minLen);
}
```

**💭 Intuition Behind the Approach:**
- We only care whether each distinct character appears at least once
- Sliding window allows linear traversal
- Shrinking ensures minimal window size
- HashMap helps track current window state efficiently

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
    * Each character enters and exits the window once
- 💾 Space Complexity
  * O(k)
    * k = number of distinct characters in s

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible for large strings
- Sliding window ensures optimal linear time
- HashMap cleanly tracks presence and removal
- This is the standard interview-accepted solution
---

## 8. Variants / Follow-Ups

- Minimum Window Substring (with frequency constraints)
- Longest substring with all distinct characters
- At most k distinct characters
- Substring covering a given set of characters
---

## 9. Tips & Observations

- First step is always counting distinct characters of the entire string
- Presence check is enough (frequency > 1 doesn’t matter)
- Sliding window problems usually follow expand → validate → shrink
---

## 10. Pitfalls

- ❌ Confusing this with minimum window substring (frequency does NOT matter here)
- ❌ Forgetting to precompute total distinct characters
- ❌ Not shrinking window after it becomes valid
- ❌ Off-by-one error in window size calculation
- ❌ Using nested loops instead of sliding window
---

<!-- #endregion -->
<!-- #region 181-Substring With K Unique characters -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q181: Substring With K Unique characters</h1>

## 1. Problem Statement

- Given a string s and an integer k, find the length of the longest substring that contains exactly k unique characters.
- If no such substring exists, return -1.
---

## 2. Problem Understanding

- You are asked to find a contiguous substring such that:
  * It contains exactly k distinct characters
  * Among all such substrings, return the maximum possible length
- Important clarifications:
  * “Exactly k” → not less, not more
  * Order matters → substring, not subsequence
  * Only the length is required, not the substring itself
---

## 3. Constraints

- 1 <= |s| <= 100000
- 1 <= k <= 100000
- Large input → need O(n) solution
---

## 4. Edge Cases

- k > number of distinct characters in s → return -1
- k == 0 → invalid (by constraints)
- Single character string
- All characters same
- k == 1 → longest block of same characters
---

## 5. Examples

```text
Example 1

Input:

s = "aabbcc", k = 1


Output:

2


Explanation: "aa", "bb", "cc"

Example 2

Input:

s = "aabbcc", k = 2


Output:

4


Explanation: "aabb", "bbcc"
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Generate all substrings and count unique characters for each.
- ❌ Why Not Used
  * Substrings → O(n^2)
  * Counting unique → O(n)
  * Total → O(n^3) → TLE

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Use a sliding window:
  * Expand window by moving right
  * Track character frequencies in a map
  * If unique characters exceed k, shrink from left
  * If unique characters == k, update maximum length

**Steps:**
- Initialize two pointers left = 0, right = 0
- Use HashMap to track character counts
- Expand window by moving right
- If map size > k, shrink from left
- If map size == k, update answer
- Continue till end

**Java Code:**
```java
public static int longestSubstringKUnique(String s, int k) {
    int n = s.length();
    if (k > n) return -1;

    Map<Character, Integer> map = new HashMap<>();
    int left = 0, maxLen = -1;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);
        map.put(c, map.getOrDefault(c, 0) + 1);

        while (map.size() > k) {
            char lChar = s.charAt(left);
            map.put(lChar, map.get(lChar) - 1);
            if (map.get(lChar) == 0) map.remove(lChar);
            left++;
        }

        if (map.size() == k) {
            maxLen = Math.max(maxLen, right - left + 1);
        }
    }

    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- Sliding window ensures linear traversal
- HashMap size directly tells number of unique characters
- Shrinking keeps the window valid
- We only update answer when condition is exactly k

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
    * Each character enters and exits the window once
- 💾 Space Complexity
  * O(k)
    * At most k characters in the map

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible
- Sliding window is optimal and clean
- Handles large input efficiently
- Common interview pattern
---

## 8. Variants / Follow-Ups

- Longest substring with at most k distinct characters
- Longest substring with all distinct characters
- Minimum window substring
- Distinct window problems
---

## 9. Tips & Observations

- “Exactly k” → update result only when map size equals k
- Use map.size() instead of recounting
- Sliding window problems usually follow expand → validate → shrink
---

## 10. Pitfalls

- ❌ Updating answer when unique characters < k
- ❌ Forgetting to shrink when map size > k
- ❌ Returning 0 instead of -1 when no valid substring
- ❌ Confusing with “at most k” variant
- ❌ Off-by-one errors in window length calculation
---

<!-- #endregion -->
<!-- #region 182-Longest Subsequence With Difference One -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q182: Longest Subsequence With Difference One</h1>

## 1. Problem Statement

- Given an array nums of size N, find the length of the longest subsequence such that the absolute difference between every adjacent element is exactly 1.
- A subsequence does not need to be contiguous, but order must be preserved.
---

## 2. Problem Understanding

- You need to select elements from the array (keeping order) such that:
  * For every adjacent pair in the subsequence
  * abs(a[i] - a[i-1]) == 1
  * Among all such subsequences, return the maximum possible length
- Important:
  * This is a subsequence, not a subarray
  * Negative numbers are allowed
  * Only the length is required
---

## 3. Constraints

- 1 <= N <= 100000
- -10^9 <= nums[i] <= 10^9
- Large constraints → brute force will not scale.
---

## 4. Edge Cases

- Array with one element → answer is 1
- All elements same → answer is 1
- Strictly increasing by 1
- Strictly decreasing by 1
- Mix of positive and negative values
---

## 5. Examples

```text
Example 1

Input:

[4, 2, 1, 5, 6]


Output:

3


Explanation: {4, 5, 6}

Example 2

Input:

[-2, 2, -1, 1, 0, -1]


Output:

4


Explanation: {-2, -1, 0, -1} or {2, 1, 0, -1}
```

---

## 6. Approaches

### Approach 1: Brute Force (Try All Subsequences)

**Idea:**
- Generate all subsequences and check which ones satisfy:
  * abs(a[i] - a[i-1]) == 1
- Track the maximum length.

**Java Code:**
```java
public static int longestSubseqDiffOneBrute(int[] nums) {
    int n = nums.length;
    int maxLen = 1;

    for (int mask = 1; mask < (1 << n); mask++) {
        int prev = Integer.MIN_VALUE;
        int len = 0;
        boolean valid = true;

        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) != 0) {
                if (prev != Integer.MIN_VALUE &&
                    Math.abs(nums[i] - prev) != 1) {
                    valid = false;
                    break;
                }
                prev = nums[i];
                len++;
            }
        }
        if (valid) maxLen = Math.max(maxLen, len);
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(2^N * N) — every subsequence is generated and validated
- Space: O(1) — ignoring recursion/bitmask overhead
- Not feasible for large N.

### Approach 2: Dynamic Programming + HashMap (Optimal ✅)

**Idea:**
- Let dp[x] represent the length of the longest valid subsequence ending with value x.
- For each number num:
  * It can extend subsequences ending with num - 1 or num + 1
  * So:
    * dp[num] = max(dp[num-1], dp[num+1]) + 1

**Java Code:**
```java
public static int longestSubseqDiffOne(int[] nums) {
    Map<Integer, Integer> dp = new HashMap<>();
    int maxLen = 1;

    for (int num : nums) {
        int left = dp.getOrDefault(num - 1, 0);
        int right = dp.getOrDefault(num + 1, 0);

        int curr = Math.max(left, right) + 1;
        dp.put(num, curr);

        maxLen = Math.max(maxLen, curr);
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- Subsequence only depends on previous values, not positions
- We only care about numbers that differ by ±1
- HashMap allows constant-time lookup for large value ranges
- This is similar to Longest Increasing Subsequence, but value-based

**Complexity (Time & Space):**
- Time: O(N) — each element is processed once
- Space: O(N) — HashMap may store up to N unique values

---

## 7. Justification / Proof of Optimality

- Brute force is exponential and infeasible
- DP + HashMap uses optimal transitions
- Handles large value ranges and negatives
- Widely accepted interview solution
---

## 8. Variants / Follow-Ups

- Brute force is exponential and infeasible
- DP + HashMap uses optimal transitions
- Handles large value ranges and negatives
- Widely accepted interview solution
---

## 9. Tips & Observations

- This is a value-based DP, not index-based
- HashMap is essential due to large number constraints
- Order is preserved automatically by iteration
---

## 10. Pitfalls

- Treating this as a subarray problem
- Forgetting absolute difference (both +1 and -1)
- Using array DP instead of HashMap (value range too large)
- Trying to sort the array (breaks subsequence order)
---

<!-- #endregion -->
<!-- #region 183-Longest Substring with Unique Characters -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q183: Longest Substring with Unique Characters</h1>

## 1. Problem Statement

- Given a string s, find the length of the longest substring such that each character appears at most once.
- A substring must be contiguous.
---

## 2. Problem Understanding

- You need to find the longest contiguous part of the string where:
- No character repeats
- Order matters
- Only the length is required, not the substring itself
- This is a classic sliding window problem.
---

## 3. Constraints

- 0 <= s.length() <= 10^4
- String can contain alphabets, digits, spaces, and symbols
---

## 4. Edge Cases

- Empty string → answer 0
- String with all same characters → answer 1
- String with all unique characters → answer is full length
- String containing spaces or symbols
- Mixed characters
---

## 5. Examples

```text
Example 1

Input:

xyzxyzyy


Output:

3


Explanation: "xyz"

Example 2

Input:

xxxxxx


Output:

1
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Substrings)

**Idea:**
- Generate all possible substrings and check if all characters in each substring are unique.

**Java Code:**
```java
public static int longestSubstringBrute(String s) {
    int n = s.length();
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            boolean[] seen = new boolean[256];
            boolean valid = true;

            for (int k = i; k <= j; k++) {
                char c = s.charAt(k);
                if (seen[c]) {
                    valid = false;
                    break;
                }
                seen[c] = true;
            }

            if (valid) {
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(n^3) — generating all substrings and checking uniqueness for each
- Space: O(1) — fixed-size character array (constant 256)

### Approach 2: Sliding Window + HashSet (Optimal ✅)

**Idea:**
- Use a sliding window:
  * Expand right pointer and add characters to a set
  * If a duplicate appears, move left until the duplicate is removed
  * Track maximum window size

**Java Code:**
```java
public static int longestSubstring(String s) {
    int n = s.length();
    Set<Character> set = new HashSet<>();

    int left = 0, maxLen = 0;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);

        while (set.contains(c)) {
            set.remove(s.charAt(left));
            left++;
        }

        set.add(c);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- A substring with unique characters must not contain duplicates
- Sliding window allows us to expand and shrink efficiently
- HashSet provides fast lookup and removal
- Each character is added and removed at most once

**Complexity (Time & Space):**
- Time: O(n) — each character enters and leaves the window once
- Space: O(n) — in worst case, all characters are unique and stored in the set

---

## 7. Justification / Proof of Optimality

- Brute force is inefficient for large strings
- Sliding window achieves linear time
- HashSet ensures uniqueness efficiently
- This is a standard interview solution
---

## 8. Variants / Follow-Ups

- Longest substring with at most k distinct characters
- Longest substring with exactly k distinct characters
- Minimum window substring
- Distinct window problems
---

## 9. Tips & Observations

- Sliding window is the key pattern here
- Use HashSet when frequency does not matter
- For index tracking, HashMap with last seen index can also be used
---

## 10. Pitfalls

- Treating this as a subsequence problem
- Forgetting to remove characters while shrinking the window
- Resetting the window completely instead of sliding
- Using nested loops unnecessarily
---

<!-- #endregion -->
<!-- #region 184-Maximum Ones After Modification -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q184: Maximum Ones After Modification</h1>

## 1. Problem Statement

- Given a binary array A and an integer B, find the length of the longest contiguous subsegment of 1s possible by changing at most B zeros to ones.
---

## 2. Problem Understanding

- You are allowed to flip at most B zeros in the array to 1.
- After these changes, you must find the longest contiguous subarray consisting of only 1s.
- Important clarifications:
  * Subsegment = contiguous subarray
  * You don’t need to actually modify the array, only compute the maximum length
  * If B is large, you may flip many zeros
- This is a sliding window with constraint problem.
---

## 3. Constraints

- 1 <= N <= 100000
- A[i] is either 0 or 1
- 1 <= B <= 100000
---

## 4. Edge Cases

- Array contains all 1s
- Array contains all 0s
- B >= number of zeros in array
- Single element array
- B = 0 (no flips allowed)
---

## 5. Examples

```text
Example 1

Input:

A = [1, 0, 0, 1, 1, 0, 1]
B = 1


Output:

4

Example 2

Input:

A = [1, 0, 0, 1, 0, 1, 0, 1, 0, 1]
B = 2


Output:

5
```

---

## 6. Approaches

### Approach 1: Brute Force (Try All Subarrays)

**Idea:**
- Check every subarray and count how many zeros it contains.
- If zeros ≤ B, update the maximum length.

**Java Code:**
```java
public static int maxOnesBrute(int[] A, int B) {
    int n = A.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int zeroCount = 0;
        for (int j = i; j < n; j++) {
            if (A[j] == 0) zeroCount++;
            if (zeroCount > B) break;
            maxLen = Math.max(maxLen, j - i + 1);
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(n^2) — checking all subarrays
- Space: O(1) — no extra data structure
- Not feasible for large N.

### Approach 2: Sliding Window (Optimal ✅)

**Idea:**
- Maintain a window with at most B zeros:
  * Expand right
  * Count zeros in current window
  * If zeros exceed B, shrink from left
  * Track maximum window length

**Java Code:**
```java
public static int maxOnes(int[] A, int B) {
    int left = 0, zeroCount = 0, maxLen = 0;

    for (int right = 0; right < A.length; right++) {
        if (A[right] == 0) zeroCount++;

        while (zeroCount > B) {
            if (A[left] == 0) zeroCount--;
            left++;
        }

        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- A valid window can contain at most B zeros
- Sliding window ensures linear traversal
- Shrinking restores validity when constraint breaks
- This pattern is common in “flip at most K” problems

**Complexity (Time & Space):**
- Time: O(n) — each element enters and exits the window once
- Space: O(1) — only counters are used

---

## 7. Justification / Proof of Optimality

- Brute force does not scale
- Sliding window efficiently enforces the zero constraint
- Handles large inputs and edge cases cleanly
- Standard interview solution
---

## 8. Variants / Follow-Ups

- Max consecutive ones with at most K flips
- Longest subarray with at most K zeros
- Binary array sliding window problems
- Replace at most K characters
---

## 9. Tips & Observations

- Count violations (zeros) instead of valid elements (ones)
- Sliding window with constraint is the key idea
- This is identical to “Longest subarray with at most K bad elements”
---

## 10. Pitfalls

- Treating this as a subsequence problem
- Forgetting to shrink when zero count exceeds B
- Resetting window instead of sliding
- Off-by-one errors in window length
---

<!-- #endregion -->
<!-- #region 185-Distinct Character Substring -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q185: Distinct Character Substring</h1>

## 1. Problem Statement

- Given a string s, find the number of substrings (not necessarily distinct) such that all characters inside the substring are distinct.
- A substring must be contiguous.
---

## 2. Problem Understanding

- You need to count all possible substrings where:
  * No character repeats inside the substring
  * Order matters
  * Substrings with same content but different positions are counted separately
- This is a counting problem, not a max/min length problem.
---

## 3. Constraints

- 1 <= |s| <= 100000
- Large input → brute force will not work
---

## 4. Edge Cases

- String with all same characters
- String with all unique characters
- Single character string
- Repeated patterns like "ababab"
---

## 5. Examples

```text
Example 1

Input:

gffg


Output:

6


Valid substrings:

"g", "gf", "f", "f", "fg", "g"

Example 2

Input:

acciojob


Output:

18
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Substrings)

**Idea:**
- Generate all substrings and check if each substring contains only distinct characters.

**Java Code:**
```java
public static int countDistinctSubstringBrute(String s) {
    int n = s.length();
    int count = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            boolean[] seen = new boolean[256];
            boolean valid = true;

            for (int k = i; k <= j; k++) {
                char c = s.charAt(k);
                if (seen[c]) {
                    valid = false;
                    break;
                }
                seen[c] = true;
            }

            if (valid) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(n^3) — generating all substrings and validating each
- Space: O(1) — fixed-size frequency array
- Not feasible for large input.

### Approach 2: Sliding Window (Optimal ✅)

**Idea:**
- Use a sliding window to maintain a window of unique characters:
  * Expand right
  * If a duplicate appears, shrink from left until window becomes valid
  * At each position right, all substrings ending at right and starting between left and right are valid
- Number of valid substrings added at each step:
  * right - left + 1

**Java Code:**
```java
public static int countDistinctSubstring(String s) {
    int n = s.length();
    Set<Character> set = new HashSet<>();

    int left = 0;
    long count = 0;

    for (int right = 0; right < n; right++) {
        char c = s.charAt(right);

        while (set.contains(c)) {
            set.remove(s.charAt(left));
            left++;
        }

        set.add(c);
        count += (right - left + 1);
    }
    return (int) count;
}
```

**💭 Intuition Behind the Approach:**
- A window with unique characters guarantees all its substrings are valid
- For each right, every substring starting from left to right is distinct
- Sliding window ensures no rechecking of characters
- This converts a nested problem into linear traversal

**Complexity (Time & Space):**
- Time: O(n) — each character is added and removed once
- Space: O(n) — set stores unique characters in current window

---

## 7. Justification / Proof of Optimality

- Brute force is infeasible due to cubic complexity
- Sliding window avoids redundant checks
- Counting substrings using window length is the key optimization
- This is a standard counting pattern in sliding window problems
---

## 8. Variants / Follow-Ups

- Longest substring with unique characters
- Count substrings with at most k distinct characters
- Count substrings with exactly k distinct characters
- Distinct window problems
---

## 9. Tips & Observations

- This problem is about counting, not max length
- Use right - left + 1 carefully
- Sliding window is reusable across many substring problems
---

## 10. Pitfalls

- Treating this as a subsequence problem
- Forgetting to shrink window when duplicate appears
- Using nested loops instead of sliding window
- Missing the counting logic for each right
---

<!-- #endregion -->
<!-- #region 186-Smallest Substring with all Characters -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q186: Smallest Substring with all Characters</h1>

## 1. Problem Statement

- Given two strings a and b, find the smallest substring of a that contains all characters of b (including duplicates).
- Rules:
  * If multiple substrings have the same minimum length, return the one with the least starting index
  * If no such substring exists, return "-1"
---

## 2. Problem Understanding

- You need to find a contiguous substring of string a such that:
  * Every character present in b is present in the substring
  * Character frequency matters (if b has two 'o', substring must have two 'o')
  * Among all valid substrings, choose the smallest length
  * If tie → choose earliest starting index
- This is a minimum window substring problem.
---

## 3. Constraints

- 1 <= a.length(), b.length() <= 1000
- Characters may repeat
- Efficient solution required
---

## 4. Edge Cases

- b.length() > a.length() → return "-1"
- a == b
- Characters in b not present in a
- Duplicate characters in b
- Multiple valid windows with same size
---

## 5. Examples

```text
Example 1

Input:

a = "acciojob"
b = "coo"


Output:

"ciojo"

Example 2

Input:

a = "timetopractice"
b = "cot"


Output:

"toprac"
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Substrings)

**Idea:**
- Generate all substrings of a and check whether each contains all characters of b with correct frequency.

**Java Code:**
```java
public static String smallestSubstringBrute(String a, String b) {
    int n = a.length();
    int minLen = Integer.MAX_VALUE;
    String ans = "-1";

    Map<Character, Integer> need = new HashMap<>();
    for (char c : b.toCharArray())
        need.put(c, need.getOrDefault(c, 0) + 1);

    for (int i = 0; i < n; i++) {
        Map<Character, Integer> freq = new HashMap<>();
        for (int j = i; j < n; j++) {
            char c = a.charAt(j);
            freq.put(c, freq.getOrDefault(c, 0) + 1);

            boolean valid = true;
            for (char key : need.keySet()) {
                if (freq.getOrDefault(key, 0) < need.get(key)) {
                    valid = false;
                    break;
                }
            }

            if (valid && (j - i + 1) < minLen) {
                minLen = j - i + 1;
                ans = a.substring(i, j + 1);
            }
        }
    }
    return ans;
}
```

**Complexity (Time & Space):**
- Time: O(n^3) — generating substrings and validating frequency each time
- Space: O(n) — frequency maps
- Not suitable for large inputs.

### Approach 2: Sliding Window + HashMap (Optimal ✅)

**Idea:**
- Use sliding window:
  * Expand right to include characters
  * Track frequency of required characters
  * When window becomes valid, shrink from left to minimize length
  * Track minimum window and starting index

**Java Code:**
```java
public static String SmallestSubstring(String a, String b) {
    if (b.length() > a.length()) return "-1";

    Map<Character, Integer> need = new HashMap<>();
    for (char c : b.toCharArray())
        need.put(c, need.getOrDefault(c, 0) + 1);

    Map<Character, Integer> window = new HashMap<>();
    int left = 0, formed = 0;
    int minLen = Integer.MAX_VALUE, start = 0;
    int required = b.length();

    for (int right = 0; right < a.length(); right++) {
        char c = a.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);

        if (need.containsKey(c) && window.get(c) <= need.get(c))
            formed++;

        while (formed == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }

            char lChar = a.charAt(left);
            window.put(lChar, window.get(lChar) - 1);

            if (need.containsKey(lChar) && window.get(lChar) < need.get(lChar))
                formed--;

            left++;
        }
    }

    return minLen == Integer.MAX_VALUE ? "-1"
            : a.substring(start, start + minLen);
}
```

**💭 Intuition Behind the Approach:**
- We expand until all required characters are present
- Shrinking ensures minimal window
- formed ensures frequency correctness
- Sliding window avoids recomputation

**Complexity (Time & Space):**
- Time: O(n) — each character enters and exits window once
- Space: O(n) — frequency maps

---

## 7. Justification / Proof of Optimality

- Brute force is too slow
- Sliding window ensures optimal time
- HashMap correctly handles duplicate characters
- Tie-breaking handled by earliest index naturally
---

## 8. Variants / Follow-Ups

- Minimum Window Substring
- Distinct window problems
- Substring with exactly k distinct characters
- Longest substring with frequency constraints
---

## 9. Tips & Observations

- Always use frequency comparison, not just presence
- Modifying formed correctly is crucial
- Sliding window problems follow expand → validate → shrink
---

## 10. Pitfalls

- Ignoring character frequency
- Shrinking window before it becomes valid
- Not handling duplicate characters in b
- Returning any window instead of smallest
- Forgetting "-1" case
---

<!-- #endregion -->
<!-- #region 187-Longest subarray with equal frequency -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q187: Longest subarray with equal frequency</h1>

## 1. Problem Statement

- You are given an array containing only 0, 1, and 2.
- Find the length of the longest contiguous subarray such that the frequency of 0, 1, and 2 is equal.
---

## 2. Problem Understanding

- You need to find a subarray (contiguous) where:
  * Count of 0 = count of 1 = count of 2
  * Order matters
  * Only the length of such subarray is required
- This is not a sliding window problem because the condition is not monotonic.
- It is a prefix-difference + hashmap problem.
---

## 3. Constraints

- 1 <= n <= 100000
- Array contains only 0, 1, 2
---

## 4. Edge Cases

- No valid subarray → return 0
- Entire array is valid
- Array with only one or two distinct values
- Subarray starting from index 0
---

## 5. Examples

```text
Example 1

Input:

[0, 1, 0, 2, 0, 1, 0]


Output:

3

Example 2

Input:

[0, 1, 1, 2, 0, 2]


Output:

6
```

---

## 6. Approaches

### Approach 1: Brute Force (Check All Subarrays)

**Idea:**
- Generate all subarrays and count frequency of 0, 1, and 2.
- If all three counts are equal, update the maximum length.

**Java Code:**
```java
public static int longestEqual012Brute(int[] arr) {
    int n = arr.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int c0 = 0, c1 = 0, c2 = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 0) c0++;
            else if (arr[j] == 1) c1++;
            else c2++;

            if (c0 == c1 && c1 == c2) {
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }
    }
    return maxLen;
}
```

**Complexity (Time & Space):**
- Time: O(n^2) — checking all subarrays
- Space: O(1) — only counters used
- Not feasible for large n.

### Approach 2: Prefix Difference + HashMap (Optimal ✅)

**Idea:**
- Instead of tracking three counts, track relative differences:
  * Let
    * d1 = count1 - count0
    * d2 = count2 - count1
- If the same (d1, d2) pair occurs again at index j, it means the subarray between previous index and j has equal 0s, 1s, and 2s.

**Steps:**
- Maintain running counts of 0, 1, 2
- Compute (d1, d2) at each index
- Store the first occurrence of each (d1, d2) in a HashMap
- When the same pair repeats, compute subarray length

**Java Code:**
```java
public static int longestEqual012(int[] arr) {
    int c0 = 0, c1 = 0, c2 = 0;
    int maxLen = 0;

    Map<String, Integer> map = new HashMap<>();
    map.put("0#0", -1); // base case

    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == 0) c0++;
        else if (arr[i] == 1) c1++;
        else c2++;

        int d1 = c1 - c0;
        int d2 = c2 - c1;

        String key = d1 + "#" + d2;

        if (map.containsKey(key)) {
            maxLen = Math.max(maxLen, i - map.get(key));
        } else {
            map.put(key, i);
        }
    }
    return maxLen;
}
```

**💭 Intuition Behind the Approach:**
- Equal counts mean relative differences cancel out
- Prefix differences convert a 3-variable condition into a hashmap lookup
- Same prefix state appearing again implies balanced subarray in between
- This technique generalizes to similar frequency-equality problems

**Complexity (Time & Space):**
- Time: O(n) — single traversal
- Space: O(n) — hashmap stores prefix states

---

## 7. Justification / Proof of Optimality

- Brute force is too slow for large inputs
- Prefix difference reduces the problem to hashmap lookup
- HashMap ensures earliest index is preserved for maximum length
- This is a classic and optimal interview approach
---

## 8. Variants / Follow-Ups

- Longest subarray with equal 0s and 1s
- Longest subarray with equal frequency of k values
- Prefix sum + hashmap problems
- Balanced subarray problems
---

## 9. Tips & Observations

- Always store the first occurrence of a prefix state
- Use a composite key for multiple differences
- Base case is crucial (0#0 at index -1)
---

## 10. Pitfalls

- Forgetting base case in hashmap
- Using absolute counts instead of differences
- Overwriting first occurrence in hashmap
- Treating this as sliding window (it is not)
---

<!-- #endregion -->
<!-- #region 188-Rabbits in Forest -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q188: Rabbits in Forest</h1>

## 1. Problem Statement

- A certain number of rabbits live in a forest.
- When asking the i-th rabbit how many rabbits have the same color as itself, it replies arr[i], excluding itself.
- You are given the array arr.
- Find the minimum possible total number of rabbits in the forest that satisfies all answers.
---

## 2. Problem Understanding

- Each rabbit tells how many other rabbits of the same color exist.
- If a rabbit says x, then its color group size must be x + 1 (including itself).
- Multiple rabbits can belong to the same color group only if their answers match.
- If more rabbits claim a color group than allowed, extra rabbits must belong to new groups.
- We must minimize the total rabbits, including those not present in the array.
- This is a grouping + counting problem, not simulation.
---

## 3. Constraints

- 1 <= n <= 1000
- 0 <= arr[i] <= 1000
---

## 4. Edge Cases

- arr[i] = 0 → rabbit is alone in its color
- All elements same
- All elements distinct
- Large frequency of same number exceeding group size
---

## 5. Examples

```text
🧪 Examples

Example 1

Input:
3
1 1 2

Output:
5


Example 2

Input:
3
10 10 10

Output:
11
```

---

## 6. Approaches

### Approach 1: Brute Force Group Simulation (Conceptual)

**Idea:**
- For each rabbit, try to form a group of size arr[i] + 1.
- Track how many rabbits have already used that group.
- Create a new group whenever capacity is exceeded.

**Steps:**
- Iterate rabbits one by one
- Try to assign to existing matching group
- Else create new group

**Java Code:**
```java
public int minimumRabbits(int[] arr) {
    Map<Integer, Integer> used = new HashMap<>();
    int total = 0;

    for (int x : arr) {
        int size = x + 1;
        int count = used.getOrDefault(x, 0);

        if (count == 0) {
            total += size;
            used.put(x, 1);
        } else if (count < size) {
            used.put(x, count + 1);
        } else {
            total += size;
            used.put(x, 1);
        }
    }
    return total;
}
```

**💭 Intuition Behind the Approach:**
- Each answer defines a fixed group size
- Once a group is full, you must start a new one
- We greedily fill groups to minimize total count

**Complexity (Time & Space):**
- Time: O(N^2) — checking group availability repeatedly
- Space: O(N) — tracking group usage

### Approach 2: HashMap Frequency (Better)

**Idea:**
- Count how many times each number appears
- For value x:
  * Each group can hold x + 1 rabbits
  * If frequency is f, number of groups needed = ceil(f / (x + 1))

**Steps:**
- Count frequencies using HashMap
- For each entry:
  * Compute number of groups
  * Add groups * (x + 1) to answer

**Java Code:**
```java
public int minimumRabbits(int[] arr) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int x : arr) {
        freq.put(x, freq.getOrDefault(x, 0) + 1);
    }

    int total = 0;
    for (int x : freq.keySet()) {
        int f = freq.get(x);
        int size = x + 1;
        int groups = (f + size - 1) / size;
        total += groups * size;
    }
    return total;
}
```

**💭 Intuition Behind the Approach:**
- Same answers → same color group size
- We batch rabbits efficiently
- Ceiling ensures leftover rabbits still form a valid group

**Complexity (Time & Space):**
- Time: O(N) — one pass for frequency, one pass for calculation
- Space: O(N) — hashmap

### Approach 3: Optimized Greedy (Optimal)

**Idea:**
- Identical to Approach 2 but viewed as color buckets
- Always assume minimum valid configuration

**Java Code:**
```java
public int minimumRabbits(int[] arr) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int x : arr) {
        freq.put(x, freq.getOrDefault(x, 0) + 1);
    }

    int result = 0;
    for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
        int x = entry.getKey();
        int f = entry.getValue();
        int groupSize = x + 1;
        result += ((f + groupSize - 1) / groupSize) * groupSize;
    }
    return result;
}
```

**💭 Intuition Behind the Approach:**
- Each rabbit forces a minimum group size
- We only create as many groups as necessary
- This guarantees minimum rabbits overall

**Complexity (Time & Space):**
- Time: O(N) — optimal
- Space: O(N) — hashmap only

---

## 7. Justification / Proof of Optimality

- Rabbits with the same answer must form groups of size x + 1
- Overfilling is invalid → new group required
- Underfilling still requires full group existence
- Greedy grouping ensures minimum total rabbits
---

## 8. Variants / Follow-Ups

- Similar to LeetCode: Rabbits in Forest
- Can be extended to:
  * Colors with unknown IDs
  * Animals reporting inclusive count
  * Maximum rabbits instead of minimum
---

## 9. Tips & Observations

- Always think in groups, not individuals
- arr[i] + 1 is the key insight
- Ceiling division is crucial
- Zero answer → single rabbit group
---

## 10. Pitfalls

- Forgetting to add missing rabbits
- Using f / (x + 1) instead of ceiling
- Treating each rabbit independently
---

<!-- #endregion -->
<!-- #region 189-LRU Cache -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q189: LRU Cache</h1>

## 1. Problem Statement

- Design a data structure that follows the constraints of a Least Recently Used (LRU) Cache.
- You must implement the following operations:
- LRUCache(int capacity) → initialize cache with given capacity
  * int get(int key) → return value if present, else -1
  * void set(int key, int value) → insert/update key-value pair
  * If cache exceeds capacity, evict the least recently used key
- ⚠️ Both get and set must run in O(1) average time complexity.
---

## 2. Problem Understanding

- LRU policy means:
  * The most recently accessed key stays
  * The least recently accessed key is removed first when capacity is full
- Any GET or SET makes that key most recently used
- We must support:
  * Fast lookup by key → O(1)
  * Fast deletion & insertion based on usage order → O(1)
- This immediately rules out arrays or plain linked lists.
---

## 3. Constraints

- 1 <= capacity <= 1000
- 1 <= Q <= 10000
- 1 <= key, value <= 1000
---

## 4. Edge Cases

- GET on non-existing key
- SET on existing key (update + move to MRU)
- Capacity = 1
- Repeated updates to same key
---

## 5. Examples

```text
Example 1

Input:
2
2
SET 1 2
GET 1

Output:
2


Example 2

Input:
2
8
SET 1 2
SET 2 3
SET 1 5
SET 4 5
SET 6 7
GET 4
SET 1 2
GET 3

Output:
5 -1
```

---

## 6. Approaches

### Approach 1: Array / List Simulation (Brute Force)

**Idea:**
- Store cache in list
- Move accessed element to end
- Remove from front when full
- Why it Fails
  * Searching → O(N)
  * Deleting middle → O(N)
  * Violates O(1) requirement

**Java Code:**
```java
// Conceptual only – NOT O(1)
```

**💭 Intuition Behind the Approach:**
- Direct simulation of LRU logic
- Simple but inefficient

**Complexity (Time & Space):**
- Time: O(N) — shifting elements
- Space: O(N)

### Approach 2: HashMap + Doubly Linked List (Optimal)

**Idea:**
- Use two data structures:
  * HashMap
    * Key → Node (for O(1) access)
  * Doubly Linked List
    * Maintains usage order
    * Head → Least Recently Used
    * Tail → Most Recently Used
- Why DLL?
  * O(1) deletion
  * O(1) insertion
  * Easy movement on access

**Steps:**
- On GET(key):
  * If key not found → -1
  * Else:
    * Move node to tail (MRU)
    * Return value
- On SET(key, value):
  * If key exists:
    * Update value
    * Move node to tail
  * Else:
    * If cache full:
      * Remove head (LRU)
    * Insert new node at tail

**Java Code:**
```java
class LRUCache {

    class Node {
        int key, value;
        Node prev, next;
        Node(int k, int v) {
            key = k;
            value = v;
        }
    }

    private int capacity;
    private Map<Integer, Node> map;
    private Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();

        head = new Node(0, 0); // dummy head
        tail = new Node(0, 0); // dummy tail
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;

        Node node = map.get(key);
        remove(node);
        insertAtTail(node);
        return node.value;
    }

    public void set(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            remove(node);
            insertAtTail(node);
        } else {
            if (map.size() == capacity) {
                Node lru = head.next;
                remove(lru);
                map.remove(lru.key);
            }
            Node newNode = new Node(key, value);
            insertAtTail(newNode);
            map.put(key, newNode);
        }
    }

    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void insertAtTail(Node node) {
        node.prev = tail.prev;
        node.next = tail;
        tail.prev.next = node;
        tail.prev = node;
    }
}
```

**💭 Intuition Behind the Approach:**
- HashMap gives instant access
- Doubly Linked List tracks usage order
- Moving nodes avoids re-creation
- Dummy head & tail simplify edge cases
- This perfectly matches LRU behavior

**Complexity (Time & Space):**
- Time: O(1) — hashmap lookup + constant pointer changes
- Space: O(N) — hashmap + linked list

---

## 7. Justification / Proof of Optimality

- LRU requires both fast lookup and fast eviction
- HashMap alone can’t track order
- LinkedList alone can’t give O(1) access
- Combination gives best of both worlds
- This is the only correct O(1) solution
---

## 8. Variants / Follow-Ups

- LFU Cache
- LRU with expiry time
- Thread-safe LRU
- LRU using LinkedHashMap (Java built-in)
---

## 9. Tips & Observations

- Always use dummy nodes in DLL
- Update usage on both GET and SET
- Evict from head, insert at tail
- Never traverse the list
---

## 10. Pitfalls

- Forgetting to move node on GET
- Removing wrong end of list
- Not updating hashmap on eviction
- Missing edge case when capacity = 1
---

<!-- #endregion -->
<!-- #region 190-Snapshot Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q190: Snapshot Array</h1>

## 1. Problem Statement

- You must design a data structure SnapshotArray supporting:
  * SnapshotArray(int length)
    * Initialize array of given size, all values = 0.
  * void set(int index, int val)
    * Set arr[index] = val.
  * int snap()
    * Take a snapshot, return a snap_id (snap count − 1).
  * int get(int index, int snap_id)
    * Return value at that index as of that snapshot.
- You will be given operations and must output results for snap() and get().
---

## 2. Problem Understanding

- We need a persistent array structure where:
  * Many updates happen (set)
  * After each snapshot, we must be able to retrieve old values
  * Up to 50k operations, so storing full copies per snap (length up to 50k) is too slow (50k * 50k = 2.5B → impossible)
- We must store only the changes per index.
- Key idea:
  * Maintain, for each index, a list of (snap_id, value) pairs.
  * On set, update the latest entry.
  * On snap, increment snap counter.
  * On get, binary search the version ≤ snap_id.
- This gives efficient storage and fast queries.
---

## 3. Constraints

- 1 ≤ length ≤ 50k
- Total operations ≤ 50k
- Values ≤ 1e9
- Snapshots may be large in count
- Must support fast get → binary search required
---

## 4. Edge Cases

- Multiple set() calls before a snap() → only last one matters
- Querying snap_id for an index that was never set → should return 0
- Setting the same value multiple times should not break logic
- Sparse updates → most indices unchanged
---

## 5. Examples

```text
Example 1:

set 0 6
snap → returns 0
set 0 3
get 0 0 → 6


Example 2:

set 1 4
set 3 34
snap → 0
get 3 0 → 34
set 3 5
snap → 1
get 3 1 → 5
```

---

## 6. Approaches

### Approach 1: Brute Force Snapshots (Invalid for Constraints)

**Idea:**
- Store a full copy of the array on every snap.

**Complexity (Time & Space):**
- Time: O(n) per snap → bad
- Space: O(n * snaps) → impossible

### Approach 2: Map of Snapshots Per Index (Optimal)

**Idea:**
- For each index, maintain:
  * index → list of (snap_id, value)
- Process:
  * set(i, v) → append or update the latest entry for current snap.
  * snap() → increment snap counter, return previous.
  * get(i, snap_id) → binary search list of versions.

**Steps:**
- Maintain snap_id = 0
- For each index, store a list of (snap, value) pairs.
- On set, if last snap matches current snap → overwrite; else append new version.
- On snap, return snap_id++.
- On get, binary search to find largest snap <= snap_id

**Java Code:**
```java
class SnapshotArray {
    
    List<int[]>[] history;
    int snapId = 0;

    public SnapshotArray(int length) {
        history = new ArrayList[length];
        for (int i = 0; i < length; i++) {
            history[i] = new ArrayList<>();
            history[i].add(new int[]{0, 0}); // at snap 0, value = 0
        }
    }

    public void set(int index, int val) {
        List<int[]> list = history[index];
        int[] last = list.get(list.size() - 1);

        if (last[0] == snapId) {
            last[1] = val; // same snap — update last value
        } else {
            list.add(new int[]{snapId, val});
        }
    }

    public int snap() {
        return snapId++;
    }

    public int get(int index, int snap_id) {
        List<int[]> list = history[index];

        int l = 0, r = list.size() - 1;
        int ans = 0;

        while (l <= r) {
            int mid = (l + r) / 2;
            if (list.get(mid)[0] <= snap_id) {
                ans = list.get(mid)[1];
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }

        return ans;
    }
}
```

**💭 Intuition Behind the Approach:**
- Snapshots do NOT require copying the array.
- We only store changed values.
- Binary search allows retrieval in logarithmic time.
- This is classic persistent array design using versioning.

**Complexity (Time & Space):**
- Time:
  * set → O(1)
  * snap → O(1)
  * get → O(log M) where M = number of changes at index
- Space:
  * O(total number of set operations)
- Optimal for given constraints.

---

## 7. Justification / Proof of Optimality

- Storing full arrays is impossible.
- Tracking only changes is efficient and avoids duplication.
- Binary search ensures fast retrieval.
- This is the standard official solution used in competitive programming and interviews.
---

## 8. Variants / Follow-Ups

- Snapshot matrix
- Fully persistent arrays
- Time-travel data structures
- Immutable arrays with versioning
---

## 9. Tips & Observations

- Only store entries when value changes.
- Early snapshots share 0-values initially.
- Always binary search by snap_id.
- List of snapshot changes stays sorted automatically.
---

## 10. Pitfalls

- Overwriting value incorrectly if same snap occurs multiple set() calls
- Forgetting to initialize with (0,0)
- Using HashMap instead of List → slower
---

<!-- #endregion -->
<!-- #region 191-Avoid Flood in The City -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q191: Avoid Flood in The City</h1>

## 1. Problem Statement

- You are given an array rains:
  * If rains[i] > 0, it rains on lake rains[i], and that lake becomes full.
  * If rains[i] == 0, it is a dry day, and you may choose any lake to dry.
- Your task:
- Return an array ans of same length:
  * If it rains at day i, ans[i] = -1
  * If dry day (0), ans[i] = lake_to_dry
- Goal: avoid any lake from being rained on while already full.
- If it is impossible → return an empty array.
- If multiple valid outputs exist → return any
---

## 2. Problem Understanding

- This is a scheduling / greedy problem.
- Key observation:
  * If lake x gets rain on day i, and it rains on x again on some future day j, we must dry lake x at some dry day between i and j.
  * We must use dry days wisely.
  * Whenever we see a repeated rain on lake x, we must find a dry day > lastRain[x] and assign that dry day to drying x.
- Thus:
  * Track last rain day of each lake.
  * Maintain a set of all available dry days (indices where rains[i] = 0).
  * For each lake that rains again, find the earliest dry day that is after the last rain day.
  * If such a day does not exist → flood → return empty array.
- Data structures:
  * lastRain = HashMap<lake, day>
  * TreeSet<Integer> dryDays for dry day positions
  * ans[] for output
---

## 3. Constraints

- 1 ≤ n ≤ 100000
- 0 ≤ rains[i] ≤ 1e9
- Greedy + TreeSet → required
---

## 4. Edge Cases

- No dry days at all → must not repeat any lake
- Many repeated rains → must match dry day order perfectly
- If lake never rains twice → no need to dry it
- Drying an empty lake does not harm — can output any dummy lake number
---

## 5. Examples

```text
Example 1
Input

4
5 2 3 4
Output

-1 -1 -1 -1
Explanation

After the first day full lakes are [5]
After the second day full lakes are [5,2]
After the third day full lakes are [5,2,3]
After the fourth day full lakes are [5,2,3,4]
There's no day to dry any lake and there is no flood in any lake.
Example 2
Input

6
2 1 0 0 1 2
Output

-1 -1 1 2 -1 -1
Explanation  : 
After the first day full lakes are [2]
After the second day full lakes are [1,2]
After the third day, we dry lake 1. Full lakes are [2]
After the fourth day, we dry lake 2. There is no full lakes.
After the fifth day, full lakes are [1].
After the sixth day, full lakes are [1,2].
It is easy that this scenario is flood-free. [-1,-1,2,1,-1,-1] is another acceptable scenario.
```

---

## 6. Approaches

### Approach 1: Brute Force (Impossible)

**Idea:**
- On each repeated rain, scan all previous dry days to pick one.
- Why impossible?
  * Worst case O(n²).

### Approach 2: Greedy + TreeSet (Optimal)

**Idea:**
- Maintain:
  * A map lastRain[lake] = day
  * A TreeSet of dry days (indices where rains[i] == 0)
  * When lake x rains at day i:
    * If this lake already rained before on day p
    * We must pick a dry day d such that d > p
    * Use TreeSet.ceiling(p+1) to find the earliest suitable dry day
    * Assign that dry day to drying this lake
    * Remove that day from TreeSet
  * Mark ans[i] = -1 on rain days

**Steps:**
- Iterate through array:
  * If rains[i] > 0:
    * If lake seen before → must find a drying day after lastRain
    * If none exists → return empty
    * Else assign dry day
    * Update lastRain
  * If rains[i] == 0:
    * Add day index to TreeSet
    * Temporarily set ans[i] = 1 (or any placeholder)

**Java Code:**
```java
public int[] avoidFlood(int[] rains) {
    int n = rains.length;
    int[] ans = new int[n];

    Map<Integer, Integer> lastRain = new HashMap<>();
    TreeSet<Integer> dryDays = new TreeSet<>();

    for (int i = 0; i < n; i++) {

        if (rains[i] == 0) {
            dryDays.add(i);
            ans[i] = 1;  // placeholder; will be overwritten if needed
        } 
        else {
            int lake = rains[i];
            ans[i] = -1;

            if (lastRain.containsKey(lake)) {
                int prev = lastRain.get(lake);
                Integer dry = dryDays.ceiling(prev + 1);

                if (dry == null) {
                    return new int[0];
                }

                ans[dry] = lake;
                dryDays.remove(dry);
            }

            lastRain.put(lake, i);
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- The only dry days that matter are the ones occurring after a lake was last filled.
- We must pick the earliest dry day possible, or we block future lakes from being saved.
- TreeSet enforces "earliest valid dry day" greedily.
- This is the same logic behind interval scheduling and avoiding conflicts.

**Complexity (Time & Space):**
- Time: O(n log n)
  * Because TreeSet operations (ceiling, remove) are log n
- Space: O(n)
  * For lastRain map, answer array, and dryDays set

---

## 7. Variants / Follow-Ups

- Schedule tasks with cooldown
- Prevent repeated bookings
- Flood scheduling variants (LeetCode #1488)
---

## 8. Tips & Observations

- Always dry the lake that appears earliest in future to avoid conflicts
- TreeSet.ceiling(x) is the core trick
- Placeholder values on dry days are fine until assigned
- Drying unnecessary lakes does not break correctness
---

## 9. Pitfalls

- Forgetting to remove dry day after using it
- Assigning dry day to wrong lake
- Not handling placeholder value correctly
- Using linear search instead of TreeSet → TLE
---

<!-- #endregion -->
<!-- #region 192-Count Subarrays with Equal 1's and 0's -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q192: Count Subarrays with Equal 1's and 0's</h1>

## 1. Problem Statement

- You are given an array of size n containing only 0 and 1.
- You must find how many subarrays contain an equal number of 0s and 1s.
---

## 2. Problem Understanding

- A subarray with equal 0s and 1s has:
  * #1s - #0s = 0
- Trick:
  * Convert 0 → -1
  * Keep 1 as it is
- Then the problem becomes:
  * Count subarrays whose sum is 0.
- This becomes identical to counting zero-sum subarrays using prefix-sum frequency.
- Let prefix[i] = sum of transformed elements from 0 ... i.
- If prefix sum repeats:
  * prefix[j] == prefix[i]  → sum(i+1...j) = 0
- Hence, total pairs of equal prefix sums give total valid subarrays.
---

## 3. Constraints

- 1 ≤ N ≤ 10^6 (very large → must be O(N))
- Values are only 0 or 1
---

## 4. Edge Cases

- All zeros → after converting to -1, no sum-zero except possible pairs
- All ones → no valid subarrays
- Large N → must use O(N) map-based approach
- Single element → no possible equal subarray
---

## 5. Examples

```text
Example 1
Input

3
1 0 1
Output

2
Explanation

the subarrays from index 0 to 1 and 1 to 2 have equal number of 1's and 0's.

Example 2
Input

2
1 1
Output

0
Explanation

No such subarray is possible with equal number of 1's and 0's.
```

---

## 6. Approaches

### Approach 1: Brute Force (Too Slow)

**Idea:**
- Try all subarrays, count 0s and 1s.

**Java Code:**
```java
public int countSubarrays(int[] arr) {
    int n = arr.length;
    int count = 0;
    for (int i = 0; i < n; i++) {
        int zeros = 0, ones = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 0) zeros++;
            else ones++;
            if (zeros == ones) count++;
        }
    }
    return count;
}
```

**Complexity (Time & Space):**
- Time: O(N^2)
- Space: O(1)

### Approach 2: Convert 0 → -1 + Prefix Sum + HashMap (Optimal)

**Idea:**
- Transform array:
  * 0 → -1
  * 1 → 1
- Count subarrays with sum = 0 using:
  * prefix sum
  * HashMap storing freq of each prefix sum
- Rule:
  * If prefix sum x appears f times:
    * f choose 2  (pairs) = f*(f-1)/2
- Plus cases where prefix sum is 0 itself.

**Java Code:**
```java
public long countSubarrays(int[] arr) {
    int n = arr.length;

    Map<Integer, Long> map = new HashMap<>();
    map.put(0, 1L); // prefix sum 0 exists initially

    int prefix = 0;
    long count = 0;

    for (int x : arr) {
        if (x == 0) prefix += -1;
        else prefix += 1;

        if (map.containsKey(prefix)) {
            count += map.get(prefix);
        }

        map.put(prefix, map.getOrDefault(prefix, 0L) + 1);
    }

    return count;
}
```

**💭 Intuition Behind the Approach:**
- Converting 0 → -1 makes equal 0s and 1s become a zero-sum subarray.
- If prefix sum repeats, the intermediate subarray has sum 0.
- Counting frequencies of prefix sums automatically counts all valid subarrays.

**Complexity (Time & Space):**
- Time: O(N)
  * One pass + constant-time map operations
- Space: O(N)
  * Worst-case all prefix sums unique

---

## 7. Justification / Proof of Optimality

- Convert 0 → -1 so that equal 0s and 1s means the subarray sum becomes 0.
- If a prefix sum repeats, the subarray between those two indices sums to 0, which means equal 0s and 1s.
- Counting how many times each prefix sum occurs automatically counts all valid subarrays.
- This approach is O(N) and optimal because checking every subarray (O(N²)) is impossible for large N.
---

## 8. Variants / Follow-Ups

- Count subarrays with sum = K
- Largest subarray with equal 0s and 1s
- Replace elements to make balanced subarrays
- Zero-sum subarrays in general arrays
---

## 9. Tips & Observations

- Always put map.put(0, 1) initially
- Use long for counting (can overflow int)
- Prefix-sum hashing is universal for subarray equality problems
---

## 10. Pitfalls

- Forgetting to convert 0 → -1
- Forgetting prefix sum = 0 case
- Incorrectly pairing prefix sums
- Using int instead of long for count
---

<!-- #endregion -->
<!-- #region 193-Count Subarrays With Equal 0s, 1s and 2s -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q193: Count Subarrays With Equal 0s, 1s and 2s</h1>

## 1. Problem Statement

- You are given an array of size n containing only 0, 1, and 2.
- You must count how many contiguous subarrays contain equal numbers o
---

## 2. Problem Understanding

- A subarray has equal 0s, 1s, 2s if:
  * count0 = count1 = count2
- Direct counting per subarray is too slow.
- Key trick:
- Instead of tracking absolute counts, track differences:
  * d10 = count1 - count0
  * d21 = count2 - count1
- If at two different indices i and j, we have:
  * (d10_i == d10_j) AND (d21_i == d21_j)
- Then the subarray from (i+1 to j) has equal 0s, 1s, 2s.
- We will use a HashMap to count frequency of each (d10, d21) pair.
- This reduces to counting equal prefix-pattern repeats.
---

## 3. Constraints

- 1 ≤ N ≤ 1e6 → must be O(N)
- Values are only {0,1,2}
---

## 4. Edge Cases

- No valid subarray → answer = 0
- Single element → cannot form equal triplet
- All zeros or all ones or all twos → output 0
- Very large N → memory-efficient HashMap usage required
---

## 5. Examples

```text
Example 1:

2 0 1 2
Subarrays:
[2,0,1] → equal 0s,1s,2s
[0,1,2] → equal 0s,1s,2s
Answer = 2


Example 2:

1 1 2
No such subarray → 0
```

---

## 6. Approaches

### Approach 1: Brute Force (Too Slow)

**Idea:**
- Check all subarrays, count 0s, 1s, 2s

**Steps:**
- For each i, for each j ≥ i, count and check equality.

**Java Code:**
```java
public long countSubarrays(int[] arr) {
    int n = arr.length;
    long ans = 0;

    for (int i = 0; i < n; i++) {
        int c0 = 0, c1 = 0, c2 = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 0) c0++;
            else if (arr[j] == 1) c1++;
            else c2++;
            if (c0 == c1 && c1 == c2) ans++;
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Brute force checks all possible subarrays — simple but too slow.

**Complexity (Time & Space):**
- Time: O(N^2) — nested loops scan all subarrays
- Space: O(1) — only counters used

### Approach 2: Prefix Difference Method + HashMap (Optimal)

**Idea:**
- Track two differences:
  * d10 = count1 - count0
  * d21 = count2 - count1
- Whenever the same (d10, d21) pair appears again at index j, a subarray ending at j has equal 0,1,2 counts.
- Use HashMap to count frequencies of (d10, d21).

**Steps:**
- Initialize d10 = 0, d21 = 0.
- Push (0,0) into map with count 1 (prefix before array starts).
- Traverse array:
  * Update counts
  * Update d10, d21
  * Add existing frequency of this pair to answer
  * Increment map frequency

**Java Code:**
```java
public long countSubarrays(int[] arr) {
    Map<String, Long> map = new HashMap<>();
    long ans = 0;

    int c0 = 0, c1 = 0, c2 = 0;

    map.put("0#0", 1L);

    for (int x : arr) {
        if (x == 0) c0++;
        else if (x == 1) c1++;
        else c2++;

        int d10 = c1 - c0;
        int d21 = c2 - c1;

        String key = d10 + "#" + d21;

        if (map.containsKey(key)) {
            ans += map.get(key);
        }

        map.put(key, map.getOrDefault(key, 0L) + 1);
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Equal 0,1,2 counts imply identical difference patterns between two prefix points.
- Matching (d10,d21) ensures balanced increments inside the subarray.

**Complexity (Time & Space):**
- Time: O(N) — single scan with O(1) map ops
- Space: O(N) — storing distinct difference pairs

---

## 7. Justification / Proof of Optimality

- Equal 0s,1s,2s means:
  * (count1 - count0) and (count2 - count1)
- must remain unchanged over a subarray.
- Prefix difference repetition guarantees this condition.
- HashMap ensures we count all such subarrays in linear time.
---

## 8. Variants / Follow-Ups

- Count subarrays with equal 0 and 1
- Count subarrays with equal 0,1,...,k
- Largest subarray with equal 0,1,2
---

## 9. Tips & Observations

- Always store prefix (0,0) initially
- Use string key or a custom pair class
- The difference trick generalizes to k categories
---

## 10. Pitfalls

- Forgetting prefix (0,0) entry
- Incorrectly updating differences
- Using wrong key mapping (must combine both differences)
---

<!-- #endregion -->
<!-- #region 194-Strong numbers -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q194: Strong numbers</h1>

## 1. Problem Statement

- A number N is called strong if for every prime factor X of N, the number X² also divides N.
- You must return:
  * 1 (true) → if N is strong
  * 0 (false) → if N is not strong
---

## 2. Problem Understanding

- A number is strong only when every prime factor appears with exponent ≥ 2 in its prime factorization.
- Example:
  * 72 = 2^3 * 3^2
  * Prime 2 exponent = 3 → ≥ 2
  * Prime 3 exponent = 2 → ≥ 2
  * → strong
  * 7 = 7^1
  * Prime 7 exponent = 1 → < 2
  * → not strong
  * So the problem reduces to:
- ✔️ Check if each prime factor of N divides N at least twice.
---

## 3. Constraints

- 1 ≤ N ≤ 10000
- Very small limit → prime factorization is trivial and fast.
---

## 4. Edge Cases

- N = 1 → strong (no prime factors fail the rule)
- Prime numbers → always not strong
- Squares → usually strong
- Mixed primes with exponent 1 → fail
---

## 5. Examples

```text
Example 1:

N = 7  
Prime factor: 7^1 → exponent < 2 → output 0


Example 2:

N = 72  
Prime factors: 2^3, 3^2 → all exponents ≥ 2 → output 1
```

---

## 6. Approaches

### Approach 1: Prime Factorization + Check Exponents (Optimal)

**Idea:**
- Factorize N using trial division up to √N.
- For every prime factor, count exponent.
- If any exponent < 2 → return false.
- Else true.

**Steps:**
- For each i from 2 to √N:
  * Count exponent of i in N.
  * If i divides N but exponent < 2 → return false.
- After loop, if remaining N > 1 → that remaining N is a prime factor with exponent 1 → return false.
- Else return true.

**Java Code:**
```java
public boolean isStrong(int N) {
    if (N == 1) return true; 

    int num = N;

    for (int p = 2; p * p <= num; p++) {
        if (num % p == 0) {
            int count = 0;
            while (num % p == 0) {
                num /= p;
                count++;
            }
            if (count < 2) return false;
        }
    }

    if (num > 1) return false;

    return true;
}
```

**💭 Intuition Behind the Approach:**
- Prime factorization exposes each prime's exponent.
- A prime factor contributes at least two copies only if it divides the number twice.
- Thus checking exponent ≥ 2 for every prime factor is enough.

**Complexity (Time & Space):**
- Time: O(√N) — trial division up to square root
- Space: O(1) — only counters used

---

## 7. Justification / Proof of Optimality

- A number is strong if and only if every prime factor appears at least twice in its prime factorization.
- Why? Because:
  * If a prime factor p divides the number only once (p¹), then p² cannot divide the number → fails the condition.
  * If a prime factor appears twice or more (p², p³, ...), then p² divides the number → condition satisfied.
  * Prime factorization uniquely determines exponents, so checking exponent ≥ 2 is both necessary and sufficient.
- Thus, verifying that each prime factor has exponent ≥ 2 gives a mathematically complete and optimal solution.
---

## 8. Variants / Follow-Ups

- Check if number is perfect square
- Check if number is powerful (same definition — this is the classical "Powerful Number" problem)
- Check if number is squareful
---

## 9. Tips & Observations

- This problem is identical to checking if N is a Powerful Number.
- Remaining N > 1 after factor loop means a leftover prime with exponent 1 → fail.
- Works efficiently up to 10k with ease.
---

## 10. Pitfalls

- Forgetting that N > 1 after factorization implies exponent 1
- Not handling N = 1
- Checking divisibility only once instead of counting exponent
---

<!-- #endregion -->
<!-- #region 195-Convert to Roman No -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q195: Convert to Roman No</h1>

## 1. Problem Statement

- You are given an integer n (1 ≤ n ≤ 3999).
- You must convert it into its Roman numeral representation using these symbols:
  * I = 1
  * V = 5
  * X = 10
  * L = 50
  * C = 100
  * D = 500
  * M = 1000
- Output the roman numeral string.
---

## 2. Problem Understanding

- Roman numeral conversion involves subtracting largest possible values first.
- Key rules:
  * Roman numbers use additive notation (e.g., III = 3, XX = 20)
  * Special subtractive pairs exist:
    * IV = 4
    * IX = 9
    * XL = 40
    * XC = 90
    * CD = 400
    * CM = 900
- Always pick the largest roman value ≤ n, append its symbol, subtract, and continue.
---

## 3. Constraints

- 1 ≤ n ≤ 3999
- Roman system supports numbers only up to 3999 by standard notation.
---

## 4. Edge Cases

- n < 10 → purely simple symbols like I, II, III…
- n containing 4 or 9 → must use subtractive notation
- Largest values like 3888 → repeated M/D/C/X etc.
---

## 5. Examples

```text
Example 1:

Input: 5  
Output: V


Example 2:

Input: 3  
Output: III


Example 3:

Input: 944  
Output: CMXLIV
```

---

## 6. Approaches

### Approach 1: Greedy + Predefined Roman Mapping (Optimal)

**Idea:**
- Use a sorted list of value–symbol pairs and repeatedly subtract the largest possible value from n while building the answer.

**Steps:**
- Create arrays of roman values and symbols in decreasing order.
- For each value:
  * While n >= value, append symbol and subtract value.
- Continue until n becomes 0

**Java Code:**
```java
public String convertToRoman(int n) {
    int[] values = 
        {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};

    String[] symbols = 
        {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    StringBuilder sb = new StringBuilder();

    for (int i = 0; i < values.length; i++) {
        while (n >= values[i]) {
            sb.append(symbols[i]);
            n -= values[i];
        }
    }

    return sb.toString();
}
```

**💭 Intuition Behind the Approach:**
- Roman numerals work in a greedy system — always choose the largest symbol that fits.
- Subtractive symbols (like 900, 400, 90, etc.) ensure correctness without special-case logic.

**Complexity (Time & Space):**
- Time: O(1) — fixed list of 13 values, constant operations
- Space: O(1) — output string at most length ~15

### Approach 2: Digit-wise Mapping (Alternative)

**Idea:**
- Handle each digit place (thousands, hundreds, tens, ones) with predefined patterns

**Java Code:**
```java
public String convertToRoman(int num) {
    String[] thousands = {"", "M", "MM", "MMM"};
    String[] hundreds  = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
    String[] tens      = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
    String[] ones      = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};

    return thousands[num / 1000] +
           hundreds[(num % 1000) / 100] +
           tens[(num % 100) / 10] +
           ones[num % 10];
}
```

**💭 Intuition Behind the Approach:**
- Roman numerals follow a strict pattern per digit place, allowing table lookup.

**Complexity (Time & Space):**
- Time: O(1) — constant digit operations
- Space: O(1) — fixed tables

---

## 7. Justification / Proof of Optimality

- Roman numerals are constructed by repeatedly taking the largest available symbol/value that does not exceed the number; subtracting it and continuing produces a valid representation.
- Including the subtractive pairs (900, 400, 90, 40, 9, 4) in the descending value list makes the greedy choice always correct because those pairs are the only legal ways to represent those values and they prevent invalid long sequences (e.g., 4 should be IV not IIII).
- Therefore a greedy algorithm over the fixed, ordered list of value–symbol pairs is necessary and sufficient and runs in constant time (the list is fixed size).
---

## 8. Variants / Follow-Ups

- Convert Roman to Integer
- Validate Roman numeral format
- Minimal Roman vs extended Roman rules
---

## 9. Tips & Observations

- Roman numerals are inherently greedy-friendly.
- Always include subtractive cases like 900, 400, 90 to simplify logic.
- Keep a fixed value–symbol mapping sorted in descending order.
---

## 10. Pitfalls

- Missing subtractive pairs leads to incorrect output
- Forgetting that the maximum valid number is 3999
- Using inefficient string concatenation instead of StringBuilder
---

<!-- #endregion -->
<!-- #region 196-Distinct Numbers in Window -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q196: Distinct Numbers in Window</h1>

## 1. Problem Statement

- You are given an array of N integers and a window size k.
- For every window A[i .. i+k-1], you must print the count of distinct elements.
- Return an array of length N - k + 1.
- If k > N, return an empty array.
---

## 2. Problem Understanding

- This is a classic sliding window problem where we must efficiently maintain:
  * The number of distinct elements in a window of size k
  * Sliding the window right by 1 means:
    * Remove the element leaving the window
    * Add the element entering the window
    * Update distinct count accordingly
  * A HashMap (or HashMap-like frequency map) is ideal:
    * Increase count when adding new element
    * Decrease count when removing element
    * When frequency becomes 0 → element is no longer in window → distinct--
- This gives O(N) performance.
---

## 3. Constraints

- 1 ≤ N, k ≤ 1e5
- Array values up to 1e9
- Must be O(N)
---

## 4. Edge Cases

- k > N → empty result
- All elements same → every window has distinct = 1
- All elements different → every window has distinct = k
- Large N → must avoid O(N*k)
---

## 5. Examples

```text
Example 1:

A = [1,2,1,3,4,3], k = 3
Output = [2,3,3,2]


Example 2:

A = [1,2,3,2], k = 1
Output = [1,1,1,1]
```

---

## 6. Approaches

### Approach 1: Sliding Window + HashMap (Optimal)

**Idea:**
- Maintain a frequency map for the current window:
  * Insert elements until the window reaches size k
  * Distinct count = number of keys with freq > 0
  * Slide window:
    * Remove outgoing element (reduce frequency or delete)
    * Add incoming element (increase frequency)
  * Append distinct count at each step

**Steps:**
- Use a HashMap<Integer, Integer> as a frequency map.
- Process first k elements → fill map → record distinct count.
- For each next position:
  * Remove old element: decrement freq; if freq = 0 → remove key.
  * Add new element: increment freq.
  * Append map.size() to result.
- Return result list/array.

**Java Code:**
```java
public List<Integer> distinctInWindow(int[] arr, int k) {
    int n = arr.length;
    List<Integer> res = new ArrayList<>();
    if (k > n) return res;

    Map<Integer, Integer> freq = new HashMap<>();

    // First window
    for (int i = 0; i < k; i++) {
        freq.put(arr[i], freq.getOrDefault(arr[i], 0) + 1);
    }
    res.add(freq.size());

    // Slide the window
    for (int i = k; i < n; i++) {

        // Remove outgoing element
        int out = arr[i - k];
        freq.put(out, freq.get(out) - 1);
        if (freq.get(out) == 0) freq.remove(out);

        // Add incoming element
        int in = arr[i];
        freq.put(in, freq.getOrDefault(in, 0) + 1);

        res.add(freq.size());
    }

    return res;
}
```

**💭 Intuition Behind the Approach:**
- The window changes by only one element at a time.
- So instead of recalculating distinct values for every window, maintain frequencies incrementally:
  * Add the entering element
  * Remove the leaving element
- HashMap size naturally tracks the number of distinct elements.
- This converts an O(N*k) brute force into O(N).

**Complexity (Time & Space):**
- Time: O(N) — each element is inserted and removed at most once from the map
- Space: O(k) — frequency map stores at most k distinct elements

### Approach 2: Brute Force (Too Slow)

**Idea:**
- For each window, use a Set to count distinct elements

**Java Code:**
```java
public List<Integer> distinctInWindow(int[] arr, int k) {
    int n = arr.length;
    List<Integer> res = new ArrayList<>();
    if (k > n) return res;

    for (int i = 0; i <= n - k; i++) {
        Set<Integer> set = new HashSet<>();
        for (int j = i; j < i + k; j++) {
            set.add(arr[j]);
        }
        res.add(set.size());
    }

    return res;
}
```

**💭 Intuition Behind the Approach:**
- Directly count distinct values in each window, but recomputing every window independently causes large repeated work.

**Complexity (Time & Space):**
- Time: O(N*k) — scanning k elements for each of the N-k+1 windows
- Space: O(k) — set stores up to k elements

---

## 7. Justification / Proof of Optimality

- Distinct count changes only when elements enter or leave the window.
- HashMap frequencies allow constant-time update of distinctness (via map size), so sliding the window with incremental updates is both necessary and sufficient to achieve O(N) time.
- Brute force repeats work; HashMap avoids recomputation.
---

## 8. Variants / Follow-Ups

- Count distinct elements in window size K for strings
- Count distinct numbers in every subarray of varying window sizes
- Sliding window max/min
- Frequency sliding window problems (e.g., longest substring with K unique characters)
---

## 9. Tips & Observations

- Removing outgoing element is crucial; forgetting to delete freq=0 breaks the distinct count.
- Works well even with very large numbers since HashMap handles arbitrary values.
- Suitable for all sliding-window frequency problems.
---

## 10. Pitfalls

- Not removing element when frequency hits zero
- Using map.size() incorrectly (should reflect distinct count)
- k > N must return empty array
---

<!-- #endregion -->
