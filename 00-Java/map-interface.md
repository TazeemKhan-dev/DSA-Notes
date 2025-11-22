<!-- #region Map Interface -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: Map Interface</h1>

## 1. Problem Understanding

- Represents a collection of key-value pairs.
- Each key is unique; values can be duplicate.
- Keys cannot be null in some implementations (TreeMap), but values can be null.
- Common implementations: HashMap, TreeMap, LinkedHashMap.
- Main methods:
  * V put(K key, V value) → Inserts a key-value pair. If the key exists, updates the value.
  * V get(Object key) → Returns the value associated with the key. Returns null if key not found.
  * V remove(Object key) → Removes the key-value pair by key. Returns the value removed.
  * boolean containsKey(Object key) → Checks if the key exists. Returns true/false.
  * boolean containsValue(Object value) → Checks if the value exists. Returns true/false.
  * Set<K> keySet() → Returns all keys as a Set.
  * Collection<V> values() → Returns all values.
  * Set<Map.Entry<K,V>> entrySet() → Returns all key-value pairs as a set of entries.

- **HashMap (java.util.HashMap)**
    - Stores key-value pairs in hash table.
    - Allows one null key and multiple null values.
    - Not ordered (insertion order is not guaranteed).
    - O(1) average time complexity for get(), put(), remove().
    - Not synchronized (thread-unsafe).
    - Example:
      * HashMap<String, Integer> map = new HashMap<>();
      * map.put("A", 1);
      * int val = map.get("A"); // returns 1
      * boolean hasKey = map.containsKey("A"); // true
      * boolean hasVal = map.containsValue(1); // true

- **TreeMap (java.util.TreeMap)**
    - Implements SortedMap → keys are sorted in natural order or by a comparator.
    - No null keys allowed (throws NullPointerException), but null values are allowed.
    - Internally uses a Red-Black Tree.
    - Time complexity: O(log n) for get(), put(), remove().
    - Example:
      * TreeMap<String, Integer> treeMap = new TreeMap<>();
      * treeMap.put("C", 3);
      * treeMap.put("A", 1);
      * treeMap.put("B", 2);
      * // Keys will be sorted: A, B, C
      * int val = treeMap.get("B"); // 2

- **Key Functions: .get() vs .containsKey() vs .containsValue()**
    - .get(key) → returns value for the given key. Returns null if key is not present.
    - .containsKey(key) → checks if a key exists. Returns true or false.
    - .containsValue(value) → checks if a value exists. Returns true or false.
    - Example:
      * HashMap<String, Integer> map = new HashMap<>();
      * map.put("X", 10);
      * map.get("X"); // returns 10
      * map.containsKey("X"); // true
      * map.containsValue(10); // true
      * map.containsKey("Y"); // false
      * map.containsValue(20); // false

- **Map Interface Methods (java.util.Map)**
    - Adding / Updating
      * V put(K key, V value) → Adds key-value pair. Updates if key exists.
      * void putAll(Map<? extends K, ? extends V> m) → Copies all mappings from another map.
      * V putIfAbsent(K key, V value) → Adds key-value pair only if key is absent.
    - Retrieving
      * V get(Object key) → Returns value for the key, or null if key not found.
      * V getOrDefault(Object key, V defaultValue) → Returns value if key exists, else returns defaultValue.
    - Removing
      * V remove(Object key) → Removes entry by key, returns removed value or null.
      * boolean remove(Object key, Object value) → Removes entry only if key maps to value. Returns true if removed.
    - Checking existence
      * boolean containsKey(Object key) → Checks if key exists.
      * boolean containsValue(Object value) → Checks if value exists.
    - Size / Emptiness
      * int size() → Number of key-value mappings.
      * boolean isEmpty() → Checks if map is empty.
    - Iteration / Views
      * Set<K> keySet() → Returns all keys.
      * Collection<V> values() → Returns all values.
      * Set<Map.Entry<K, V>> entrySet() → Returns all key-value pairs as Map.Entry.
    - Replacement / Compute
      * V replace(K key, V value) → Replaces value for key if it exists.
      * boolean replace(K key, V oldValue, V newValue) → Replaces only if current value matches oldValue.
      * V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction) → Computes new value.
      * V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction) → Computes and inserts value if key is absent.
      * V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction) → Computes new value only if key is present.
    - Merge / Other Utilities
      * V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction) → Merges value with existing value.
      * void clear() → Removes all entries.
      * default void forEach(BiConsumer<? super K, ? super V> action) → Performs action on each entry.
---

## 2. Approaches

### Approach 1: 

**Java Code:**
```java
import java.util.*;
import java.util.function.*;

public class MapMethodsExample {
    public static void main(String[] args) {

        // Create a HashMap
        Map<String, Integer> map = new HashMap<>();
        
        // --- Adding / Updating ---
        map.put("A", 10); // Add
        map.put("B", 20); 
        map.put("A", 15); // Update
        map.putIfAbsent("C", 30); // Add only if key is absent
        map.putIfAbsent("B", 50); // Won't update because B exists
        System.out.println("After put & putIfAbsent: " + map);

        // --- Retrieving ---
        System.out.println("get(\"A\"): " + map.get("A")); // 15
        System.out.println("getOrDefault(\"D\", 100): " + map.getOrDefault("D", 100)); // 100

        // --- Checking existence ---
        System.out.println("containsKey(\"B\"): " + map.containsKey("B")); // true
        System.out.println("containsValue(30): " + map.containsValue(30)); // true

        // --- Removing ---
        map.remove("C"); // remove by key
        System.out.println("After remove(\"C\"): " + map);
        boolean removed = map.remove("B", 25); // remove only if value matches
        System.out.println("Attempt to remove B with value 25: " + removed + " | Map: " + map);

        // --- Size / Emptiness ---
        System.out.println("size(): " + map.size()); // 2
        System.out.println("isEmpty(): " + map.isEmpty()); // false

        // --- Iteration / Views ---
        System.out.println("Keys: " + map.keySet()); // [A, B]
        System.out.println("Values: " + map.values()); // [15, 20]
        System.out.println("Entries: " + map.entrySet()); // [A=15, B=20]

        // Iterate using forEach
        map.forEach((k, v) -> System.out.println(k + " -> " + v));

        // --- Replacement / Compute ---
        map.replace("A", 50); // replace value
        map.replace("B", 20, 40); // replace only if old value matches
        System.out.println("After replace: " + map);

        map.compute("A", (k, v) -> v + 5); // 50 + 5
        map.computeIfAbsent("D", k -> 100); // add only if absent
        map.computeIfPresent("B", (k, v) -> v * 2); // 40 * 2
        System.out.println("After compute methods: " + map);

        // --- Merge / Utilities ---
        map.merge("A", 20, (oldVal, newVal) -> oldVal + newVal); // 55 + 20 = 75
        System.out.println("After merge: " + map);

        map.clear();
        System.out.println("After clear: " + map + " | isEmpty: " + map.isEmpty());
    }
}
```

---

<!-- #endregion -->
