# Text Justification

## Problem Statement
Given an array of strings `words` and an integer `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully justified.

- Words should be packed in a **greedy** manner, meaning as many words as possible should be added to each line.
- Extra spaces should be distributed **evenly** between words.
- If spaces cannot be evenly distributed, the leftmost slots receive more spaces.
- The last line should be **left-justified**, meaning no extra spaces between words.

### Example Cases
#### Example 1
**Input:**
```python
words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
```
**Output:**
```python
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

#### Example 2
**Input:**
```python
words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
```
**Output:**
```python
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
```

#### Example 3
**Input:**
```python
words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
```
**Output:**
```python
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

### Constraints
- `1 <= words.length <= 300`
- `1 <= words[i].length <= 20`
- `words[i]` consists of only English letters and symbols.
- `1 <= maxWidth <= 100`
- `words[i].length <= maxWidth`

---

## Solution Approach

This problem is best solved using a **greedy** approach:
1. **Group words into lines**: Iteratively add words to a line until adding another word exceeds `maxWidth`.
2. **Distribute spaces**: Adjust spacing so that each line is fully justified except for the last line.
3. **Left-justify the last line**: Unlike other lines, the last line should not have extra spaces between words.

### **Algorithm Steps:**
1. Iterate through the words list, adding words to a temporary line while keeping track of the total length.
2. When adding another word would exceed `maxWidth`, justify the current line:
   - If only one word, left-align it and pad with spaces.
   - Otherwise, distribute spaces evenly between words.
3. Add the last line with left-justification.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        """
        Formats the given words into fully justified text with maxWidth characters per line.
        """
        res, line, line_length = [], [], 0
        
        for word in words:
            # Check if adding the next word exceeds maxWidth
            if line_length + len(word) + len(line) > maxWidth:
                # Distribute spaces evenly
                for i in range(maxWidth - line_length):  
                    # Assign spaces to the slots between words
                    # The modulo ensures spaces are distributed cyclically to the leftmost gaps first
                    line[i % (len(line) - 1 or 1)] += ' ' 
                res.append(''.join(line))
                line, line_length = [], 0
            
            line.append(word)
            line_length += len(word)
        
        # Last line - left-justified (no extra spaces between words)
        res.append(' '.join(line).ljust(maxWidth))
        
        return res

# Example Usage
solution = Solution()
print(solution.fullJustify(["This", "is", "an", "example", "of", "text", "justification."], 16))
```

---

## Explanation of Key Code Snippets

### **Space Distribution Logic:**
```python
for i in range(maxWidth - line_length):  # Distribute spaces
    line[i % (len(line) - 1 or 1)] += ' ' 
```
- `maxWidth - line_length`: Calculates the total number of spaces needed to justify the line.
- `len(line) - 1 or 1`: Determines how many gaps exist between words.
  - If there is only one word in the line, `or 1` ensures we don’t divide by zero.
- `i % (len(line) - 1 or 1)`: Uses modulo to distribute extra spaces from left to right in a cyclic pattern, ensuring left gaps receive more spaces when needed.

---

## Complexity Analysis
- **Time Complexity:** `O(n)`, where `n` is the number of words, since each word is processed once.
- **Space Complexity:** `O(n)`, as we store the output lines in a list.

---

## Interview Tips
- **Edge Cases to Consider:**
  - Single-word lines.
  - Last line should be left-justified with no extra spaces between words.
  - Words with different lengths requiring uneven spacing.
  - Large values of `maxWidth`.

- **Common Mistakes:**
  - Not handling the last line separately for left justification.
  - Incorrect space distribution leading to inconsistent formatting.
  - Forgetting to pad lines with spaces to match `maxWidth`.

- **Follow-up Questions:**
  - How would you modify the algorithm to handle very large text inputs efficiently?
  - Can you implement this using `deque` to improve performance?
