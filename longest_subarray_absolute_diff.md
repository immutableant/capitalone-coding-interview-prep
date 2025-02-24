# Longest Continuous Subarray With Absolute Diff ≤ Limit

## Problem Statement
Given an array of integers `nums` and an integer `limit`, return the size of the **longest non-empty subarray** such that the **absolute difference** between any two elements in this subarray is **less than or equal to** `limit`.

---

### Example Cases
#### Example 1
**Input:**
```python
nums = [8,2,4,7], limit = 4
```
**Output:**
```python
2
```
**Explanation:**
- The longest valid subarray is `[2,4]` with max difference `|2 - 4| = 2 ≤ 4`.
- Subarrays like `[8,2]` and `[8,2,4]` exceed the limit.

#### Example 2
**Input:**
```python
nums = [10,1,2,4,7,2], limit = 5
```
**Output:**
```python
4
```
**Explanation:**
- The longest valid subarray is `[2,4,7,2]` where max difference `|2 - 7| = 5 ≤ 5`.

#### Example 3
**Input:**
```python
nums = [4,2,2,2,4,4,2,2], limit = 0
```
**Output:**
```python
3
```
**Explanation:**
- The longest valid subarray is `[2,2,2]` where max difference is `0 ≤ 0`.

### Constraints
- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^9`
- `0 <= limit <= 10^9`

---

## Solution Approach
This problem requires finding the **longest valid subarray** where:
- `max(subarray) - min(subarray) ≤ limit`
- We need an efficient way to track the **min and max** values dynamically.

### **Sliding Window with Deques (Optimal Solution)**
1. Use a **sliding window** approach by maintaining a **left pointer (`left`)**.
2. Use two **monotonic deques**:
   - `max_deque`: Stores elements in **decreasing order** (helps track max in window).
   - `min_deque`: Stores elements in **increasing order** (helps track min in window).
3. **Expand the window (`right` pointer moves forward)**:
   - Insert elements into the deques while maintaining order.
4. **If window violates `max - min > limit`**, shrink it by **moving `left` pointer**.
5. **Track the maximum window size** encountered.

This approach runs in **O(N) time** since each element is processed at most twice (inserted and removed from deques).

---

## Optimized Python Solution
```python
from collections import deque
from typing import List

class Solution:
    def longestSubarray(self, nums: List[int], limit: int) -> int:
        """
        Finds the length of the longest subarray where max difference ≤ limit.
        """
        max_deque = deque()  # Stores decreasing elements (max tracker)
        min_deque = deque()  # Stores increasing elements (min tracker)
        left = 0  # Left pointer of the sliding window
        max_length = 0  # Track the longest valid subarray
        
        # In Python, enumerate() is a built-in function that adds a counter to an iterable and returns it as an enumerate object. This object yields pairs of the form (index, element), where index is the position of the element in the iterable, starting from 0 by default, and element is the value of the element itself.
        for right, num in enumerate(nums):
            # Maintain max_deque (track decreasing max values)
            while max_deque and max_deque[-1] < num:
                max_deque.pop()
            max_deque.append(num)
            
            # Maintain min_deque (track increasing min values)
            while min_deque and min_deque[-1] > num:
                min_deque.pop()
            min_deque.append(num)
            
            # If max - min > limit, shrink window by moving left pointer
            while max_deque[0] - min_deque[0] > limit:
                if nums[left] == max_deque[0]:
                    max_deque.popleft()
                if nums[left] == min_deque[0]:
                    min_deque.popleft()
                left += 1  # Shrink window
            
            # Update max_length with the valid window size
            max_length = max(max_length, right - left + 1)
        
        return max_length
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, since each element is inserted and removed from the deques at most once.
- **Space Complexity:** `O(N)`, due to the storage of elements in the deques.

---

## Interview Tips
- **Edge Cases to Consider:**
  - Single-element arrays (`nums = [5]` should return `1`).
  - `limit = 0`, meaning all elements in a subarray must be equal.
  - Large values where `nums.length = 10^5`.
  - All elements are the same (`nums = [3,3,3,3]` should return `4`).

- **Common Mistakes:**
  - Not maintaining the **monotonic** property in deques (they should always be sorted).
  - Forgetting to **shrink the window** when `max - min > limit`, leading to incorrect results.
  - Using `O(N^2)` brute-force solutions instead of `O(N)` sliding window.

- **Follow-up Questions:**
  - How would you modify this approach if elements could be **added or removed dynamically**?
  - Can this be solved using a **Balanced BST (e.g., AVL Tree)** instead of deques?
  - How would you extend this approach to **2D matrices**?
