<!-- #region 227-Asteroid Collision -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q227: Asteroid Collision</h1>

## 1. Problem Statement

- You are given an integer array asteroids representing asteroids in a row.
- The absolute value of asteroids[i] represents the size of the asteroid.
- The sign represents the direction:
  * Positive → moving right
  * Negative → moving left
- Asteroids collide based on the following rules:
  * If two asteroids of different sizes meet, the smaller one explodes
  * If two asteroids of equal sizes meet, both explode
  * Asteroids moving in the same direction never meet
- If no asteroid remains, print -1.
---

## 2. Problem Understanding

- Collision is possible only when:
  * A right-moving asteroid (+) is followed by a left-moving asteroid (-)
- We must simulate collisions in order
- Multiple collisions may occur for one asteroid
- This is a classic stack simulation problem.
---

## 3. Constraints

- 2 ≤ n ≤ 10^4
- -1000 ≤ asteroids[i] ≤ 1000
- asteroids[i] ≠ 0
- Efficient solution required → O(N)
---

## 4. Edge Cases

- All asteroids moving in same direction
- Chain collisions (one asteroid destroys multiple)
- Equal-sized collision
- Final result empty → print -1
---

## 5. Examples

```text
Input

3
5 10 -5


Output

5 10
```

---

## 6. Approaches

### Approach 1: Brute Force Simulation (Not Optimal)

**Idea:**
- Repeatedly scan the array and resolve one collision at a time until no more collisions are possible.
- This is slow but conceptually simple.

**Steps:**
- Convert array to list
- Scan from left to right
- If a[i] > 0 and a[i+1] < 0, resolve collision
- Modify list
- Restart scan
- Stop when no collision occurs

**Java Code:**
```java
public static List<Integer> asteroidCollisionBrute(int[] asteroids) {
    List<Integer> list = new ArrayList<>();
    for (int a : asteroids) list.add(a);

    boolean changed = true;

    while (changed) {
        changed = false;
        for (int i = 0; i < list.size() - 1; i++) {
            int a = list.get(i);
            int b = list.get(i + 1);

            if (a > 0 && b < 0) {
                if (Math.abs(a) > Math.abs(b)) {
                    list.remove(i + 1);
                } else if (Math.abs(a) < Math.abs(b)) {
                    list.remove(i);
                } else {
                    list.remove(i + 1);
                    list.remove(i);
                }
                changed = true;
                break;
            }
        }
    }

    return list.size() == 0 ? Arrays.asList(-1) : list;
}
```

**Complexity (Time & Space):**
- Time: O(N^2) — repeated scans
- Space: O(N) — list storage

### Approach 2: Stack Simulation (Optimal)

**Idea:**
- Use a stack to simulate collisions efficiently.
- Only collide when:
  * stack.peek() > 0 && current < 0

**Steps:**
- Initialize an empty stack
- Traverse asteroids from left to right
- For each asteroid a:
  * Assume it is alive
  * While:
    * stack is not empty
    * stack top is > 0
    * current asteroid a < 0
    * → collision is possible
- Compare sizes:
  * If |stack.peek()| < |a|
  * → stack top explodes (pop)
  * If |stack.peek()| == |a|
  * → both explode (pop + discard a)
  * If |stack.peek()| > |a|
  * → current asteroid explodes
- If current asteroid survives all collisions → push it into stack
- Stack contents after traversal represent final state

**Java Code:**
```java
public int[] asteroidCollision(int[] asteroids) {
    Stack<Integer> st = new Stack<>();

    for (int a : asteroids) {
        boolean alive = true;

        while (alive && !st.isEmpty() && st.peek() > 0 && a < 0) {
            if (Math.abs(st.peek()) < Math.abs(a)) {
                st.pop();
            } else if (Math.abs(st.peek()) == Math.abs(a)) {
                st.pop();
                alive = false;
            } else {
                alive = false;
            }
        }

        if (alive) st.push(a);
    }

    if (st.isEmpty()) return new int[]{-1};

    int[] res = new int[st.size()];
    for (int i = st.size() - 1; i >= 0; i--) {
        res[i] = st.pop();
    }
    return res;
}
```

**💭 Intuition Behind the Approach:**
- Stack stores asteroids that haven’t collided yet
- Only the nearest previous asteroid can collide with the current one
- Right-moving asteroids wait on the stack
- Left-moving asteroids may cause chain reactions
- Each asteroid is:
  * pushed once
  * popped at most once
- This guarantees linear processing.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Time: O(N) —
  * Each asteroid is pushed and popped at most once, so total operations are linear.
- 💾 Space Complexity
  * Space: O(N) —
  * Stack can store all asteroids in the worst case (all moving in same direction).

### Approach 3: Push-Then-Resolve Stack

**Idea:**
- Push current asteroid first
- Then repeatedly check top two elements
- Resolve collision immediately if needed

**Java Code:**
```java
public static List<Integer> asteroidCollision(int[] asteroids) {
    Stack<Integer> s = new Stack<>();

    for (int z : asteroids) {
        s.push(z);

        while (s.size() > 1) {
            int as2 = s.pop();
            int as1 = s.pop();

            if (!(as1 > 0 && as2 < 0)) {
                s.push(as1);
                s.push(as2);
                break;
            } else {
                int m = Math.abs(as1);
                int n = Math.abs(as2);

                if (m == n) {
                    break; // both destroyed
                } else if (m < n) {
                    s.push(as2);
                } else {
                    s.push(as1);
                }
            }
        }
    }

    List<Integer> res = new ArrayList<>();
    while (!s.isEmpty()) res.add(s.pop());
    Collections.reverse(res);

    return res.size() == 0 ? Arrays.asList(-1) : res;
}
```

**💭 Intuition Behind the Approach:**
- Stack always stores valid asteroids
- Collision is checked only for adjacent top elements
- Ensures nearest collision first
- Same O(N) guarantee

**Complexity (Time & Space):**
- Time: O(N) — each asteroid pushed/popped once
- Space: O(N) — stack storage

---

## 7. Justification / Proof of Optimality

- Brute force helps understand collisions
- Stack optimizes collision handling
- Your solution is a clean alternate stack formulation
---

## 8. Variants / Follow-Ups

- Circular asteroid collision
- Asteroids with velocity
- 2D collision simulation
---

## 9. Tips & Observations

- Collision condition is direction-based
- Always compare absolute values
- Chain collisions are the tricky part
---

## 10. Pitfalls

- Missing equal-size case
- Allowing same-direction collision
- Forgetting -1 when empty
---

<!-- #endregion -->
