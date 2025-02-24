# Count Operations to Obtain Zero

## Problem Statement
You are given two **non-negative integers** `num1` and `num2`.

In one operation:
- If `num1 >= num2`, subtract `num2` from `num1`.
- Otherwise, subtract `num1` from `num2`.

The process repeats until either `num1 == 0` or `num2 == 0`.

Return the **number of operations** required to reach zero for either number.

---

### Example Cases
#### Example 1
**Input:**
```python
num1 = 2, num2 = 3
```
**Output:**
```python
3
```
**Explanation:**
1. `num1 = 2, num2 = 3`. Since `num1 < num2`, subtract `num1` from `num2` → `num1 = 2, num2 = 1`.
2. `num1 = 2, num2 = 1`. Since `num1 > num2`, subtract `num2` from `num1` → `num1 = 1, num2 = 1`.
3. `num1 = 1, num2 = 1`. Since `num1 == num2`, subtract `num2` from `num1` → `num1 = 0, num2 = 1`.
4. **Total operations: 3**.

#### Example 2
**Input:**
```python
num1 = 10, num2 = 10
```
**Output:**
```python
1
```
**Explanation:**
1. `num1 = 10, num2 = 10`. Since `num1 == num2`, subtract `num2` from `num1` → `num1 = 0, num2 = 10`.
2. **Total operations: 1**.

### Constraints
- `0 <= num1, num2 <= 10^5`

---

## Solution Approach
The problem follows a **greedy** approach where we always subtract the smaller number from the larger one. Instead of performing one subtraction at a time, we can **optimize using division**:

1. If `num1 == 0` or `num2 == 0`, return `0` immediately.
2. Use **integer division** to calculate how many times `num2` can be subtracted from `num1`.
3. Swap values and repeat until one of the numbers becomes `0`.

This approach runs in **O(log(min(num1, num2)))** time, similar to the Euclidean algorithm for computing the greatest common divisor (GCD).

---

## Optimized Python Solution
```python
class Solution:
    def countOperations(self, num1: int, num2: int) -> int:
        """
        Counts the number of operations to make either num1 or num2 zero.
        """
        operations = 0  # Initialize operation counter
        
        while num1 > 0 and num2 > 0:
            if num1 > num2:
                # Instead of subtracting one-by-one, subtract num2 as many times as possible
                operations += num1 // num2  # Number of full subtractions possible
                num1 %= num2  # Reduce num1 to remainder
            else:
                operations += num2 // num1
                num2 %= num1
        
        return operations
```

---

## Complexity Analysis
- **Time Complexity:** `O(log(min(num1, num2)))`, since each step reduces one number significantly (similar to GCD computation).
- **Space Complexity:** `O(1)`, as only a few integer variables are used.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `num1 = 0` or `num2 = 0` (return `0` immediately).
  - Large values where one number is significantly bigger than the other (`10^5 vs 1`).
  - Cases where `num1 == num2` (should return `1` in one step).

- **Common Mistakes:**
  - Using a simple `while` loop with `num1 -= num2` instead of optimized division.
  - Not handling `0` cases early, leading to unnecessary computations.

- **Follow-up Questions:**
  - How would you modify this approach to return the sequence of operations instead of just the count?
  - Can you apply a similar approach to **finding GCD** using the subtraction method?
