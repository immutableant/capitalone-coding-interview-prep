# Find the Length of the Longest Common Prefix

## Problem Statement
You are given two arrays of **positive integers** `arr1` and `arr2`.

A **prefix** of a number is one or more of its **leading digits**. For example:
- `123` is a prefix of `12345`, but `234` is not.

A **common prefix** of two numbers `a` and `b` is an integer `c` such that `c` is a prefix of both `a` and `b`. For example:
- `5655359` and `56554` have common prefixes `565` and `5655`.
- `1223` and `43456` **do not** have a common prefix.

### **Goal:**
Find the **length of the longest common prefix** between any pair `(x, y)`, where:
- `x` belongs to `arr1`
- `y` belongs to `arr2`

If **no common prefix exists**, return `0`.

---

### Example Cases
#### Example 1
**Input:**
```python
arr1 = [1,10,100]
arr2 = [1000]
```
**Output:**
```python
3
```
**Explanation:**
- The longest common prefix of `(1, 1000)` is `1`.
- The longest common prefix of `(10, 1000)` is `10`.
- The longest common prefix of `(100, 1000)` is `100`.
- The longest common prefix is `100` with a **length of 3**.

#### Example 2
**Input:**
```python
arr1 = [1,2,3]
arr2 = [4,4,4]
```
**Output:**
```python
0
```
**Explanation:**
- No common prefixes exist for any pair, so we return `0`.

### Constraints
- `1 <= arr1.length, arr2.length <= 5 * 10^4`
- `1 <= arr1[i], arr2[i] <= 10^8`

---

## Solution Approach
### **Efficient Sorting + Binary Search (O(N log N) Solution)**
Instead of comparing every pair (**O(N × M) brute force**), we use a more **efficient approach**:

1. **Convert all numbers to strings** for easy prefix matching.
2. **Sort both arrays** (`arr1` and `arr2`) as strings.
3. **Use binary search**:
   - For each number in `arr1`, find the closest match in `arr2`.
   - Compare the longest common prefix using string matching.
4. Track and return the **maximum prefix length** found.

This ensures an **O(N log N + M log M) runtime**, which is efficient given the constraints.

---

## Optimized Python Solution
```python
from typing import List
from bisect import bisect_left

class Solution:
    def longestCommonPrefix(self, arr1: List[int], arr2: List[int]) -> int:
        """
        Finds the longest common prefix between numbers in arr1 and arr2.
        """
        # Convert numbers to strings for easier prefix comparison
        arr1 = sorted(map(str, arr1))
        arr2 = sorted(map(str, arr2))
        max_prefix_length = 0
        
        def common_prefix_length(a: str, b: str) -> int:
            """Returns the length of the longest common prefix between two strings."""
            length = 0
            for i in range(min(len(a), len(b))):
                if a[i] == b[i]:
                    length += 1
                else:
                    break
            return length
        
        # Binary search for closest match in arr2
        for num in arr1:
            idx = bisect_left(arr2, num)  # Find closest position in sorted arr2
            
            # Compare with nearest neighbors in arr2 (if they exist)
            if idx < len(arr2):
                max_prefix_length = max(max_prefix_length, common_prefix_length(num, arr2[idx]))
            if idx > 0:
                max_prefix_length = max(max_prefix_length, common_prefix_length(num, arr2[idx - 1]))
        
        return max_prefix_length
```

---

## Complexity Analysis
- **Sorting:** `O(N log N + M log M)` for sorting `arr1` and `arr2`.
- **Binary Search:** `O(N log M)`, since we search for each number in `arr1`.
- **Overall Complexity:** `O(N log N + M log M)`, which is much better than `O(N × M)` brute force.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `arr1` and `arr2` have no common digits → return `0`.
  - Single-element arrays → should return `len(arr1[0])` if they match.
  - Large `arr1` and `arr2` values → ensure sorting and binary search are efficient.

- **Common Mistakes:**
  - Forgetting to convert numbers to **strings** before checking prefixes.
  - Not handling edge cases where `bisect_left` returns **0 or len(arr2)**.
  - Using a **brute force O(N × M) approach**, which is too slow for large inputs.

- **Follow-up Questions:**
  - How would you modify this approach to **find the longest common prefix across multiple arrays**?
  - Can you extend this solution to **support negative numbers**?
  - How would you solve this problem if **numbers were stored in a database**?
