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
