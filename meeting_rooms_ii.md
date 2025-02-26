# Meeting Rooms II

## Problem Statement
Given an array of **meeting time intervals** `intervals` where `intervals[i] = [starti, endi]`, return the **minimum number of conference rooms required**.

---

### Example Cases
#### Example 1
**Input:**
```python
intervals = [[0,30],[5,10],[15,20]]
```
**Output:**
```python
2
```
**Explanation:**
- Meeting 1: `[0,30]`
- Meeting 2: `[5,10]`
- Meeting 3: `[15,20]`
- Meetings 1 and 2 overlap, so at least **two rooms** are needed.

#### Example 2
**Input:**
```python
intervals = [[7,10],[2,4]]
```
**Output:**
```python
1
```
**Explanation:**
- Meeting 1: `[7,10]`
- Meeting 2: `[2,4]`
- Since these meetings **do not overlap**, only **one room** is required.

### Constraints
- `1 <= intervals.length <= 10^4`
- `0 <= starti < endi <= 10^6`

---

## Solution Approach
### **Min-Heap Approach (O(n log n))**
To efficiently track the number of rooms required:
1. **Sort the meetings by start time**.
2. **Use a Min-Heap** to track the earliest ending meeting.
3. **Iterate through the sorted intervals:**
   - If the current meeting **starts after or when the earliest meeting ends**, remove that meeting from the heap.
   - Always add the current meeting’s **end time** to the heap.
4. The **size of the heap** at any point gives the **number of rooms needed**.

### **Python Implementation (Min-Heap)**
```python
from typing import List
import heapq

class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        """
        Finds the minimum number of meeting rooms required using a min-heap.
        """
        if not intervals:
            return 0

        # Step 1: Sort meetings by start time
        intervals.sort(key=lambda x: x[0])
        
        # Min-heap to track end times of meetings
        min_heap = []
        
        # Step 2: Add the first meeting’s end time to the heap
        heapq.heappush(min_heap, intervals[0][1])
        
        # Step 3: Iterate through remaining meetings
        for meeting in intervals[1:]:
            # If the earliest ending meeting has ended, remove it from heap
            if meeting[0] >= min_heap[0]:
                heapq.heappop(min_heap)
            
            # Allocate a room for the current meeting
            heapq.heappush(min_heap, meeting[1])
        
        # The heap size represents the number of rooms needed
        return len(min_heap)
```

### **Complexity Analysis**
- **Time Complexity:** `O(n log n)` due to sorting and heap operations.
- **Space Complexity:** `O(n)` for the heap storing end times.

---

## Alternative Approach: **Two-Pointer Method**
Instead of using a heap, we can leverage **sorted start and end times** to track room allocation.

### **Python Implementation (Two Pointers)**
```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        """
        Finds the minimum number of meeting rooms required using the two-pointer method.
        """
        if not intervals:
            return 0
        
        # Step 1: Separate and sort start and end times
        start_times = sorted([i[0] for i in intervals])
        end_times = sorted([i[1] for i in intervals])
        
        start_ptr = end_ptr = 0
        used_rooms = 0
        
        # Step 2: Process all meetings
        while start_ptr < len(intervals):
            # If the earliest meeting has ended, free up a room
            if start_times[start_ptr] >= end_times[end_ptr]:
                used_rooms -= 1
                end_ptr += 1
            
            # Allocate a new room
            used_rooms += 1
            start_ptr += 1
        
        return used_rooms
```

### **Complexity Analysis**
- **Time Complexity:** `O(n log n)` due to sorting.
- **Space Complexity:** `O(n)` for storing sorted times.

---

## Interview Tips
- **Key Concepts to Discuss:**
  - **Sorting start and end times separately**.
  - **Using a min-heap to efficiently track active meetings**.
  - **Greedy approach to track overlapping intervals**.

- **Common Edge Cases:**
  - Single meeting → return `1`.
  - Fully overlapping meetings → need as many rooms as meetings.
  - Meetings that just touch but don’t overlap (e.g., `[1, 3]` and `[3, 5]`).

- **Comparison Between Approaches:**
  - **Min-Heap Approach:** More intuitive for tracking dynamic allocations.
  - **Two-Pointer Approach:** Uses sorted times for efficient tracking, no heap required.

