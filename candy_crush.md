# Candy Crush

## Problem Statement
This problem involves implementing a basic **elimination algorithm** for Candy Crush.

Given an `m x n` integer array `board`, where `board[i][j]` represents the type of candy, we need to restore the board to a **stable state** following these rules:

1. If **three or more** candies of the same type are **adjacent vertically or horizontally**, crush them (set them to `0`).
2. After crushing, **candies above** fall down to fill empty spaces. No new candies appear from outside the grid.
3. Repeat the process **until no more candies can be crushed**.
4. Return the stabilized board.

### Example Cases
#### Example 1
**Input:**
```python
board = [
    [110,5,112,113,114],
    [210,211,5,213,214],
    [310,311,3,313,314],
    [410,411,412,5,414],
    [5,1,512,3,3],
    [610,4,1,613,614],
    [710,1,2,713,714],
    [810,1,2,1,1],
    [1,1,2,2,2],
    [4,1,4,4,1014]
]
```
**Output:**
```python
[
    [0,0,0,0,0],
    [0,0,0,0,0],
    [0,0,0,0,0],
    [110,0,0,0,114],
    [210,0,0,0,214],
    [310,0,0,113,314],
    [410,0,0,213,414],
    [610,211,112,313,614],
    [710,311,412,613,714],
    [810,411,512,713,1014]
]
```

#### Example 2
**Input:**
```python
board = [
    [1,3,5,5,2],
    [3,4,3,3,1],
    [3,2,4,5,2],
    [2,4,4,5,5],
    [1,4,4,1,1]
]
```
**Output:**
```python
[
    [1,3,0,0,0],
    [3,4,0,5,2],
    [3,2,0,3,1],
    [2,4,0,5,2],
    [1,4,3,1,1]
]
```

### Constraints
- `m == board.length`
- `n == board[i].length`
- `3 <= m, n <= 50`
- `1 <= board[i][j] <= 2000`

---

## Solution Approach

1. **Identify crushable candies** by scanning the board for consecutive **horizontal** or **vertical** matches.
2. **Mark candies for removal** by setting them to `0`.
3. **Apply gravity** by shifting non-zero elements downward, filling empty spaces.
4. **Repeat** the process until no more candies can be crushed.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def candyCrush(self, board: List[List[int]]) -> List[List[int]]:
        """
        Simulates the Candy Crush game until the board is stable.
        """
        m, n = len(board), len(board[0])
        found = True
        
        while found:
            found = False
            # Step 1: Mark candies to be crushed
            crush = [[False] * n for _ in range(m)]
            
            # Check for horizontal matches
            for i in range(m):
                for j in range(n - 2):
                    if board[i][j] != 0 and board[i][j] == board[i][j+1] == board[i][j+2]:
                        crush[i][j] = crush[i][j+1] = crush[i][j+2] = True
                        found = True
            
            # Check for vertical matches
            for i in range(m - 2):
                for j in range(n):
                    if board[i][j] != 0 and board[i][j] == board[i+1][j] == board[i+2][j]:
                        crush[i][j] = crush[i+1][j] = crush[i+2][j] = True
                        found = True
            
            # Step 2: Crush candies (set to 0)
            for i in range(m):
                for j in range(n):
                    if crush[i][j]:
                        board[i][j] = 0
            
            # Step 3: Apply gravity
            for j in range(n):
                # Collect non-zero elements in the column
                temp = [board[i][j] for i in range(m) if board[i][j] != 0]
                temp = [0] * (m - len(temp)) + temp  # Pad with zeros at the top
                for i in range(m):
                    board[i][j] = temp[i]
        
        return board

# Example Usage
solution = Solution()
print(solution.candyCrush([[1,3,5,5,2],[3,4,3,3,1],[3,2,4,5,2],[2,4,4,5,5],[1,4,4,1,1]]))
```

---

## Complexity Analysis
- **Scanning for matches:** `O(m * n)`
- **Crushing candies:** `O(m * n)`
- **Applying gravity:** `O(m * n)`
- **Overall Complexity:** `O(m * n)` per iteration, running multiple times until stabilization.

---

## Interview Tips
- **Edge Cases to Consider:**
  - A board that is already stable.
  - A board where all elements collapse into one row.
  - The smallest board size (`3x3`).
  - Boards with multiple cascading crushes.

- **Common Mistakes:**
  - Not correctly marking candies before setting them to `0`.
  - Forgetting to check for new crushes after gravity is applied.
  - Incorrectly implementing the gravity logic, causing misplaced elements.

- **Follow-up Questions:**
  - How would you optimize this solution for larger boards?
  - What if new candies appeared from the top after each crush?
  - Can you solve this problem using BFS or DFS instead?
