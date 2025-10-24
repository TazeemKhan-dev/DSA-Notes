<!-- #region 3-Recursion with Backtracking -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q3: Recursion with Backtracking</h1>

## 1. Problem Understanding

- **Recursion**
    - A function that calls itself to solve smaller subproblems.
    - Components:
      * Base case: Stops recursion.
      * Recursive case: Calls function with smaller input.
    - Usually computes one solution.
    - Examples: Factorial, Fibonacci, GCD.

- **Backtracking**
    - A special type of recursion for exploring all possible solutions.
    - Follows “try → check → undo” pattern.
    - Steps:
      * Choose a possibility.
      * Explore recursively.
      * Check if it leads to a valid solution.
      * Undo / Backtrack to try other possibilities.
    - Examples: N-Queens, Sudoku, Maze solving, Permutations/Combinations.

- **Key Differences between Recursion and Backtracking**
    - Recursion solves a problem; backtracking explores all solutions.
    - Recursion doesn’t need to undo choices; backtracking does.
    - Recursion often finds one solution, backtracking finds all valid solutions.

- **Characteristics of Backtracking**
    - Decision-making at each step.
    - Recursive exploration of choices.
    - Undoing invalid choices to explore alternatives.
    - Prunes invalid paths to save computation.

- **Backtracking Template:**
    - void backtrack(parameters) {
        * if (base_case_condition) {
            * // process solution
            * return;
        * }
        * for (each choice) {
            * if (choice is valid) {
                * make choice;
                * backtrack(next_state);
                * undo choice;  // backtrack
            * }
        * }
    - }

- **When to Use Backtracking**
    - Combinatorial problems: subsets, permutations, combinations.
    - Constraint satisfaction problems: Sudoku, N-Queens, crossword puzzles.
    - Path-finding problems: Maze solving, word search in a grid.

- **Important Notes:**
    - All backtracking problems are recursive, but not all recursion is backtracking.
    - Backtracking systematically explores all paths and prunes invalid branches.
---

<!-- #endregion -->
