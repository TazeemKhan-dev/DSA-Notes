<!-- #region Java Comparator & Custom Sorting — Beginner → Advance  -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: Java Comparator & Custom Sorting — Beginner → Advance </h1>

## 1. Problem Understanding

- **Why Sorting Needs a Comparator?**
    - Java can sort primitives automatically (int[], double[]...).
    - But Java does NOT know how to sort:
      * Complex objects (Student, Pair, Node, Employee)
      * 2D arrays (int[][])
      * Lists with custom rules
      * Sorting by multiple keys
      * Descending order
    - So Java allows you to teach it how to compare two items.
    - This “teaching” is done using a Comparator.

- **What Comparator Actually Does (MOST IMPORTANT RULE)**
    - A Comparator must return an int:
      * Negative → a < b
      * Zero → a == b
      * Positive → a > b
    - ✔ What does “a < b” mean here?
      * It means a should come before b in the sorted order.
    - ✔ What does “a > b” mean?
      * It means b should come before a.
    - ✔ So final rule:
      * If return < 0 → a is placed before b
      * If return > 0 → b is placed before a
      * If return = 0 → order does not change

- **The MOST CONFUSING PART (cleared in simple words)**
    - When you write:
      * (a, b) -> a - b
    - This means:
      * If a < b
      * a - b is negative → a goes before b
      * If a > b
      * a - b is positive → b goes before a
      * If equal → order remains
    - Thus:
      * ✔ Negative return means: keep current order (a first)
      * ✔ Positive return means: swap them (put b first)

- **How Java Uses Comparator? (Internal Logic)**
    - You provide a function:
      * compare(a, b)
    - The sorting algorithm internally checks:
      * if(compare(a, b) > 0) {
          * swap(a, b);
      * }
    - This is the secret.

- **Simple Examples**
    - Sort ascending
      * (a, b) -> a - b
    - Sort descending
      * (a, b) -> b - a
    - Sort by first element of 2D array
      * (a, b) -> a[0] - b[0]
    - Sort by second element
      * (a, b) -> a[1] - b[1]
    - Sort by two keys (primary & secondary)
      * (a, b) -> {
          * if(a[0] != b[0]) return a[0] - b[0];
          * return a[1] - b[1];
      * }

- **Java Comparator — Full Theory**
    - ✔ Comparator is used when:
      * You cannot modify the class
      * You want different ways of sorting
      * You want sorting logic outside the class
    - ✔ Defined using lambda:
      * Comparator<Integer> comp = (a, b) -> a - b;
    - ✔ Or use directly inside sort:
      * Arrays.sort(arr, (a, b) -> a - b);

- **Java Custom Sorting (Theory)**
    - Custom sorting means:
      * You define your own comparison logic
      * Sorting happens according to your business rule
    - Examples:
      * Sort students by marks
      * Sort employees by salary then by age
      * Sort intervals by ending time
      * Sort pairs by second element
      * Sorting is ALWAYS controlled by your comparator.

- **Comparator vs Comparable (MOST ASKED INTERVIEW Q)**
    - ✔ Comparable
      * Used for natural sorting (default sort order)
      * Implemented inside the class itself
      * Class implements:
        * class Student implements Comparable<Student> {
            * public int compareTo(Student other) {
                * return this.marks - other.marks;
            * }
        * }
      * Used when the class decides how it should be sorted
    - ✔ Comparator
          * Sorting logic written outside the class
          * You can write multiple comparators for multiple sorting patterns
          * Flexibility is high
    - ✔ Simple difference summary (in bullets):
          * Comparable → sorting rule inside class (compareTo())
          * Comparator → sorting rule outside class (compare())
          * Comparable → only one rule
          * Comparator → infinite rules
          * Comparable → used by Collections.sort(list) or Arrays.sort() automatically
          * Comparator → must be explicitly passed

- **Java Arrays.sort with Comparator (Theory)**
    - Works for:
      * int[][]
      * Integer[]
      * custom objects
    - Signature:
      * Arrays.sort(arr, comparator);
    - Sorting 2D array example:
      * Arrays.sort(arr, (a, b) -> a[0] - b[0]);
    - Internal algorithm:
      * Uses TimSort for object arrays
      * Uses Dual Pivot QuickSort for primitive arrays

- **Java Collections.sort with Comparator (Theory)**
    - Used only for:
      * List<T> (ArrayList, LinkedList)
    - Syntax:
      * Collections.sort(list, comparator);
    - Example:
      * Collections.sort(students, (a, b) -> a.marks - b.marks);
    - Internally:
      * Uses TimSort
      * Fast and stable

- **Java Lambda Comparator (Theory)**
    - Lambda is just a shorter syntax for Comparator.
      * Without lambda (old style):
        * Collections.sort(list, new Comparator<Integer>() {
            * public int compare(Integer a, Integer b) {
                * return a - b;
            * }
        * });
      * With lambda:
        * Collections.sort(list, (a, b) -> a - b);
      * Lambda Rules:
        * (a, b) are parameters
        * -> means "using this rule"
        * Right side is return statement
      * Much cleaner and preferred.

- **Comparators & Custom Sorting — Overall Theory (High-Level)**
    - Comparator tells Java how to compare two items
    - Sorting algorithm repeatedly calls your comparator
    - You control sort order completely
    - Used heavily in:
      * DSA problems
      * sorting intervals
      * sorting pairs
      * sorting objects
      * greedy algorithms
      * scheduling problems
      * minimum platforms
      * merge intervals
      * activity selection
      * graph edges (Kruskal)
    - Custom sorting is a critical DSA skill.

- **The Full Logic Behind Comparison (Deep Theory)**
    - Every comparison function defines a strict weak ordering:
      * Transitive
      * Consistent
      * Antisymmetric
    - Meaning:
      * If compare(a, b) < 0 and compare(b, c) < 0, then compare(a, c) MUST also be < 0.
      * Otherwise sorting becomes unstable or incorrect.
    - This is why the comparator must be deterministic and consistent.

- **Real DSA Problem Examples**
    - You use custom comparators in:
      * Sort intervals by start
      * Sort intervals by end
      * Sort jobs by profit
      * Sort edges by weight (Kruskal)
      * Sort students by multiple criteria
      * Sort arrays of pairs
      * Sort frequency maps (descending order of frequency)
      * Sort characters by frequency in strings
      * Sort points by distance from origin
    - Sorting is core to many greedy and DP problems.

- **Conclusion (Everything You Need to Know)**
    - Comparator → flexible external custom sorting
    - Comparable → one fixed sorting inside the class
    - Arrays.sort → for arrays
    - Collections.sort → for lists
    - Lambda comparator → modern, cleaner syntax
    - Return negative → keep order
    - Return positive → swap
    - Custom sorting is essential in DSA
---

<!-- #endregion -->
