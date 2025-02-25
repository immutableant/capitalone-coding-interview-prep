# Rotate Image

## Problem Statement
You are given an `n x n` **2D matrix** representing an image. Rotate the image **90 degrees clockwise**.

### **Constraints:**
- You **must rotate the image in-place**, meaning you **cannot allocate another 2D matrix** for rotation.
- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

---

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
[[7,4,1],
 [8,5,2],
 [9,6,3]]
```

#### Example 2
**Input:**
```python
matrix = [[5,1,9,11],
          [2,4,8,10],
          [13,3,6,7],
          [15,14,12,16]]
```
**Output:**
```python
[[15,13,2,5],
 [14,3,4,1],
 [12,6,8,9],
 [16,7,10,11]]
```

---

## Solution Approach
We can achieve the in-place rotation using **two steps**:

1. **Transpose the matrix** (convert rows to columns):
   - Swap `matrix[i][j]` with `matrix[j][i]` for all `i < j`.

2. **Reverse each row** (swap first and last columns):
   - Reverse each row to achieve the 90-degree clockwise rotation.

This approach ensures **O(N²) time complexity** (since we iterate over all elements) and **O(1) space complexity** (since we perform swaps in-place).

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Rotates the matrix 90 degrees clockwise in-place.
        """
        n = len(matrix)
        
        # Step 1: Transpose the matrix
        for i in range(n):
            for j in range(i + 1, n):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]  # Swap elements
        
        # Step 2: Reverse each row
        for row in matrix:
            row.reverse()
```

---

## Complexity Analysis
- **Time Complexity:** `O(N²)`, since we iterate over all elements.
- **Space Complexity:** `O(1)`, since we modify the matrix in-place.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `n = 1` (single-element matrix, remains unchanged).
  - `n = 2` (smallest matrix that requires swaps).
  - A **matrix with duplicate values** should still be rotated correctly.
  - **Negative values** should be handled properly.

- **Common Mistakes:**
  - Swapping elements incorrectly in the **transpose step**.
  - Forgetting to reverse rows after transposing.
  - Trying to use **extra space**, which violates constraints.

- **Follow-up Questions:**
  - How would you rotate the matrix **counterclockwise** instead?
  - What if the matrix was **not square (`m x n`)**? How would you rotate it?
  - Can you perform this **rotation efficiently for very large matrices**?
