# Largest Rectangle in Histogram

## Problem Statement
Given an array `heights` representing the **histogram's bar heights**, where the **width of each bar is 1**, return the **area of the largest rectangle** that can be formed within the histogram.

---

### Example Cases
#### Example 1
**Input:**
```python
heights = [2,1,5,6,2,3]
```
**Output:**
```python
10
```
**Explanation:**
- The largest rectangle is formed using the heights `[5,6]` with width `2`, giving an area of `5 × 2 = 10`.

#### Example 2
**Input:**
```python
heights = [2,4]
```
**Output:**
```python
4
```
**Explanation:**
- The largest rectangle is just the bar of height `4` with width `1`, giving an area of `4 × 1 = 4`.

### Constraints
- `1 <= heights.length <= 10^5`
- `0 <= heights[i] <= 10^4`

---

## Solution Approach
### **Optimized Stack-Based Approach (O(N))**
The **largest rectangle** at any index depends on the **nearest smaller bar to the left and right**.

1. **Use a Monotonic Stack**:
   - Store **indices** of histogram bars in increasing order.
   - When a **smaller height** is found, process the previous bars to calculate areas.

2. **Algorithm**:
   - Iterate through `heights`, pushing indices onto the stack if the height increases.
   - When a smaller height is encountered, **pop from the stack** and calculate the **largest possible area** for the removed height.
   - Continue until all elements are processed.

This ensures **O(N) time complexity** instead of the naive **O(N²) brute force** approach.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        """
        Finds the largest rectangular area in a histogram using a monotonic stack.
        """
        stack = []  # Stack to store indices of heights
        max_area = 0  # Maximum area found
        heights.append(0)  # Append a zero to process remaining elements
        
        for i, h in enumerate(heights):
            # Maintain increasing stack; process when a smaller element is found
            while stack and heights[stack[-1]] > h:
                height = heights[stack.pop()]  # Get the height of the rectangle
                width = i if not stack else i - stack[-1] - 1  # Calculate width
                max_area = max(max_area, height * width)  # Update max area
            
            stack.append(i)  # Push current index onto the stack
        
        return max_area
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, since each element is pushed and popped from the stack once.
- **Space Complexity:** `O(N)`, due to the storage of indices in the stack.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `heights = [0,0,0]` (All zero heights, should return `0`)
  - `heights = [1,1,1,1]` (All equal heights, should return `4`)
  - `heights = [10000, 10000, 10000]` (Large numbers, should handle efficiently)
  - `heights = [2,1,5,6,2,3]` (Typical case with varying heights)

- **Common Mistakes:**
  - Not handling the **remaining elements in the stack** after iteration.
  - Using a **brute-force O(N²) approach**, which is too slow.
  - Forgetting to **calculate width correctly** when popping elements.

- **Follow-up Questions:**
  - How would you modify this solution to handle **2D histograms (matrix representation)?**
  - Can you implement this using a **divide and conquer approach**?
  - How would you optimize this for **streaming data where heights arrive in real-time**?
