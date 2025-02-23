# Longest Substring Without Repeating Characters

## Problem Statement
Given a string `s`, find the length of the longest substring without repeating characters.

### Example Cases
#### Example 1
**Input:**
```python
s = "abcabcbb"
```
**Output:**
```python
3
```
**Explanation:** The answer is "abc", with the length of 3.

#### Example 2
**Input:**
```python
s = "bbbbb"
```
**Output:**
```python
1
```
**Explanation:** The answer is "b", with the length of 1.

#### Example 3
**Input:**
```python
s = "pwwkew"
```
**Output:**
```python
3
```
**Explanation:** The answer is "wke", with the length of 3. Note that "pwke" is a subsequence, not a substring.

### Constraints
- `0 <= s.length <= 5 * 10^4`
- `s` consists of English letters, digits, symbols, and spaces.

---

## Solution Approach
This problem can be efficiently solved using the **Sliding Window** technique.

### **Algorithm Steps:**
1. Use a **set** to track characters within the current window.
2. Expand the window by moving the **right** pointer.
3. If a character repeats, shrink the window by moving the **left** pointer until the substring is valid again.
4. Keep track of the **maximum length** encountered.
5. Return the maximum length after iterating through the string.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        """
        Finds the length of the longest substring without repeating characters.
        """
        char_set = set()
        left = 0  # Left pointer of the sliding window
        max_length = 0  # Stores the maximum substring length found

        for right in range(len(s)):
            # If character at `right` is already in the set, move `left` to remove duplicate
            while s[right] in char_set:
                char_set.remove(s[left])  # Remove character at `left`
                left += 1  # Move `left` pointer forward
            
            # Add current character at `right` to the set
            char_set.add(s[right])
            
            # Update max_length with the new window size
            max_length = max(max_length, right - left + 1)
        
        return max_length

# Example Usage
solution = Solution()
print(solution.lengthOfLongestSubstring("abcabcbb"))  # Output: 3
```

---

## Complexity Analysis
- **Time Complexity:** `O(n)`, since each character is visited at most twice (once by `right`, once by `left`).
- **Space Complexity:** `O(k)`, where `k` is the number of unique characters in the string.

---

## Alternative Approach: Hash Map
Instead of using a set, we can use a **hash map** to track character indices, allowing us to jump the `left` pointer directly past duplicate characters.

### **Alternative Code Using Hash Map:**
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        """
        Finds the length of the longest substring without repeating characters using a hash map.
        """
        char_index = {}  # Dictionary to store last seen index of characters
        left = 0  # Left pointer of the sliding window
        max_length = 0  # Stores the maximum substring length found

        for right, char in enumerate(s):
            # If character is already seen and within the current window, move `left` past it
            if char in char_index and char_index[char] >= left:
                left = char_index[char] + 1
            
            # Store/update character's latest index
            char_index[char] = right
            
            # Update max_length with the new window size
            max_length = max(max_length, right - left + 1)
        
        return max_length
```

---

## Interview Tips
- **Edge Cases to Consider:**
  - An empty string.
  - A string with all identical characters.
  - A string with no repeating characters.
- **Common Mistakes:**
  - Not handling edge cases correctly (e.g., `s = ""`).
  - Forgetting to update the **left** pointer correctly when a duplicate character is found.
  - Confusing substrings with subsequences.

- **Follow-up Questions:**
  - Can you modify the solution to return the actual substring instead of just the length?
  - How would you adapt the approach for a **streaming input** where characters arrive one by one?
