# Number of Islands

## Problem Statement
Given an `m x n` **2D binary grid** `grid` where:
- `'1'` represents **land**.
- `'0'` represents **water**.

Return the **number of islands** in the grid.

### **Definition of an Island:**
- An **island** is surrounded by water.
- An island is formed by **connecting adjacent lands horizontally or vertically**.
- You may assume **all four edges of the grid are surrounded by water**.

---

### Example Cases
#### Example 1
**Input:**
```python
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
```
**Output:**
```python
1
```

#### Example 2
**Input:**
```python
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```
**Output:**
```python
3
```

### Constraints
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

---

## Solution Approach
### **Depth-First Search (DFS) Approach (O(m × n))**
1. **Iterate through each cell** in the grid.
2. When encountering a `'1'` (land):
   - **Perform DFS** to explore and mark the entire island as visited.
   - **Increment island count**.
3. **Base case for DFS:**
   - Ignore **out-of-bounds** indices.
   - Ignore **water (`'0'`)** or already visited land.

### **Python Implementation (DFS)**
```python
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        visited = set()

        def dfs(r, c):
            """Performs DFS to mark all connected land cells as visited."""
            if (
                r < 0 or c < 0 or
                r >= rows or c >= cols or
                grid[r][c] == "0" or
                (r, c) in visited
            ):
                return
            
            visited.add((r, c))  # Mark current cell as visited
            
            # Explore adjacent cells
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        island_count = 0

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1" and (r, c) not in visited:
                    dfs(r, c)  # Perform DFS
                    island_count += 1  # Increment island count

        return island_count
```

### **Complexity Analysis**
- **Time Complexity:** `O(m × n)` → Every cell is visited at most once.
- **Space Complexity:** `O(m × n)` → Worst case due to recursion depth.

---

## Alternative Approach: **Breadth-First Search (BFS)**
Instead of using recursion, **BFS** uses a **queue** to explore neighboring land cells iteratively.

### **Python Implementation (BFS)**
```python
from collections import deque
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        visited = set()

        def bfs(r, c):
            """Performs BFS to mark all connected land cells as visited."""
            queue = deque([(r, c)])
            while queue:
                row, col = queue.popleft()
                for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                    nr, nc = row + dr, col + dc
                    if (
                        0 <= nr < rows and 0 <= nc < cols and
                        grid[nr][nc] == "1" and (nr, nc) not in visited
                    ):
                        visited.add((nr, nc))
                        queue.append((nr, nc))

        island_count = 0

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1" and (r, c) not in visited:
                    visited.add((r, c))
                    bfs(r, c)
                    island_count += 1

        return island_count
```

### **Complexity Analysis**
- **Time Complexity:** `O(m × n)`, since each cell is visited once.
- **Space Complexity:** `O(min(m, n))`, due to queue usage in BFS.

---

## Interview Tips
- **Key Concepts to Discuss:**
  - **Graph traversal** using DFS and BFS.
  - **Connected components** in a grid.
- **Common Edge Cases:**
  - **Empty grid** (`grid = [[]]`) → Return `0`.
  - **All water** (`grid = [['0', '0'], ['0', '0']]`) → Return `0`.
  - **All land** (`grid = [['1', '1'], ['1', '1']]`) → Return `1`.
- **Comparison Between DFS & BFS:**
  - **DFS:** Uses recursion (can cause stack overflow for large grids).
  - **BFS:** Uses an explicit queue (avoids deep recursion issues).
  