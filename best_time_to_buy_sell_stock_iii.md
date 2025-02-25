# Best Time to Buy and Sell Stock III

## Problem Statement
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.

Find the **maximum profit** you can achieve. You may complete at most **two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

---

### Example Cases
#### Example 1
**Input:**
```python
prices = [3,3,5,0,0,3,1,4]
```
**Output:**
```python
6
```
**Explanation:**
- Buy on **day 4** (price = 0) → Sell on **day 6** (price = 3), profit = `3 - 0 = 3`
- Buy on **day 7** (price = 1) → Sell on **day 8** (price = 4), profit = `4 - 1 = 3`
- **Total profit: 3 + 3 = 6**

#### Example 2
**Input:**
```python
prices = [1,2,3,4,5]
```
**Output:**
```python
4
```
**Explanation:**
- Buy on **day 1** (price = 1) → Sell on **day 5** (price = 5), profit = `5 - 1 = 4`

#### Example 3
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
- **Total profit: 0**

### Constraints
- `1 <= prices.length <= 10^5`
- `0 <= prices[i] <= 10^5`

---

## Solution Approach
Since we can perform at most **two transactions**, we need to track **four states**:
1. **First Buy (`buy1`)** → The lowest price paid for the first stock purchase.
2. **First Sell (`sell1`)** → The maximum profit after the first sell.
3. **Second Buy (`buy2`)** → The maximum profit after buying a second stock (using first sell profit).
4. **Second Sell (`sell2`)** → The maximum total profit after selling the second stock.

### **Optimized Dynamic Programming Approach (O(N))**
1. **Iterate through prices** and update `buy1`, `sell1`, `buy2`, and `sell2` dynamically.
2. **Update each variable greedily** to maximize profit.
3. **Return `sell2`**, which represents the best profit from two transactions.

This approach ensures **O(N) time complexity** with **O(1) space complexity**.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        Calculates the maximum profit from at most two stock transactions.
        """
        if not prices:
            return 0
        
        buy1, sell1 = float('-inf'), 0  # First transaction variables
        buy2, sell2 = float('-inf'), 0  # Second transaction variables
        
        for price in prices:
            # Track the lowest price to buy in the first transaction
            buy1 = max(buy1, -price)
            # Track the highest profit after selling once
            sell1 = max(sell1, buy1 + price)
            # Track the lowest effective price to buy in the second transaction
            buy2 = max(buy2, sell1 - price)
            # Track the highest profit after selling the second stock
            sell2 = max(sell2, buy2 + price)
        
        return sell2
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, since we iterate through `prices` once.
- **Space Complexity:** `O(1)`, as we use only a few variables.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `prices` contains only one day (`prices = [5]`) → Profit is `0`.
  - All prices are the same (`prices = [3,3,3,3]`) → Profit is `0`.
  - Prices are strictly decreasing (`prices = [10,9,8,7]`) → Profit is `0`.
  - A mix of increasing and decreasing values (`prices = [2,1,4,3,7,5]`) should maximize profit by making two transactions.

- **Common Mistakes:**
  - Selling before buying (wrong index order).
  - Not updating `buy1` and `buy2` correctly when iterating.
  - Using `O(N^2)` brute force solutions instead of `O(N)` dynamic programming.

- **Follow-up Questions:**
  - How would you modify this approach if you can **complete unlimited transactions**? (Greedy approach).
  - What if **transaction fees** are introduced? How would you modify the solution?
  - Can you solve this using **Dynamic Programming with a table** instead of tracking only four states?
