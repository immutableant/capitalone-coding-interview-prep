# Spiral Matrix

## Problem Statement
Given an `m x n` matrix, return all elements of the matrix in **spiral order**.

### Example Cases
#### Example 1
**Input:**
```python
matrix = [[1,2,3],
          [4,5,6],
          [7,8,9]]
```
**Output:**
```python
[1,2,3,6,9,8,7,4,5]
```

#### Example 2
**Input:**
```python
matrix = [[1,2,3,4],
          [5,6,7,8],
          [9,10,11,12]]
```
**Output:**
```python
[1,2,3,4,8,12,11,10,9,5,6,7]
```

### Constraints
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

---

## Solution Approach

To extract elements in a **spiral order**, we:
1. Traverse from **left to right** across the top row.
2. Traverse **top to bottom** along the rightmost column.
3. Traverse from **right to left** across the bottom row (if any rows remain).
4. Traverse **bottom to top** along the leftmost column (if any columns remain).
5. Repeat until all elements are visited.

This approach effectively shrinks the boundary of the matrix layer by layer.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        """
        Returns all elements of the given matrix in spiral order.
        """
        if not matrix or not matrix[0]:
            return []  # Return empty if the matrix is empty
        
        result = []
        # Define boundaries for the unprocessed portion of the matrix
        top, bottom, left, right = 0, len(matrix) - 1, 0, len(matrix[0]) - 1
        
        while top <= bottom and left <= right:
            # Traverse from left to right along the top row
            for col in range(left, right + 1):
                result.append(matrix[top][col])
            top += 1  # Move top boundary downward to exclude the processed row
            
            # Traverse from top to bottom along the rightmost column
            for row in range(top, bottom + 1):
                result.append(matrix[row][right])
            right -= 1  # Move right boundary leftward to exclude the processed column
            
            if top <= bottom:
                # Traverse from right to left along the bottom row
                for col in range(right, left - 1, -1):
                    result.append(matrix[bottom][col])
                bottom -= 1  # Move bottom boundary upward to exclude the processed row
                
            if left <= right:
                # Traverse from bottom to top along the leftmost column
                for row in range(bottom, top - 1, -1):
                    result.append(matrix[row][left])
                left += 1  # Move left boundary rightward to exclude the processed column
                
        return result

# Example Usage
solution = Solution()
print(solution.spiralOrder([[1,2,3],[4,5,6],[7,8,9]]))  # Output: [1,2,3,6,9,8,7,4,5]
```

---

## Complexity Analysis
- **Time Complexity:** `O(m * n)`, since each element is visited exactly once.
- **Space Complexity:** `O(1)`, since the output list does not count towards extra space complexity.

---

## Interview Tips
- **Edge Cases to Consider:**
  - A single row or a single column.
  - A square vs. rectangular matrix.
  - Smallest case (`1x1` matrix).

- **Common Mistakes:**
  - Forgetting to update boundary conditions after each traversal.
  - Not handling empty matrices properly.
  - Incorrectly accessing indices when shrinking boundaries.

- **Follow-up Questions:**
  - How would you modify this to traverse in a counterclockwise spiral?
  - Can you implement this iteratively without modifying boundary variables?
