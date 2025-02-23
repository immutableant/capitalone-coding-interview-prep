# Rotating the Box

## Problem Statement
You are given an `m x n` matrix of characters `boxGrid` representing a side-view of a box. Each cell of the box is one of the following:
- A stone `'#'`
- A stationary obstacle `'*'`
- Empty `'.'`

The box is rotated **90 degrees clockwise**, causing some of the stones to **fall due to gravity**. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box.

Return an `n x m` matrix representing the box after rotation.

### Example Cases
#### Example 1
**Input:**
```python
boxGrid = [["#",".","#"]]
```
**Output:**
```python
[["."],
 ["#"],
 ["#"]]
```

#### Example 2
**Input:**
```python
boxGrid = [["#",".","*","."],
           ["#","#","*","."]]
```
**Output:**
```python
[["#","."],
 ["#","#"],
 ["*","*"],
 [".","."]]
```

#### Example 3
**Input:**
```python
boxGrid = [["#","#","*",".","*","."],
           ["#","#","#","*",".","."],
           ["#","#","#",".","#","."]]
```
**Output:**
```python
[[".","#","#"],
 [".","#","#"],
 ["#","#","*"],
 ["#","*","."],
 ["#",".","*"],
 ["#",".","."]]
```

### Constraints
- `m == boxGrid.length`
- `n == boxGrid[i].length`
- `1 <= m, n <= 500`
- `boxGrid[i][j]` is either `'#'`, `'*'`, or `'.'`.

---

## Solution Approach

To solve this problem, we need to:
1. **Simulate gravity**: Iterate from bottom to top in each row and shift `'#'` stones downward until they hit an obstacle `'*'` or another stone.
2. **Rotate the matrix 90 degrees clockwise**: After adjusting the stones' positions, convert the box into the new rotated form.

### **Algorithm Steps:**
1. Iterate through each row and simulate falling stones by using a two-pointer approach.
2. Rotate the adjusted matrix 90 degrees clockwise by transforming rows into columns.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def rotateTheBox(self, boxGrid: List[List[str]]) -> List[List[str]]:
        """
        Rotates the box 90 degrees clockwise while simulating gravity for falling stones.
        """
        m, n = len(boxGrid), len(boxGrid[0])
        
        # Simulate gravity for each row
        for row in boxGrid:
            empty_pos = len(row) - 1  # Position where the next stone should fall
            for i in range(len(row) - 1, -1, -1):
                if row[i] == '#':
                    row[i], row[empty_pos] = row[empty_pos], row[i]
                    empty_pos -= 1
                elif row[i] == '*':
                    empty_pos = i - 1  # Stones cannot fall past obstacles
        
        # Rotate the box 90 degrees clockwise
        rotated = [[boxGrid[m - 1 - i][j] for i in range(m)] for j in range(n)]
        
        return rotated

# Example Usage
solution = Solution()
print(solution.rotateTheBox([["#",".","#"],
                             ["#","#","."]]))
```

---

## Complexity Analysis
- **Time Complexity:** `O(m * n)`, as each cell is visited once for gravity simulation and once for rotation.
- **Space Complexity:** `O(m * n)`, since we store the rotated matrix separately.

---

## Interview Tips
- **Edge Cases to Consider:**
  - Single row or single column inputs.
  - Multiple obstacles `'*'` blocking stone movements.
  - Large values of `m, n` (ensure efficiency).

- **Common Mistakes:**
  - Forgetting to reset `empty_pos` when an obstacle is encountered.
  - Misplacing indexes during the 90-degree rotation.

- **Follow-up Questions:**
  - How would you optimize this for in-place rotation?
  - Can you handle different gravity directions instead of just falling down?
