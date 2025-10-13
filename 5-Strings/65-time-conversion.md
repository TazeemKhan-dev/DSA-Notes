<!-- #region 65-Time Conversion -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q65: Time Conversion</h1>

## 1. Problem Understanding

- You are given a time in 12-hour format (hh:mm:ssAM or hh:mm:ssPM).
- Your task is to convert it to 24-hour (military) format.
- Key Rules:
  * "12:00:00AM" → "00:00:00" (midnight)
  * "12:00:00PM" → "12:00:00" (noon)
  * For AM, hours 01–11 stay the same, except 12AM → 00.
  * For PM, hours 01–11 become 13–23.
---

## 2. Constraints

- s.length() == 10
- Time format is always valid: "hh:mm:ssAM" or "hh:mm:ssPM"
- → i.e. first 2 chars = hour, next 2 = minutes, next 2 = seconds, last 2 = AM/PM.
---

## 3. Edge Cases

- "12:00:00AM" → "00:00:00" ✅
- "12:00:00PM" → "12:00:00" ✅
- "01:00:00AM" → "01:00:00" ✅
- "01:00:00PM" → "13:00:00" ✅
- Works for all values between 01–12 hours.
---

## 4. Examples

```text
Example 1
Input:
07:05:45PM
Output:
19:05:45
Explanation:7 PM means 7 + 12 = 19, while minutes and seconds remain same.

Example 2
Input:
12:01:00PM
Output:
12:01:00
Explanation:12 PM → 12 (no change), minutes and seconds remain same.
```

---

## 5. Approaches

### Approach 1: String Parsing

**Idea:**
- Extract hour (hh), minutes (mm), seconds (ss), and meridian (AM/PM).
- Apply conversion rule:
  * If AM and hour == 12 → hour = 00
  * If PM and hour != 12 → hour += 12
- Combine back in "HH:MM:SS" format.

**Steps:**
- Read input string s.
- Split into:
  * hour = Integer.parseInt(s.substring(0, 2))
  * minute = s.substring(3, 5)
  * second = s.substring(6, 8)
  * ampm = s.substring(8, 10)
- Apply conversion rules.
- Print formatted 24-hour time.

**Java Code:**
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();

        int hour = Integer.parseInt(s.substring(0, 2));
        String minutes = s.substring(3, 5);
        String seconds = s.substring(6, 8);
        String meridian = s.substring(8, 10);

        if (meridian.equals("AM")) {
            if (hour == 12) hour = 0; // midnight case
        } else {
            if (hour != 12) hour += 12; // add 12 for PM hours
        }

        System.out.printf("%02d:%s:%s", hour, minutes, seconds);
    }
}
```

**Complexity (Time & Space):**
- Time Complexity: O(1) — only fixed string operations.
- Space Complexity: O(1) — only a few variables used.

---

## 6. Justification / Proof of Optimality

- This is the most direct and optimal solution — parsing the input and performing constant-time adjustments.
- No loops or data structures needed.
---

## 7. Variants / Follow-Ups

- Convert 24-hour → 12-hour format with AM/PM.
- Handle user input with missing leading zeros (e.g., "7:05:45PM").
- Implement using DateTime API (e.g., LocalTime.parse() in Java 8+).
---

## 8. Tips & Observations

- %02d ensures leading zeros for single-digit hours (like 07:00:00).
- Always separate logic for AM and PM to avoid mixing edge cases.
- Simple string slicing problems like this test your attention to detail and formatting precision.
---

<!-- #endregion -->
