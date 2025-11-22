<!-- #region printf() Formatting -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: printf() Formatting</h1>

## 1. Problem Understanding

- Syntax: System.out.printf("format_string", var1, var2, …);
- Each % symbol in the format string corresponds to one variable after the comma.
- The order of placeholders and variables must match.
- If the type doesn’t match the format specifier, Java throws an IllegalFormatConversionException.

- **Common Format Specifiers**
    - %d → integer
    - %f → floating-point (double/float)
    - %s → string
    - %c → character
    - %b → boolean
    - %n → newline (platform independent, better than \n)

- **Integer Formatting Rules (%d)**
    - %d → normal integer printing
    - %5d → prints integer in width 5 (right-aligned)
    - %-5d → prints integer in width 5 (left-aligned)
    - %05d → width 5, padded with zeros on the left
    - %,d → prints number with commas (e.g., 12,345)
    - %+d → shows sign (e.g., +45)
    - %(d → encloses negative numbers in parentheses (e.g., (45))
    - %x → print integer in hexadecimal
    - %o → print integer in octal
      * 🧩 Example:
        * System.out.printf("|%5d|%-5d|%05d|", 42, 42, 42);
        * Output:
          * | 42|42 |00042|

- **Floating-Point Formatting (%f, %e, %g)**
    - %f → prints floating-point numbers (default 6 digits after decimal)
    - %.2f → 2 digits after decimal
    - %8.3f → width 8, 3 digits after decimal
    - %e → scientific notation (e.g., 3.14e+00)
    - %g → automatically picks shortest representation
    - 🧩 Example:
      * System.out.printf("%.2f %8.3f %e", 3.14159, 3.14159, 3.14159);
      * Output:
      * 3.14 3.142 3.141590e+00

- **String Formatting (%s)**
    - %s → normal string
    - %20s → right-aligned in width 20
    - %-20s → left-aligned in width 20
    - %.5s → prints only first 5 characters of string
      * 🧩 Example:
      * System.out.printf("|%10s|%-10s|%.3s|", "Java", "Code", "Learning");
      * Output:
      * | Java|Code |Lea|

- **Character Formatting (%c)**
    - Prints a single character
    - You can use integer values (ASCII codes) to print characters
      * 🧩 Example:
      * System.out.printf("%c %c", 'A', 66);
      * Output:
      * A B

- **Boolean Formatting (%b)**
    - %b → prints true or false
    - If the variable is null, it prints false
      * 🧩 Example:
      * System.out.printf("%b %b", true, null);
      * Output:
      * true false

- **Date and Time (%t)**
    - %t or %T → used for date/time formatting
      * Example placeholders:
        * %tY → year (e.g., 2025)
        * %tm → month (e.g., 10)
        * %td → day (e.g., 18)
        * %tH:%tM:%tS → hour:minute:second

- **Combining Multiple Placeholders**
    - You can print multiple variables in a single statement.
      * Example:
      * int hour = 5;
      * String minutes = "09";
      * String seconds = "45";
      * System.out.printf("%02d:%s:%s", hour, minutes, seconds);
      * Output:
      * 05:09:45
    - %02d → width 2, padded with zeros (ensures double-digit hours like 05)
---

## 2. Tips & Observations

- %n is better than \n because it works across all operating systems.
- Always match the data type with the format specifier.
- You can combine alignment, width, and precision in one format specifier.
- Avoid mixing System.out.print() and printf() for same-line formatting (they handle buffers differently).
- You can use String.format() with the same rules to store formatted output in a string.
---

<!-- #endregion -->
