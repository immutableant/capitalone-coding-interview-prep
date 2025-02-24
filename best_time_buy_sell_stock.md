# Best Time to Buy and Sell Stock

## Problem Statement
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.

You want to **maximize your profit** by choosing a **single day to buy** one stock and choosing a **different day in the future to sell** that stock.

Return the **maximum profit** you can achieve from this transaction. If no profit is possible, return `0`.

---

### Example Cases
#### Example 1
**Input:**
```python
prices = [7,1,5,3,6,4]
```
**Output:**
```python
5
```
**Explanation:**
- Buy on **day 2** (`price = 1`) and sell on **day 5** (`price = 6`).
- Profit = `6 - 1 = 5`.
- **Cannot** sell before buying.

#### Example 2
**Input:**
```python
prices = [7,6,4,3,1]
```
**Output:**
```python
0
```
**Explanation:**
- Prices are continuously decreasing, so no transactions should be made.
- Maximum possible profit is `0`.

### Constraints
- `1 <= prices.length <= 10^5`
- `0 <= prices[i] <= 10^4`

---

## Solution Approach
This problem can be solved efficiently using a **single pass (O(N)) greedy approach**:

1. **Initialize** a `min_price` variable to track the lowest price encountered so far.
2. **Iterate** through `prices`, updating `min_price` and calculating `profit` at each step:
   - If the current price is lower than `min_price`, update `min_price`.
   - Otherwise, calculate profit by `current price - min_price` and update `max_profit`.
3. **Return the maximum profit** found.

This approach runs in **O(N) time** and **O(1) space**, making it optimal for large inputs.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        Finds the maximum profit from buying and selling a stock once.
        """
        min_price = float('inf')  # Track the lowest price seen so far
        max_profit = 0  # Track the maximum profit found
        
        for price in prices:
            if price < min_price:
                # Update min_price if a new lower price is found
                min_price = price
            else:
                # Calculate potential profit and update max_profit
                max_profit = max(max_profit, price - min_price)
        
        return max_profit
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, since we iterate through the list once.
- **Space Complexity:** `O(1)`, as we only use two variables.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `prices` contains only one day (`prices = [5]`) → Profit is `0`.
  - All prices are the same (`prices = [3,3,3,3]`) → Profit is `0`.
  - Prices are strictly decreasing (`prices = [10,9,8,7]`) → Profit is `0`.
  - Prices start high, dip low, and rise again (`prices = [7,1,5,3,6,4]`) → Should correctly identify min buy price.

- **Common Mistakes:**
  - Selling before buying (wrong index order).
  - Not updating `min_price` correctly when iterating.
  - Using `O(N^2)` brute force solutions instead of `O(N)` greedy approach.

- **Follow-up Questions:**
  - What if you can buy and sell **multiple times** instead of just once? (Dynamic Programming approach)
  - What if **transaction fees** are introduced? How would you modify your approach?
  - Can you solve this using **two pointers**?
