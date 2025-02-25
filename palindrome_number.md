# Palindrome Number

## Problem Statement
Given an integer `x`, return `True` if `x` is a **palindrome**, and `False` otherwise.

A number is a **palindrome** if it reads the same forward and backward.

---

### Example Cases
#### Example 1
**Input:**
```python
x = 121
```
**Output:**
```python
True
```
**Explanation:** `121` reads the same forward and backward.

#### Example 2
**Input:**
```python
x = -121
```
**Output:**
```python
False
```
**Explanation:** `-121` reads as `121-`, which is not a palindrome.

#### Example 3
**Input:**
```python
x = 10
```
**Output:**
```python
False
```
**Explanation:** `10` reads as `01`, which is not a palindrome.

### Constraints
- `-2^{31} <= x <= 2^{31} - 1`

**Follow-up:** Can you solve this problem **without converting the integer to a string**?

---

## Solution Approach

### **Method 1: String Reversal (Simple Approach - O(N))**
1. Convert `x` to a string.
2. Check if the string is equal to its reverse.
3. Return `True` if they match, otherwise return `False`.

This method runs in **O(1) time**.

## Optimized Python Solution
```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        s = str(x)
        return s == s[::-1]
```

### **Method 2: Reverse Half the Number (Optimal Approach - O(log N))**
1. **Negative numbers are never palindromes**, so return `False` if `x < 0`.
2. **Single-digit numbers are always palindromes**, so return `True` if `x < 10`.
3. Reverse the second half of the number using **modulus and division**.
4. Compare the reversed half with the first half:
   - If they match, return `True`.
   - If they don’t match, return `False`.

This method runs in **O(log N) time** and uses **O(1) space**, making it the most efficient.

---

## Optimized Python Solution
```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        """
        Checks if a number is a palindrome without converting it to a string.
        """
        if x < 0 or (x % 10 == 0 and x != 0):
            return False  # Negative numbers and numbers ending in 0 (except 0 itself) are not palindromes
        
        reversed_half = 0
        while x > reversed_half:
            reversed_half = reversed_half * 10 + x % 10  # Extract last digit and add to reversed half
            x //= 10  # Remove last digit from x
        
        # Compare the first half with the reversed second half
        return x == reversed_half or x == reversed_half // 10
```

---

## Complexity Analysis
- **Time Complexity:** `O(log N)`, since we only process **half the digits**.
- **Space Complexity:** `O(1)`, since no extra space is used.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `x < 0` (negative numbers are not palindromes).
  - `x = 0` (should return `True`).
  - `x = 10, 100, 1000` (trailing zeros make it non-palindromic).
  - `x = 1, 22, 12321` (valid palindromes).

- **Common Mistakes:**
  - Forgetting to check for negative numbers.
  - Incorrectly handling trailing zeros (`100` should not be a palindrome).
  - Using `O(N)` space when an `O(1)` solution exists.

- **Follow-up Questions:**
  - How would you solve this problem for **very large numbers**?
  - Can you optimize this for **floating-point numbers**?
  - What modifications would be needed for **binary palindromes**?
