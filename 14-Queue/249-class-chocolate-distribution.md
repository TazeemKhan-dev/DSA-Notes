<!-- #region 249-Class chocolate distribution -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q249: Class chocolate distribution</h1>

## 1. Problem Statement

- There are n classes standing in a queue to receive chocolates.
- Each class has a certain number of students, and each student needs exactly 1 chocolate.
- You are given:
  * A 0-indexed integer array classes where classes[i] represents the number of students in the i-th class.
  * An integer k, representing the index of a specific class.
- Rules:
  * A class can take only 1 chocolate at a time.
  * Taking 1 chocolate takes 1 second.
  * After taking a chocolate:
    * If the class still has students left → it goes to the end of the queue instantly.
    * If the class has no students left → it leaves the queue.
  * The queue order is maintained strictly.
- Task:
  * Return the total time (in seconds) taken for the class at index k to finish receiving chocolates for all its students.
---

## 2. Problem Understanding

- This is a round-robin distribution problem.
- Each class gets turns cyclically.
- Time increases by exactly 1 second per chocolate given.
- We stop counting the moment class k gets its last chocolate.
- 👉 Core idea: How many times does each class get to take a chocolate before class k finishes?
---

## 3. Constraints

- 1 <= n <= 100
- 1 <= classes[i] <= 100
- 0 <= k < n
---

## 4. Edge Cases

- classes[k] == 1 → finishes in the first cycle.
- n == 1 → time = classes[0]
- Classes before k may finish early and leave the queue.
- Classes after k do not contribute in the final second.
---

## 5. Examples

```text
Input:
3
2 3 2
2

Output:
6


Input:
4
5 1 1 1
0

Output:
8
```

---

## 6. Approaches

### Approach 1: Brute Force Queue Simulation

**Idea:**
- Simulate the process exactly as described using a queue.

**Steps:**
- Push (index, remainingStudents) into a queue.
- While queue is not empty:
  * Pop front.
  * Give 1 chocolate → time++.
  * If this class becomes empty:
    * If it is class k, stop.
- Else push it back.

**Java Code:**
```java
static int timeRequired(int[] classes, int k) {
    Queue<int[]> q = new ArrayDeque<>();
    for (int i = 0; i < classes.length; i++) {
        q.offer(new int[]{i, classes[i]});
    }

    int time = 0;

    while (!q.isEmpty()) {
        int[] curr = q.poll();
        time++;
        curr[1]--;

        if (curr[1] == 0) {
            if (curr[0] == k) return time;
        } else {
            q.offer(curr);
        }
    }
    return time;
}
```

**💭 Intuition Behind the Approach:**
- We literally execute the rules step by step.
- Queue preserves order, and every chocolate consumes 1 second.

**Complexity (Time & Space):**
- Time: O(sum(classes)) — one operation per chocolate
- Space: O(n) — queue storage

### Approach 2: Optimized Mathematical Counting (Optimal)

**Idea:**
- Class k needs classes[k] chocolates → classes[k] rounds.
  * Classes before or at k can get chocolates in all those rounds
  * Classes after k can get chocolates in only classes[k] - 1 rounds

**Steps:**
- Let target = classes[k]
- For every class i:
  * If i <= k → add min(classes[i], target)
  * If i > k → add min(classes[i], target - 1)
- Sum is the answer.

**Java Code:**
```java
static int timeRequired(int[] classes, int k) {
    int time = 0;
    int target = classes[k];

    for (int i = 0; i < classes.length; i++) {
        if (i <= k) {
            time += Math.min(classes[i], target);
        } else {
            time += Math.min(classes[i], target - 1);
        }
    }
    return time;
}
```

**💭 Intuition Behind the Approach:**
- The last chocolate of class k ends everything.
- Classes after k cannot delay that last second.
- So we count only the effective blocking time.

**Complexity (Time & Space):**
- Time: O(n) — single pass
- Space: O(1) — no extra data structures

---

## 7. Justification / Proof of Optimality

- Queue simulation matches the problem definition exactly.
- Optimized approach counts how many seconds each class blocks class k.
- Both produce identical results.
---

## 8. Variants / Follow-Ups

- Ticket Buying (LeetCode 2073)
- Round-Robin CPU Scheduling
- Printer Queue problems
---

## 9. Tips & Observations

- “Go to end of the line” → Queue
- “One unit per second” → Time simulation
- Always check if last round can be excluded for optimization
---

## 10. Pitfalls

- Counting classes after k in the last round
- Forgetting that re-queueing is instantaneous
- Over-simulating when constraints are small
---

<!-- #endregion -->
