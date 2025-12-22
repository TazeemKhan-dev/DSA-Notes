<!-- #region 233-Circular Tour -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q233: Circular Tour</h1>

## 1. Problem Statement

- There are N petrol pumps arranged in a circular manner.
- You are given:
  * An array petrol[] where petrol[i] is the amount of petrol at pump i
  * An array distance[] where distance[i] is the distance from pump i to the next pump (i+1)
- Assume:
  * 1 unit of petrol allows the truck to travel 1 unit distance
- Find the starting petrol pump index from where the truck can complete the entire circular tour without running out of petrol.
- If no such starting point exists, return -1.
---

## 2. Problem Understanding

- The truck starts with 0 petrol
- At each pump:
  * It gains petrol[i]
  * It must travel distance[i] to the next pump
- The tour is circular → after last pump, it must return to the first
- If petrol ever becomes negative → journey fails
- This is a feasibility + optimization problem.
---

## 3. Constraints

- 2 <= N <= 10000
- 1 <= petrol[i], distance[i] <= 1000
---

## 4. Edge Cases

- Total petrol < total distance → impossible
- Single valid starting point
- Multiple pumps but only one feasible start
- All pumps individually fail but total still sufficient
---

## 5. Examples

```text
Input

4
4 6 7 4
6 5 3 5
Output

1
Explanation

There are 4 petrol pumps with amount of petrol and distance to next

petrol pump value pairs as {4, 6}, {6, 5}, {7, 3} and {4, 5}. The first point from

where truck can make a circular tour is 2nd petrol pump. Output in this case is 1

(index of 2nd petrol pump).

Example 2
Input

2
1 1 2 3
Output

-1
Explanation

No solution exists.
```

---

## 6. Approaches

### Approach 1: Brute Force (Try Every Starting Point)

**Idea:**
- Try starting the tour from each petrol pump and simulate the entire circular journey.

**Steps:**
- For each index i from 0 to N-1
- Start with fuel = 0
- Traverse all pumps circularly
- If fuel becomes negative → break
- If full circle completes → return i

**Java Code:**
```java
static int circularTourBrute(int[] petrol, int[] dist, int n) {
    for (int start = 0; start < n; start++) {
        int fuel = 0;
        boolean possible = true;

        for (int i = 0; i < n; i++) {
            int idx = (start + i) % n;
            fuel += petrol[idx] - dist[idx];

            if (fuel < 0) {
                possible = false;
                break;
            }
        }

        if (possible) return start;
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Direct simulation
- Simple and easy to reason
- Inefficient because it repeats work for every start

**Complexity (Time & Space):**
- Time: O(N^2) — full traversal for each start
- Space: O(1)

### Approach 2: Greedy / Queue Elimination (Optimal)

**Idea:**
- Instead of trying all starts:
  * Track current fuel
  * Track total petrol vs total distance
  * If fuel becomes negative at index i, then no pump between previous start and i can be a valid start

**Steps:**
- Initialize:
- totalFuel = 0
- currFuel = 0
- start = 0
- Traverse pumps from 0 to N-1
- Update:
  * currFuel += petrol[i] - distance[i]
  * totalFuel += petrol[i] - distance[i]
- If currFuel < 0:
  * Reset currFuel = 0
  * Set start = i + 1
- After loop:
  * If totalFuel < 0 → return -1
  * Else return start

**Java Code:**
```java
static int circularTour(int[] petrol, int[] dist, int n) {
    int totalFuel = 0;
    int currFuel = 0;
    int start = 0;

    for (int i = 0; i < n; i++) {
        int gain = petrol[i] - dist[i];
        totalFuel += gain;
        currFuel += gain;

        if (currFuel < 0) {
            start = i + 1;
            currFuel = 0;
        }
    }

    return totalFuel >= 0 ? start : -1;
}

Alternative Greedy Implementation

int tour(int petrol[], int distance[]) {
    int n = petrol.length;

    int totalPetrol = 0;
    int totalDistance = 0;

    for (int i = 0; i < n; i++) {
        totalPetrol += petrol[i];
        totalDistance += distance[i];
    }

    if (totalPetrol < totalDistance) return -1;

    int currFuel = 0;
    int start = 0;

    for (int i = 0; i < n; i++) {
        currFuel += petrol[i] - distance[i];

        if (currFuel < 0) {
            start = i + 1;
            currFuel = 0;
        }
    }

    return start;
}
```

**💭 Intuition Behind the Approach:**
- If you fail at pump i, any start before i also fails
- So we safely skip all those starts
- Total fuel check ensures feasibility
- One pass is enough
- Alternateive approach
- If total petrol is insufficient, no solution exists
- If the truck fails at index i, any start before i is invalid
- Resetting start skips all impossible pumps
- One full pass is enough to find the answer

**Complexity (Time & Space):**
- Time: O(N) — single traversal
- Space: O(1)

---

## 7. Justification / Proof of Optimality

- Brute force is too slow for large N
- Greedy eliminates impossible starts early
- Optimal solution is clean, fast, and interview-standard
---

## 8. Variants / Follow-Ups

- Gas Station (LeetCode)
- Circular queue feasibility
- Resource allocation problems
- Scheduling with wrap-around constraints
---

## 9. Tips & Observations

- Always check total petrol vs total distance
- Reset start only when current fuel becomes negative
- Greedy works because failure region is provably invalid
---

## 10. Pitfalls

- Returning start without checking total fuel
- Off-by-one errors in start update
- Trying BFS/DP unnecessarily
- Overthinking circular traversal
---

<!-- #endregion -->
