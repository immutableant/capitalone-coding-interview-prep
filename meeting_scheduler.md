# Meeting Scheduler

## Problem Statement
Given the availability time slots arrays `slots1` and `slots2` of two people and a meeting duration `duration`, return the **earliest** time slot that works for both of them and is of duration `duration`.

If there is no common time slot that satisfies the requirements, return an empty array `[]`.

Each time slot is represented as `[start, end]`, indicating an **inclusive** time range from `start` to `end`.

It is guaranteed that **no two availability slots of the same person overlap**.

### Example Cases
#### Example 1
**Input:**
```python
slots1 = [[10,50],[60,120],[140,210]]
slots2 = [[0,15],[60,70]]
duration = 8
```
**Output:**
```python
[60,68]
```

#### Example 2
**Input:**
```python
slots1 = [[10,50],[60,120],[140,210]]
slots2 = [[0,15],[60,70]]
duration = 12
```
**Output:**
```python
[]
```

### Constraints
- `1 <= slots1.length, slots2.length <= 10^4`
- `slots1[i].length, slots2[i].length == 2`
- `slots1[i][0] < slots1[i][1]`
- `slots2[i][0] < slots2[i][1]`
- `0 <= slots1[i][j], slots2[i][j] <= 10^9`
- `1 <= duration <= 10^6`

---

## Solution Approach

The optimal approach is to use a **two-pointer technique** after sorting both availability slots.

### **Algorithm Steps:**
1. **Sort** both `slots1` and `slots2` based on start times.
2. Use two pointers to iterate through both slot lists, comparing intervals.
3. For each overlapping interval, check if the overlap duration is at least `duration`.
4. If a valid slot is found, return it immediately.
5. If no valid slot is found, return an empty array `[]`.

This approach efficiently finds the earliest available slot with `O(N log N + M log M)` complexity due to sorting.

---

## Optimized Python Solutions
### **Two-pointer Approach**
```python
from typing import List

class Solution:
    def minAvailableDuration(self, slots1: List[List[int]], slots2: List[List[int]], duration: int) -> List[int]:
        """
        Finds the earliest available time slot of at least `duration` length that both people can attend.
        """
        # Step 1: Sort both availability slots based on start times
        slots1.sort()
        slots2.sort()
        
        # Step 2: Use two pointers to iterate through slots
        i, j = 0, 0
        while i < len(slots1) and j < len(slots2):
            # Find the overlap of two current slots
            start = max(slots1[i][0], slots2[j][0])
            end = min(slots1[i][1], slots2[j][1])
            
            # Step 3: Check if the overlapping interval is sufficient for the meeting
            if end - start >= duration:
                return [start, start + duration]
            
            # Step 4: Move the pointer that has the earlier ending time
            if slots1[i][1] < slots2[j][1]:
                i += 1
            else:
                j += 1
        
        # No valid slot found
        return []
```

### **Heap-based Approach**
```python
import heapq
from typing import List

class Solution:
    def minAvailableDuration(self, slots1: List[List[int]], slots2: List[List[int]], duration: int) -> List[int]:
        """
        Finds the earliest available time slot using a min-heap for efficiency.
        """
        # Step 1: Filter out slots that are too short and merge both lists
        slots = [slot for slot in slots1 + slots2 if slot[1] - slot[0] >= duration]
        
        # Step 2: Convert the list into a min-heap based on start times
        heapq.heapify(slots)
        
        # Step 3: Extract slots and compare overlapping intervals
        while len(slots) > 1:
            s1 = heapq.heappop(slots)  # Pop the earliest slot
            s2 = slots[0]  # Peek at the next slot
            
            # Step 4: Check if the overlap duration is at least `duration`
            if min(s1[1], s2[1]) - s2[0] >= duration:
                return [s2[0], s2[0] + duration]
        
        # No valid slot found
        return []
```

---

## Complexity Analysis
- **Two-pointer Approach:** `O(N log N + M log M)` due to sorting, and `O(N + M)` for traversal.
- **Heap-based Approach:** `O((N + M) log (N + M))` due to heap operations.

---

## Interview Tips
- **Edge Cases to Consider:**
  - No common slot exists.
  - The slots barely overlap but are too short.
  - Slots are already sorted (checking algorithm performance).
  - Large values in input constraints.

- **Common Mistakes:**
  - Forgetting to sort the input lists before applying the two-pointer method.
  - Incorrectly computing overlap duration.
  - Failing to check both `slots1` and `slots2` lengths properly before accessing elements.

- **Follow-up Questions:**
  - How would you modify this to return **all** possible meeting slots?
  - What if we allow for slight flexibility in meeting durations?
  - Can this be optimized further using different data structures?
