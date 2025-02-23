# Two Sum Less Than K

## Problem Statement
Given an array `nums` of integers and an integer `k`, return the **maximum sum** such that there exists `i < j` with `nums[i] + nums[j] = sum` and `sum < k`. If no such `i, j` exist, return `-1`.

### Example Cases
#### Example 1
**Input:**
```python
nums = [34,23,1,24,75,33,54,8]
k = 60
```
**Output:**
```python
58
```
**Explanation:** We can use `34` and `24` to sum `58`, which is less than `60`.

#### Example 2
**Input:**
```python
nums = [10,20,30]
k = 15
```
**Output:**
```python
-1
```
**Explanation:** No valid pairs satisfy the condition.

### Constraints
- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 1000`
- `1 <= k <= 2000`

---

## Solution Approach

The problem can be solved optimally using the **two-pointer approach** after sorting the array.

### **Algorithm Steps:**
1. **Sort** the array `nums` in ascending order.
2. Use two pointers:
   - `left` starts from the beginning.
   - `right` starts from the end.
3. While `left < right`:
   - Calculate the sum of `nums[left] + nums[right]`.
   - If the sum is **less than** `k`, update the max sum and move `left` forward to explore larger values.
   - If the sum is **greater than or equal to** `k`, move `right` backward to reduce the sum.
4. Return the maximum sum found, or `-1` if no valid sum exists.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def twoSumLessThanK(self, nums: List[int], k: int) -> int:
        """
        Finds the maximum sum of two numbers less than k.
        """
        nums.sort()  # Step 1: Sort the array
        left, right = 0, len(nums) - 1
        max_sum = -1  # Initialize max_sum as -1
        
        while left < right:
            current_sum = nums[left] + nums[right]  # Compute current sum
            
            if current_sum < k:
                max_sum = max(max_sum, current_sum)  # Update max_sum if valid
                left += 1  # Move left pointer forward to increase sum
            else:
                right -= 1  # Move right pointer backward to decrease sum
        
        return max_sum

# Example Usage
solution = Solution()
print(solution.twoSumLessThanK([34,23,1,24,75,33,54,8], 60))  # Output: 58
```

---

## Complexity Analysis
- **Sorting Complexity:** `O(n log n)` due to sorting `nums`.
- **Two-pointer Traversal:** `O(n)`, as each element is processed at most once.
- **Overall Complexity:** `O(n log n)`, making it efficient for `n ≤ 100`.

---

## Interview Tips
- **Edge Cases to Consider:**
  - No valid pairs exist (`nums = [10, 20, 30], k = 15`).
  - Smallest possible `nums.length = 1`.
  - `nums` already sorted vs. unsorted.
  - Large values for `nums[i]` near `1000`.

- **Common Mistakes:**
  - Forgetting to handle the case where no valid sum exists (`return -1`).
  - Incorrectly updating pointers, causing an infinite loop.
  - Not sorting the array before applying the two-pointer technique.

- **Follow-up Questions:**
  - How would you modify this to return **all** valid pairs instead of just the maximum sum?
  - Can you solve this problem in `O(n^2)` without sorting?
