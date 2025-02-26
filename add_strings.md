# Add Strings

## Problem Statement
You are given two **non-negative integers** `num1` and `num2` represented as **strings**. Return the **sum** of `num1` and `num2` as a **string**.

### **Constraints:**
- You **must not** use built-in functions for handling large integers (e.g., `int()` or `BigInteger`).
- You **must not** convert the inputs directly to integers.
- `1 <= num1.length, num2.length <= 10^4`
- `num1` and `num2` consist of **only digits (0-9)**.
- `num1` and `num2` **do not have leading zeros**, except when they are `"0"`.

---

### Example Cases
#### Example 1
**Input:**
```python
num1 = "11"
num2 = "123"
```
**Output:**
```python
"134"
```

#### Example 2
**Input:**
```python
num1 = "456"
num2 = "77"
```
**Output:**
```python
"533"
```

#### Example 3
**Input:**
```python
num1 = "0"
num2 = "0"
```
**Output:**
```python
"0"
```

---

## Solution Approach
### **Simulating Manual Addition (O(N) Solution)**
Since we cannot use built-in integer conversion, we need to simulate **manual addition** just like we do on paper:

1. **Start from the rightmost digit** (least significant digit).
2. **Add digits one by one**, keeping track of **carry**.
3. **Continue until all digits are processed**, ensuring to handle leftover carry.
4. **Reverse the result** since digits were added from right to left.

This approach ensures **O(N) time complexity**, where `N` is the length of the longer string.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        """
        Adds two non-negative integers represented as strings without using built-in integer conversion.
        """
        i, j = len(num1) - 1, len(num2) - 1  # Pointers to the last digits
        carry = 0  # Carry for addition
        result = []  # Store the result digits
        
        while i >= 0 or j >= 0 or carry:
            digit1 = ord(num1[i]) - ord('0') if i >= 0 else 0  # Convert char to int
            digit2 = ord(num2[j]) - ord('0') if j >= 0 else 0  # Convert char to int
            
            total = digit1 + digit2 + carry  # Sum of digits and carry
            carry = total // 10  # Compute new carry
            result.append(str(total % 10))  # Append last digit of sum to result
            
            i -= 1  # Move left in num1
            j -= 1  # Move left in num2
        
        return ''.join(result[::-1])  # Reverse result and join to form final string
```

---

## Complexity Analysis
- **Time Complexity:** `O(N)`, where `N` is the length of the longer input string.
- **Space Complexity:** `O(N)`, as we store the result in a list before converting to a string.

---

## Why Not Use `int()`?
Python provides a built-in `int()` function that efficiently converts strings to integers. However, this problem **disallows** its usage to ensure we manually implement number addition. Here's how `int(str)` works internally:

1. **Parses the string and validates characters.**
2. **Converts each character to a digit** using `ord(char) - ord('0')`.
3. **Computes the integer value** using:
   ```
   num = (digit1 * 10^n) + (digit2 * 10^(n-1)) + ... + digitN
   ```
4. **Handles large numbers efficiently** using optimized multiplication and addition routines in C.

### **Why Not Implement Our Own `int()`?**
- Python’s `int(str)` is **highly optimized in C** and significantly faster than any custom implementation.
- The built-in function **supports large integers natively**, whereas our implementation would need additional logic to handle arbitrary precision.
- Using `int(str)` directly would defeat the purpose of manually understanding how integer operations work at the character level.

Thus, while we could simply use `int(num1) + int(num2)`, this problem is designed to **demonstrate how integer arithmetic works internally**.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `num1 = "0"`, `num2 = "0"` (should return `"0"`).
  - Unequal lengths (`num1 = "999"`, `num2 = "1"` should return `"1000"`).
  - Large values where `num1.length = num2.length = 10^4`.

- **Common Mistakes:**
  - Forgetting to **handle carry correctly** when summing digits.
  - Not processing the **remaining carry** after all digits are processed.
  - Using `int()` conversion instead of manual digit handling.

- **Follow-up Questions:**
  - How would you modify this solution for **subtraction** instead of addition?
  - Can you extend this to **support decimal numbers**?
  - How would you implement **multiplication of two large numbers** using a similar approach?
