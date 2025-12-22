<!-- #region 0-Queue -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Queue</h1>

## 1. Problem Statement

- Queue is a linear data structure that follows FIFO order:
- First In First Out
  * The element added first is removed first
- Think of a line at a counter:
  * first person → served first

- **Core Terminology**
- Enqueue → Insert element at the rear
- Dequeue → Remove element from the front
- Front / Peek → Element at the front without removing it
- Rear → Last inserted element
- isEmpty → Check if queue is empty
- size → Number of elements

- **Intuition Behind the Concept**
- Queue is used when order matters
- Useful when tasks must be processed in the same order they arrive
- Unlike stack (LIFO), queue ensures fair processing

- **Basic Operations (Conceptually)**
- Enqueue → push from one side
- Dequeue → pop from the other side
- Access is restricted:
    * No random access like arrays

- **💡 Types of Queue (Theory)**
- Simple Queue
- Circular Queue
- Deque (Double Ended Queue)
- Priority Queue

- **Queue Declaration in Java (Most Important)**
- ✅ Using LinkedList (Most Common)
  * Queue<Integer> q = new LinkedList<>();
  * Why?
  * LinkedList implements Queue
  * Allows dynamic size
  * Fast enqueue & dequeue → O(1)
- ✅ Using ArrayDeque (Recommended for DSA)
  * Queue<Integer> q = new ArrayDeque<>();
  * Why better?
  * Faster than LinkedList
  * No unnecessary node overhead
  * Preferred in interviews & competitive coding
- ❌ Using PriorityQueue (Different behavior)
    * Queue<Integer> q = new PriorityQueue<>();
    * ❌ Does NOT follow FIFO
    * Elements come out by priority, not order

```text
q.add(10);        // Enqueue (throws exception if fails)
q.offer(20);      // Enqueue (returns false if fails)

q.remove();       // Dequeue (throws exception if empty)
q.poll();         // Dequeue (returns null if empty)

q.peek();         // Front element (null if empty)
q.element();      // Front element (exception if empty)

q.isEmpty();      // Check empty
q.size();         // Queue size

```

- **add vs offer (Interview Favorite)**
- add() → throws exception on failure
- offer() → safe, returns false

- **remove vs poll**
- remove() → exception if empty
- poll() → returns null

- **When to Use Queue in DSA**
- BFS (Breadth First Search)
- Level order traversal (Trees)
- Sliding window problems
- Task scheduling
- Producer–Consumer problems

- **Complexity**
- ⏱️ Time Complexity (Core Operations)
  * Enqueue → O(1)
  * Dequeue → O(1)
  * Peek → O(1)
  * Why?
  * Because insertion & removal happen only at ends
- 💾 Space Complexity
  * O(n) — stores n elements
---

## 2. Pitfalls

- Using PriorityQueue when FIFO is required
- Using remove() instead of poll() without empty check
- Confusing Queue vs Deque
- Expecting random access like array
---

<!-- #endregion -->
