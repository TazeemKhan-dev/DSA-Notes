<!-- #region qq -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q: qq</h1>

## 1. Problem Statement

- You are given n piles of bananas, where the i-th pile contains piles[i] bananas.
- Koko can choose an integer eating speed k (bananas per hour).
- Each hour:
  * She selects one pile.
  * She eats k bananas from that pile.
  * If the pile has fewer than k bananas → she eats the whole pile and the rest of the hour is idle.
- The guards will return in h hours, and Koko wants to finish eating all the bananas before they return.
- You must return the minimum integer eating speed k such that Koko finishes all piles within h hours.
---

## 2. Problem Understanding

- This problem is asking:
  * What is the slowest speed k at which Koko can still finish all bananas within exactly h hours?
- Key details:
  * Koko eats from only one pile per hour.
  * If she eats k bananas per hour:
    * A pile of size p requires ceil(p / k) hours to finish.
  * Total hours needed is:
    * sum( ceil(piles[i] / k) ) for all i
  * As k increases:
    * Eating becomes faster.
    * The total hours needed decreases.
  * As k decreases:
    * Eating becomes slower.
    * The total hours increases.
- We need to find the minimum k such that:
  * totalHours(k) <= h
- This forms a monotonically decreasing function with respect to k,
- which makes the problem a classic Binary Search on Answer (BSOA) problem.
---

<!-- #endregion -->
