<!-- #region Bitmasking(Used in q-55) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">QBitmasking(Used in q: 55)</h1>

## 1. Problem Understanding

- Bitmasking is a technique that uses bits inside an integer to represent multiple true/false states compactly.
- Each bit in an integer acts like a flag — 1 means "set/present", 0 means "unset/absent".
- It allows efficient manipulation and checking of states using bitwise operations.

- **Why Use Bitmasking**
    - Saves memory — stores 32 (or 64) flags in a single integer.
    - Bitwise operations are extremely fast — each runs in O(1).
    - Ideal for problems involving:
       * Presence or absence of items (like letters in a string).
       * Subset generation and combinatorial search.
       * State tracking in dynamic programming (DP).
       * Representing visited/unvisited states in graphs.
       * Encoding multiple binary states efficiently.

- **Bitwise Operators**
    - & (AND) → Result bit is 1 only if both bits are 1.
    - | (OR) → Result bit is 1 if any of the bits is 1.
    - ^ (XOR) → Result bit is 1 if bits are different.
    - ~ (NOT) → Inverts every bit (1 becomes 0, 0 becomes 1).
    - << (Left Shift) → Shifts bits left → multiplies number by 2 for each shift.
    - >> (Right Shift) → Shifts bits right → divides number by 2 for each shift.

- **Common Bitmask Operations**
    - Set a bit (turn ON) → mask |= (1 << i) → Sets the i-th bit to 1.
    - Unset a bit (turn OFF) → mask &= ~(1 << i) → Sets the i-th bit to 0.
    - Toggle a bit → mask ^= (1 << i) → Flips the i-th bit (1 → 0 or 0 → 1).
    - Check if bit is set → (mask & (1 << i)) != 0 → Returns true if the i-th bit is 1.
    - Count bits that are ON → Integer.bitCount(mask) → Gives how many bits are 1 in total.

- **Bitmask Expressions Explained**
    - mask |= (1 << (ch - 'a'))
      * Purpose: Marks a letter as seen in the bitmask.
      * Step-by-step:
        * ch - 'a' → Converts the letter 'a'–'z' to index 0–25.
        * Example: 'a' - 'a' = 0, 'b' - 'a' = 1, … 'z' - 'a' = 25.
        * 1 << (ch - 'a') → Creates a number where only that bit is 1.
       * Example: 'c' → 1 << 2 → 000...0100 (3rd bit ON).
        * mask |= … → Performs bitwise OR with the current mask.
        * Turns ON that bit without affecting other bits.
      * Effect: Marks the letter as “present” in the mask.
    - mask == (1 << 26) - 1
      * Purpose: Checks if all 26 letters are present.
      * Step-by-step:
        * 1 << 26 → Creates a number with 1 followed by 26 zeros in binary: 100…0 (27 bits).
        * (1 << 26) - 1 → Subtract 1 → all 26 bits become 1: 111…1 (binary with 26 ones).
        * mask == (1 << 26) - 1 → Compares current mask with “all letters present” mask.
          * If equal → all letters 'a'–'z' seen.
          * If not equal → some letters missing.

- **Example — Pangram Problem**
    - Each alphabet letter corresponds to one bit (26 total).
    - When a letter appears, turn ON its bit: mask |= (1 << (ch - 'a')).
    - After processing all characters, check if all 26 bits are ON: mask == (1 << 26) - 1.
    - If true → pangram; else → not pangram.

- **Where Bitmasking is Used**
    - Pangram check (presence of 26 letters).
    - Subset generation (for (int mask = 0; mask < (1 << n); mask++)).
    - Traveling Salesman Problem (TSP) and DP on subsets.
    - Counting pairs or combinations efficiently.
    - Representing visited states in graph search.
    - Problems involving toggling or tracking multiple binary choices.
---

<!-- #endregion -->
