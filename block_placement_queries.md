# Block Placement Queries

## Problem Statement
There exists an infinite number line, with its origin at `0` and extending towards the positive x-axis.

You are given a 2D array `queries`, which contains two types of queries:

1. **Type 1 Query:** `queries[i] = [1, x]` → Build an obstacle at distance `x` from the origin. It is guaranteed that there is no obstacle at distance `x` when the query is asked.
2. **Type 2 Query:** `queries[i] = [2, x, sz]` → Check if it is possible to place a block of size `sz` anywhere in the range `[0, x]` on the line, such that the block entirely lies in the range `[0, x]`. A block cannot be placed if it **intersects** with any obstacle, but it may touch it. Note that you do not actually place the block; queries are separate.

Return a **boolean array** `results`, where `results[i]` is `True` if you can place the block specified in the `i-th` query of type `2`, and `False` otherwise.

### Example Cases
#### Example 1
**Input:**
```python
queries = [[1,2],[2,3,3],[2,3,1],[2,2,2]]
```
**Output:**
```python
[False, True, True]
```

#### Example 2
**Input:**
```python
queries = [[1,7],[2,7,6],[1,2],[2,7,5],[2,7,6]]
```
**Output:**
```python
[True, True, False]
```

### Constraints
- `1 <= queries.length <= 15 * 10^4`
- `2 <= queries[i].length <= 3`
- `1 <= queries[i][0] <= 2`
- `1 <= x, sz <= min(5 * 10^4, 3 * queries.length)`
- The input is generated such that for queries of type `1`, no obstacle exists at distance `x` when the query is asked.
- There is at least one query of type `2`.

---

## Solution Approach
To efficiently handle queries:
1. **Use a `SortedList` to track obstacles efficiently.**
2. **Use a `SortedList` to track the largest contiguous free space dynamically.**
3. **Process queries in reverse order to maintain increasing gaps and ensure efficient updates.**

---

## Optimized Python Solution
```python
from typing import List
from sortedcontainers import SortedList
import math

class Solution:
    def getResults(self, queries: List[List[int]]) -> List[bool]:
        """
        Processes block placement queries efficiently using a SortedList to track obstacles
        and gaps, ensuring fast insertions and lookups.
        """
        # Sorted list to maintain obstacle positions efficiently
        sl = SortedList()
        # Maximum possible x-coordinate, sys.maxsize can be also used
        # original: n = min(5 * 10 ** 4, len(queries) * 3) this takes into account constrains.
        n = math.inf
        # Initialize with boundary values (0 and n) to simplify range calculations
        sl.add(0)
        sl.add(n)
        
        ans = []
        # First pass: Add all obstacle placements to the sorted list
        for q in queries:
            if q[0] == 1:
                sl.add(q[1])
        
        # Sorted list to track gaps between obstacles
        gap = SortedList()
        gap.add((0, 0))  # Initialize with a dummy value
        curr = 0  # Track the largest gap encountered
        
        # Compute initial gaps between obstacles
        for x, y in zip(sl, sl[1:]):
            # (g := y - x) == assigning the result of y - x to g.
            # y - x is the current gap we are checking.
            if (g := y - x) > curr:
                gap.add((y, g))
                curr = g
        
        # Process queries in reverse order (to maintain increasing gaps)
        for q in reversed(queries):
            if q[0] == 1:
                # Type 1 Query: Remove obstacle and recompute gaps
                x = q[1]
                index = sl.index(x)
                after = sl[index + 1]  # Right neighbor
                before = sl[index - 1]  # Left neighbor
                sl.remove(x)  # Remove obstacle from sorted list
                
                # Compute new gap after removing the obstacle
                g = after - before
                
                # Remove any old gaps that are no longer valid
                index = gap.bisect_left((x, 0))
                while index < len(gap) and gap[index][1] <= g:
                    gap.pop(index)
                
                # Add the new computed gap to the sorted list
                if gap[index - 1][1] < g:
                    gap.add((after, g))
            else:
                # Type 2 Query: Check if a block of size sz can fit before x
                _, x, sz = q
                
                # Find the largest free space available before x
                index = sl.bisect_right(x)
                before = sl[index - 1]  # Leftmost obstacle before x
                
                # Find the maximum gap that can be used
                index = gap.bisect_right((before, math.inf)) - 1
                
                # Check if a block of size `sz` can fit in the free space
                ans.append((x - before) >= sz or gap[index][1] >= sz)
        
        # Reverse the results to match the original order of queries
        ans.reverse()
        return ans
```

---

## Complexity Analysis
- **Obstacle Insertions:** `O(log n)` per insertion (using `SortedList`).
- **Obstacle Lookups:** `O(log n)` using `SortedList` binary search.
- **Querying max gap size:** `O(log n)` per query.
- **Overall Complexity:** `O(n log n)`, where `n` is the number of queries.

---

## Interview Tips
- **Edge Cases to Consider:**
  - Queries only contain type `2` queries.
  - Large values for `x` and `sz`.
  - Multiple consecutive obstacles blocking placement.
  - The first query being a block placement check before any obstacles.

- **Common Mistakes:**
  - Forgetting that obstacles are **guaranteed to be unique**.
  - Not handling the edge case where no obstacles exist in the range.
  - Using `O(n^2)` brute-force checks instead of `O(log n)` binary search.

- **Follow-up Questions:**
  - How would you handle dynamic removal of obstacles?
  - Can this be extended to support **multi-dimensional block placement**?
  - How would you optimize for very large values of `x` (e.g., `10^9`)?
