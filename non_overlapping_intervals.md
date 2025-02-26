# Non-overlapping Intervals

## Problem Statement
Given an array of **intervals** `intervals[i] = [starti, endi]`, return the **minimum number of intervals to remove** so that the remaining intervals are **non-overlapping**.

### **Rules:**
- Intervals **that only touch at a single point are NOT overlapping**.
- Example: `[1,2]` and `[2,3]` **are non-overlapping**.

---

### Example Cases
#### Example 1
**Input:**
```python
intervals = [[1,2],[2,3],[3,4],[1,3]]
```
**Output:**
```python
1
```
**Explanation:**
- Removing `[1,3]` makes all remaining intervals **non-overlapping**.

#### Example 2
**Input:**
```python
intervals = [[1,2],[1,2],[1,2]]
```
**Output:**
```python
2
```
**Explanation:**
- We need to remove **two** `[1,2]` to leave only one interval.

#### Example 3
**Input:**
```python
intervals = [[1,2],[2,3]]
```
**Output:**
```python
0
```
**Explanation:**
- No removals are needed since the intervals **do not overlap**.

### Constraints
- `1 <= intervals.length <= 10^5`
- `intervals[i].length == 2`
- `-5 * 10^4 <= starti < endi <= 5 * 10^4`

---

## Solution Approach
### **Greedy Approach (O(N log N) Solution)**
To remove the minimum number of intervals, we need to **keep as many non-overlapping intervals as possible**. The **best way** to do this is:

1. **Sort the intervals by their end times** (`end` in ascending order).
2. **Iterate through the intervals**, keeping track of the last interval's end time.
3. **If an interval overlaps**, remove it.
4. **If it doesn't overlap**, update the end time.

Sorting the intervals ensures that we always **prioritize intervals that finish earlier**, leaving more space for future intervals.

This ensures **O(N log N) time complexity** (due to sorting) and **O(1) space complexity**.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        """
        Finds the minimum number of intervals to remove to make the rest non-overlapping.
        """
        if not intervals:
            return 0  # No intervals to remove
        
        # Step 1: Sort intervals by ending times (greedy approach)
        intervals.sort(key=lambda x: x[1])
        
        # Step 2: Track the end of the last non-overlapping interval
        non_overlapping = 1  # At least one interval remains
        last_end = intervals[0][1]  # End time of first interval
        
        # Step 3: Iterate through sorted intervals
        for start, end in intervals[1:]:
            if start >= last_end:
                # If this interval does NOT overlap, keep it
                non_overlapping += 1
                last_end = end  # Update the last interval's end time
            # Otherwise, discard the interval (it overlaps)
        
        # Number of intervals removed = total intervals - non-overlapping intervals
        return len(intervals) - non_overlapping
```

---

## Complexity Analysis
- **Time Complexity:** `O(N log N)`, due to sorting the intervals.
- **Space Complexity:** `O(1)`, since sorting is done in-place.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `intervals = [[1,2]]` (Only one interval, return `0`)
  - `intervals = [[1,5],[2,3],[3,4]]` (Ensure sorting by end time works)
  - `intervals = [[1,100],[2,3],[3,4],[4,5]]` (Must prioritize smaller end times)
  - `intervals = [[1,2],[1,2],[1,2]]` (Duplicate intervals should be removed)

- **Common Mistakes:**
  - Sorting by **start time** instead of **end time** (incorrect approach).
  - Not handling cases where **no intervals overlap**.
  - Using a **brute force O(N²) approach**, which is too slow.

- **Follow-up Questions:**
  - How would this change if you were allowed to **merge overlapping intervals** instead of removing them?
  - How would you extend this to **higher dimensions** (e.g., overlapping rectangles)?
  - What if intervals were sorted by **start time** instead of end time?
