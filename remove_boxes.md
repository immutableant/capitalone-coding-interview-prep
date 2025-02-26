# Remove Boxes

## Problem Statement
You are given several boxes with different colors represented by **different positive numbers**.

You may remove boxes in **several rounds** until there are **no boxes left**. Each time you can choose a **continuous sequence of boxes with the same color** (at least one box) and remove them to earn points.

### **Scoring Rule**
If you remove `k` consecutive boxes of the same color, you earn **`k * k` points**.

Return the **maximum points** you can get.

---

### Example Cases
#### Example 1
**Input:**
```python
boxes = [1,3,2,2,2,3,4,3,1]
```
**Output:**
```python
23
```
**Explanation:**
```
[1, 3, 2, 2, 2, 3, 4, 3, 1]  
----> [1, 3, 3, 4, 3, 1] (3*3=9 points)  
----> [1, 3, 3, 3, 1] (1*1=1 points)  
----> [1, 1] (3*3=9 points)  
----> [] (2*2=4 points)  
Total: 23 points
```

#### Example 2
**Input:**
```python
boxes = [1,1,1]
```
**Output:**
```python
9
```

#### Example 3
**Input:**
```python
boxes = [1]
```
**Output:**
```python
1
```

---

## Solution Approach
### **Dynamic Programming with Memoization (O(N³) Solution)**
Since removing boxes **affects future moves**, a **greedy approach won't work**. Instead, we use **Dynamic Programming (DP)**:

1. **Define `dp(l, r, k)`:**
   - `l, r` represent the **subarray bounds** in `boxes`.
   - `k` represents the **extra boxes of the same color** to the left of `l`.
   - The function returns the **maximum points possible**.

2. **State Transition:**
   - Remove the contiguous boxes of the same color first (`boxes[l]`).
   - Try merging adjacent boxes with the same color (`boxes[m] == boxes[l]`).
   - Use **recursion + memoization** to avoid recomputation.

3. **Base Case:**
   - If `l > r`, return `0` (no boxes left).
   
This approach ensures **O(N³) time complexity**, making it feasible for `N ≤ 100`.

---

## Why is Memoization Important?
Memoization **stores previously computed results** to avoid redundant calculations. In this problem:
- **Recursive DP without memoization** leads to **exponential time complexity** (`O(3^N)`) due to repeated subproblems.
- **Memoization reduces the complexity** to **O(N³)** by storing already computed values and reusing them.
- The `@lru_cache(None)` decorator in Python **automatically caches results**, preventing unnecessary recomputation.

### **Example of Overlapping Subproblems**
Without memoization:
```
dp(0, 5, 0) calls dp(1, 5, 0) and dp(0, 4, 1)
dp(1, 5, 0) calls dp(2, 5, 0) and dp(1, 4, 1)
```
- The **same `dp(x, y, k)` calls** happen multiple times, leading to inefficiency.
- **With memoization**, each unique `(l, r, k)` state is computed **only once**.

---

## Optimized Python Solution
```python
from typing import List
from functools import lru_cache

class Solution:
    def removeBoxes(self, boxes: List[int]) -> int:
        """
        Solves the problem using Dynamic Programming with memoization.
        """
        @lru_cache(None)  # Memoization to store computed states
        def dp(l: int, r: int, k: int) -> int:
            """
            Computes the max score possible for a given subarray (l, r) with k extra same-colored boxes to the left.
            """
            if l > r:
                return 0  # No boxes left
            
            # Extend the sequence of same-colored boxes
            while l < r and boxes[l] == boxes[l + 1]:
                l += 1
                k += 1
            
            # Remove the contiguous block first
            result = (k + 1) * (k + 1) + dp(l + 1, r, 0)
            
            # Try merging with other same-colored boxes
            for m in range(l + 1, r + 1):
                if boxes[m] == boxes[l]:
                    result = max(result, dp(l + 1, m - 1, 0) + dp(m, r, k + 1))
            
            return result
        
        return dp(0, len(boxes) - 1, 0)
```

---

## Complexity Analysis
- **Time Complexity:** `O(N³)`, since each state `(l, r, k)` takes `O(1)` time and there are `O(N³)` states.
- **Space Complexity:** `O(N³)`, due to memoization storing results for all possible `(l, r, k)` states.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `boxes = [1,1,1,1]` (all same color, should return `16`).
  - `boxes = [1,2,3,4,5]` (all different, should return `5`).
  - Large `N = 100` to test performance.

- **Common Mistakes:**
  - Trying to solve greedily (it **fails** for cases like `[1,3,2,2,2,3]`).
  - Forgetting to memoize states (`O(3^N)` naive recursion is too slow).
  - Not handling `k` correctly when merging blocks.

- **Follow-up Questions:**
  - Can you optimize further using **iterative DP** instead of recursion?
  - What if you can only remove **at most `K` boxes** in one operation?
  - Can you solve this problem using **Bitmask DP**?
