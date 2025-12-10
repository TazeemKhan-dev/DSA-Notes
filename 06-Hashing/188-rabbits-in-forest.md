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
