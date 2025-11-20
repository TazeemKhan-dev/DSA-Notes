<!-- #region 127-Boats to Save People -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q127: Boats to Save People</h1>

## 1. Problem Understanding

- You're given:
  * an array people[] of weights
  * unlimited boats
  * each boat can carry at most 2 people
  * total weight inside a boat must be ≤ limit
- Goal: Find the minimum number of boats needed.
- This is a classic greedy pairing problem.
---

## 2. Constraints

- 1 ≤ N ≤ 5 * 10^4
- 1 ≤ people[i] ≤ limit ≤ 3 * 10^4
- Every person must be placed in a boat
- At most 2 people per boat
- Order does not matter
---

## 3. Edge Cases

- N = 1 → answer = 1
- Everyone too heavy to pair → each gets its own boat
- Perfect pairs exist → minimum boats
- All weights identical
- Very light + very heavy pairing scenario
---

## 4. Examples

```text
Example 1
N = 2, limit = 3
people = [1, 2]

Output:
1
Pair (1, 2)

Example 2
N = 4, limit = 5
people = [3, 5, 3, 4]

Possible pairs:
(5)
(4)
(3)
(3)

Output:
4
```

---

## 5. Approaches

### Approach 1: Try All Pairings (Brute Force: TLE)

**Idea:**
- Try every combination of two people to place into boats.
- ❌ Why it fails
  * O(N²) or worse
  * N up to 50k → impossible

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(N²) or worse
- 💾 Space Complexity
  * O(1)

### Approach 2: Greedy (Two-Pointer Technique) Optimal

**Idea:**
- We want to place heavy people first, and try to pair them with the lightest person available.
- Why it works?
  * If the heaviest cannot pair with the lightest, they cannot pair with anyone.
  * If the heaviest can pair with the lightest, it reduces boat count.
- This greedy rule ensures minimal boats.

**Steps:**
- Sort the array
- Use two pointers:
  * i at start (lightest)
  * j at end (heaviest)
- While i <= j:
  * If people[i] + people[j] <= limit
    * pair them → i++, j--
  * Else
    * send heavy alone → j--
  * Increment boats count

**Java Code:**
```java
public int numRescueBoats(int[] people, int limit) {
    Arrays.sort(people);
    int i = 0, j = people.length - 1;
    int boats = 0;

    while (i <= j) {
        if (people[i] + people[j] <= limit) {
            i++;   // lightest used
            j--;   // heaviest used
        } else {
            j--;   // heaviest goes alone
        }
        boats++;
    }

    return boats;
}
```

**💭 Intuition Behind the Approach:**
- The key observation:
  * The heaviest person must be placed in a boat now.
  * If they can be paired with the lightest, it's optimal to do so.
  * If not, pairing them with anyone heavier is impossible → they go alone.
- By always using the least possible space per boat and maximizing pairing,
- we guarantee minimum boats.
- This matches the greedy strategy of "minimize waste per step."

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Sorting → O(N log N)
  * Two-pointer scan → O(N)
  * Total:
    * O(N log N)
- 💾 Space Complexity
  * O(1) extra (sorting in-place, no extra arrays)

---

## 6. Justification / Proof of Optimality

- The greedy strategy is optimal because pairing the heaviest person with anyone heavier cannot work.
- If the heaviest can pair with the lightest, it’s always optimal to pair them, as:
  * It “saves” one person
  * It avoids wasting a boat
- Sorting + two pointers ensures we try the best possible pairing at each step.
- This is the solution used in major interview problems.
---

## 7. Variants / Follow-Ups

- Boats with capacity for 3 people
- Boats with variable weight limits
- Each boat can take unlimited people but max total weight K (bin packing light version)
- Grouping students into taxis (same logic)
- Pairing problems in greedy optimization
---

## 8. Tips & Observations

- Always pair heavy with lightest first
- If they cannot pair, heavy MUST go alone
- Sort only once
- Use two pointers, not nested loops
- When i == j, that one person requires a boat
- Never skip pairing opportunities; greedy is optimal
---

<!-- #endregion -->
