<!-- #region 123-Design Browser History -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q123: Design Browser History</h1>

## 1. Problem Understanding

- We need to simulate a browser with:
  * A homepage
  * Ability to visit a new page (clears forward history)
  * Ability to go back X steps
  * Ability to go forward X steps
- This is a classic pointer-based history navigation problem.
---

## 2. Constraints

- 1 <= n <= 5000
- Each URL length ≤ 20
- Steps ≤ 100
---

## 3. Edge Cases

- Back beyond history → stop at earliest page
- Forward beyond history → stop at latest page
- Visiting a new URL clears forward history
---

## 4. Examples

```text
Example 1
Input

google.com
5
visit facebook.com
visit linkedin.com
back 2
forward 1
visit gmail.com
Output

google.com
facebook.com
Explanation

We are on google.com, we move to facebook.com then linkedin.com, when we go back 2 we come back to google.com therefore first string is google.com.

When we move one forward we go to facebook.com therefore next output is facebook.com

Example 2
Input

google.com
5
visit facebook.com
back 3
visit linkedin.com
back 1
forward 1
Output

google.com
google.com
linkedin.com
Explanation

We are on google.com, we move to facebook.com then go back at max 2 steps which is google.com. Then go to linkedin.com and 1 step back again which is google.com and then one step forward which is linkedin.com.
```

---

## 5. Approaches

### Approach 1: Using Two Stacks

**Idea:**
- Maintain:
  * A back stack
  * A forward stack
  * A current page
- Visiting a new page:
  * Push current into back stack
  * Clear forward stack
- Back operation:
  * Move from back stack → forward stack
- Forward operation:
  * Move from forward stack → back stack

**Steps:**
- Initialize homepage as current.
- For visit(url):
  * Push current into back
  * Set current = url
  * Clear forward
- For back(steps):
  * While steps > 0 and back stack not empty:
    * Push current → forward
    * Pop from back → current
- Similar for forward.

**Java Code:**
```java
class BrowserHistory {
    Stack<String> back;
    Stack<String> forward;
    String current;

    public BrowserHistory(String homepage) {
        back = new Stack<>();
        forward = new Stack<>();
        current = homepage;
    }

    public void visit(String url) {
        back.push(current);
        current = url;
        forward.clear();
    }

    public String back(int steps) {
        while (steps > 0 && !back.isEmpty()) {
            forward.push(current);
            current = back.pop();
            steps--;
        }
        return current;
    }

    public String forward(int steps) {
        while (steps > 0 && !forward.isEmpty()) {
            back.push(current);
            current = forward.pop();
            steps--;
        }
        return current;
    }
}
```

**💭 Intuition Behind the Approach:**
- A browser's behavior naturally follows a stack pattern:
  * You push pages as you visit them.
  * When you go back, you "undo" by popping from the back stack.
  * The undone states form the forward stack.
  * Visiting a new page deletes all forward states — exactly like a stack reset.
- So two stacks model the real browser perfectly.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * Every operation (visit, back, forward) is:
  * O(steps) for back/forward but each URL moves stacks only once.
  * Why?
    * Each page is pushed/popped at most once into each stack.
    * Total ops ≤ 5000, so efficient.
- 💾 Space Complexity
  * O(n)
  * Why?
    * Storing the history (max N URLs).

### Approach 2: Using ArrayList + Pointer

**Idea:**
- Maintain:
  * A dynamic list storing all pages visited in chronological order.
  * A pointer idx pointing to the current page.
  * Visiting removes all elements after idx.

**Steps:**
- Homepage added at index 0
- Visiting new URL:
  * Remove all URLs after idx
  * Add the new page
  * Move index to last
- Back:
  * Move pointer left steps or until 0
- Forward:
  * Move pointer right steps or until last index

**Java Code:**
```java
class BrowserHistory {
    List<String> history;
    int idx;

    public BrowserHistory(String homepage) {
        history = new ArrayList<>();
        history.add(homepage);
        idx = 0;
    }

    public void visit(String url) {
        while (history.size() > idx + 1) {
            history.remove(history.size() - 1);
        }
        history.add(url);
        idx++;
    }

    public String back(int steps) {
        idx = Math.max(0, idx - steps);
        return history.get(idx);
    }

    public String forward(int steps) {
        idx = Math.min(history.size() - 1, idx + steps);
        return history.get(idx);
    }
}
```

**💭 Intuition Behind the Approach:**
- We simulate history using a dynamic list.
- The pointer acts like a cursor:
  * Moving back/forward adjusts pointer.
  * Visiting new page trims all forward entries — just like real browser.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * visit: O(n) for removals
  * back/forward: O(1)
  * Why?
    * Removing from end is fast but worst-case visit might delete many entries.
- 💾 Space Complexity
  * O(n) history storage.

### Approach 3: Doubly Linked List (MOST REALISTIC)

**Idea:**
- Use a custom doubly linked list node:
- prev <— current —> next
- Browser actions map perfectly:
  * back → move to prev
  * forward → move to next
  * visit → delete all nodes after current and append a new one

**Steps:**
- Build a node for homepage
- Current pointer = homepage
- Visit:
  * Clear current.next
  * Link new node
- Back/Forward:
  * Move using prev and next

**Java Code:**
```java
class BrowserHistory {

    class Node {
        String url;
        Node prev, next;
        Node(String url) {
            this.url = url;
        }
    }

    Node curr;

    public BrowserHistory(String homepage) {
        curr = new Node(homepage);
    }

    public void visit(String url) {
        Node newNode = new Node(url);
        curr.next = null;
        newNode.prev = curr;
        curr.next = newNode;
        curr = newNode;
    }

    public String back(int steps) {
        while (steps > 0 && curr.prev != null) {
            curr = curr.prev;
            steps--;
        }
        return curr.url;
    }

    public String forward(int steps) {
        while (steps > 0 && curr.next != null) {
            curr = curr.next;
            steps--;
        }
        return curr.url;
    }
}
```

**💭 Intuition Behind the Approach:**
- A doubly linked list exactly matches the browser model:
  * Each page is a node
  * Back/forward are simple pointer moves
  * Visiting a new page discards the entire forward chain
- It’s clean, memory-efficient, and intuitive.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * visit: O(1) → link a new node
  * back/forward: O(steps) → move pointer
  * Why?
    * Doubly linked list allows O(1) pointer updates.
- 💾 Space Complexity
  * O(n) → nodes for each visited page

---

## 6. Justification / Proof of Optimality

- The Linked List approach most closely matches real browser behavior:
  * True back/forward navigations
  * No expensive deletions
  * O(1) visit
  * Clean pointer movement
- But stack implementation is also widely accepted due to simplicity.
---

## 7. Variants / Follow-Ups

- Add multi-tab browsing
- Add "close tab" and "restore closed tab"
- Add timestamps
- Add bookmarking
---

## 8. Tips & Observations

- LL approach is best for simulation
- Stacks are best for interview speed
- ArrayList is easiest but has deletion overhead
- Visiting a new page always clears forward history
---

<!-- #endregion -->
