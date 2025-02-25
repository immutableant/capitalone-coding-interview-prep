# Best Time to Buy and Sell Stock II

## Problem Statement
You are given an integer array `prices`, where `prices[i]` represents the price of a given stock on the `i-th` day.

Unlike the previous version of this problem, you are allowed to buy and sell **multiple times**.
- You can **only hold at most one share** at any time.
- You may buy and immediately sell on the **same day**.

Return the **maximum profit** you can achieve.

---

### Example Cases
#### Example 1
**Input:**
```python
prices = [7,1,5,3,6,4]
```
**Output:**
```python
7
```
**Explanation:**
- Buy on **day 2** (price = 1) → Sell on **day 3** (price = 5), profit = `5 - 1 = 4`
- Buy on **day 4** (price = 3) → Sell on **day 5** (price = 6), profit = `6 - 3 = 3`
- **Total profit: 4 + 3 = 7**

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
- **Total profit: 4**

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
- `0 <= prices[i] <= 10^4`

---

## Solution Approach
### **Greedy Strategy (O(N) Solution)**
The key observation is that **any increasing segment** in `prices` represents a potential profit. Instead of waiting for the global maximum, we can make **greedy decisions**:

1. Iterate through `prices` and track **profitable differences**.
2. If `prices[i] > prices[i-1]`, add the difference to `max_profit`.
3. This ensures we capture **all upward movements**, maximizing profit.

Since we are only making **one pass (O(N) time complexity)** and using **O(1) extra space**, this is optimal.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        Calculates the maximum profit by summing all increasing segments in the price list.
        """
        max_profit = 0  # Store total profit
        
        for i in range(1, len(prices)):
            if prices[i] > prices[i - 1]:
                # If today's price is higher than yesterday, sell for profit
                max_profit += prices[i] - prices[i - 1]
        
        return max_profit
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, since we iterate through `prices` once.
- **Space Complexity:** `O(1)`, as we use only a single variable (`max_profit`).

---

## Interview Tips
- **Edge Cases to Consider:**
  - `prices` contains only one day (`prices = [5]`) → Profit is `0`.
  - All prices are the same (`prices = [3,3,3,3]`) → Profit is `0`.
  - Prices are strictly decreasing (`prices = [10,9,8,7]`) → Profit is `0`.
  - A mix of increasing and decreasing values (`prices = [2,1,4,3,7,5]`) should maximize profit by making multiple transactions.

- **Common Mistakes:**
  - Waiting for a global maximum instead of **selling at local peaks**.
  - Forgetting to check `prices[i] > prices[i-1]` before making a transaction.
  - Using a **brute-force approach** (`O(N^2)`) instead of greedy (`O(N)`).

- **Follow-up Questions:**
  - What if you are allowed **at most K transactions**? (Use **Dynamic Programming**).
  - What if a **transaction fee** is applied for each buy-sell operation?
  - How would you modify this solution if you can **only hold at most 2 shares** at a time?
