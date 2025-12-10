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
