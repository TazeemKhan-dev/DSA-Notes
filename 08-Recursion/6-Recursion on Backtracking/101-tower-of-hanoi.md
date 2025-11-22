<!-- #region 101-Tower of Hanoi -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q101: Tower of Hanoi</h1>

## 1. Problem Understanding

- You are given N discs and three rods: 1, 2, and 3.
- Initially, all discs are stacked on rod 1.
- You must move all discs to rod 3, following these rules:
  * Only one disc can be moved at a time.
  * A larger disc cannot be placed on top of a smaller disc.
  * You may use rod 2 as an auxiliary helper.
- Each move must be printed in the format:
- move disk i from rod x to rod y
- Goal → Move all discs from rod 1 to rod 3 using the minimum number of moves.
---

## 2. Constraints

- 1 <= N <= 15
- Total moves = 2^N - 1
- Must follow Tower of Hanoi rules strictly.
---

## 3. Edge Cases

- N = 1 → Only one move required.
- For large N, recursion depth could be significant.
- Ensure output format is consistent and rods are referenced correctly.
---

## 4. Examples

```text
Input:

3


Output:

move disk 1 from rod 1 to rod 3
move disk 2 from rod 1 to rod 2
move disk 1 from rod 3 to rod 2
move disk 3 from rod 1 to rod 3
move disk 1 from rod 2 to rod 1
move disk 2 from rod 2 to rod 3
move disk 1 from rod 1 to rod 3
```

---

## 5. Approaches

### Approach 1: Recursive Approach (Classic Divide & Conquer)

**Idea:**
- To move N disks from source → destination:
  * Move the top N-1 disks from source → helper.
  * Move the bottom (largest) disk from source → destination.
  * Move the N-1 disks from helper → destination.
- This is a divide-and-conquer problem where each step breaks the problem into smaller subproblems.

**Steps:**
- Base case: if N=1, directly move from fromRod → toRod.
- Recursive case:
  * Move N−1 disks to the auxiliary rod.
  * Move the N th disk to the destination rod.
  * Move N−1 disks from the auxiliary rod to the destination rod.

**Java Code:**
```java
public static void towerOfHanoi(int n, int fromRod, int toRod, int auxRod) {
    if (n == 1) {
        System.out.println("move disk 1 from rod " + fromRod + " to rod " + toRod);
        return;
    }

    towerOfHanoi(n - 1, fromRod, auxRod, toRod);
    System.out.println("move disk " + n + " from rod " + fromRod + " to rod " + toRod);
    towerOfHanoi(n - 1, auxRod, toRod, fromRod);
}

🌳 Recursion Tree (Example for N = 3)
T(3, 1→3, 2)
 ├── T(2, 1→2, 3)
 │    ├── T(1, 1→3, 2)  → move disk 1 from 1→3
 │    ├── move disk 2 from 1→2
 │    └── T(1, 3→2, 1)  → move disk 1 from 3→2
 ├── move disk 3 from 1→3
 └── T(2, 2→3, 1)
      ├── T(1, 2→1, 3)  → move disk 1 from 2→1
      ├── move disk 2 from 2→3
      └── T(1, 1→3, 2)  → move disk 1 from 1→3
```

**💭 Intuition Behind the Approach:**
- The key idea is that the largest disk can’t move until all smaller ones are out of its way.
- So:
  * Move N-1 smaller disks to the helper rod (temporary storage).
  * Move the largest disk directly to its destination.
  * Then move the smaller stack on top again.
- You’re essentially recursively clearing the path, moving the goal disk, and rebuilding — a perfect example of recursion mirroring real-world logic.
- Whenever you see:
  * f(n - 1, from, aux, to)
  * move n-th disk
  * f(n - 1, aux, to, from)
- → it’s a “move smaller stack → move big one → rebuild smaller stack” pattern.

**Complexity (Time & Space):**
- Recurrence: T(N) = 2T(N - 1) + 1
- Time: O(2^N)
- Space: O(N)   // recursion stack

### Approach 2: Iterative Approach (Using Bit Manipulation)

**Idea:**
- The iterative version simulates recursion using bit patterns.
- Each move follows a repeating pattern based on the parity (odd/even) of N.
- The smallest disk moves in a fixed direction (clockwise or counterclockwise).
- The remaining disks move following the only possible legal move at each step.

**Steps:**
- Calculate total moves = 2^N - 1.
- Label rods differently for odd and even N:
  * If N is odd → move smallest disk 1 → 3 → 2 → 1 (clockwise).
  * If N is even → move smallest disk 1 → 2 → 3 → 1 (counterclockwise).
- At every step:
  * Move the smallest disk as per pattern.
  * Then make the only possible legal move with the remaining disks.

**Java Code:**
```java
public static void towerOfHanoiIterative(int n, int fromRod, int toRod, int auxRod) {
    int totalMoves = (1 << n) - 1;  // 2^n - 1
    if (n % 2 == 0) {
        int temp = toRod;
        toRod = auxRod;
        auxRod = temp;
    }

    for (int i = 1; i <= totalMoves; i++) {
        if (i % 3 == 1) {
            moveDisk(fromRod, toRod, n);
        } else if (i % 3 == 2) {
            moveDisk(fromRod, auxRod, n);
        } else if (i % 3 == 0) {
            moveDisk(auxRod, toRod, n);
        }
    }
}

private static void moveDisk(int from, int to, int n) {
    System.out.println("move disk from rod " + from + " to rod " + to);
}
```

**💭 Intuition Behind the Approach:**
- In recursion, the sequence of moves is inherently determined by call order.
- But in the iterative version, you mimic the recursive structure using bitwise properties:
  * Each move number corresponds to a binary representation of disks.
  * The disk that moves on step i is the rightmost set bit in i.
- This works because each disk’s movement follows a power-of-two frequency.
- It’s a brilliant way to “flatten recursion” using mathematical patterns.

**Complexity (Time & Space):**
- Time: O(2^N)
- Space: O(1)

---

## 6. Justification / Proof of Optimality

- The recursive approach ensures minimum moves because each smaller disk must be moved twice per larger disk — once to clear space, once to rebuild.
- The total move count follows the recurrence T(N) = 2T(N - 1) + 1, which simplifies to 2^N - 1, proving optimality.
- The iterative version replicates the same transition pattern without recursion, maintaining identical move count and correctness.
- Therefore, both methods achieve the optimal and minimal sequence of moves in O(2^N) time and O(N) space.
---

## 7. Variants / Follow-Ups

- Count the number of moves only → return 2^N - 1.
- Solve for 4 or more rods → Frame–Stewart algorithm (optimal but complex).
- Visual simulation using recursion tree or iterative pattern.
---

## 8. Tips & Observations

- 2^N - 1 is the minimum possible moves — proven mathematically.
- The recursive version is best for clarity and learning recursion.
- The iterative version is great for optimization and understanding bit-level patterns.
- Always remember:
  * Odd N → move smallest disk clockwise.
  * Even N → move smallest disk counterclockwise.
---

<!-- #endregion -->
