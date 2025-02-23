# Count Primes

## Problem Statement
Given an integer `n`, return the number of prime numbers that are **strictly less than** `n`.

### Example Cases
#### Example 1
**Input:**
```python
n = 10
```
**Output:**
```python
4
```
**Explanation:** The prime numbers less than 10 are **2, 3, 5, 7**.

#### Example 2
**Input:**
```python
n = 0
```
**Output:**
```python
0
```

#### Example 3
**Input:**
```python
n = 1
```
**Output:**
```python
0
```

### Constraints
- `0 <= n <= 5 * 10^6`

---

## Solution Approach

To efficiently count prime numbers less than `n`, we use **The Sieve of Eratosthenes**, which is an optimized algorithm for finding all primes up to a given number.

### **Algorithm Explanation:**
1. Create a boolean list `is_prime` initialized to `True` for numbers from `0` to `n-1`.
2. Mark `0` and `1` as `False` since they are not prime.
3. Iterate through numbers starting from `2` and mark all multiples of each prime as `False` (since they are composite).
4. Count the numbers that remain `True` in the list.

---

## Optimized Python Solution
```python
from typing import List

class Solution:
    def countPrimes(self, n: int) -> int:
        """
        Uses the Sieve of Eratosthenes to count the number of primes less than n.
        """
        if n < 2:
            return 0  # No prime numbers exist below 2
        
        # Step 1: Initialize a boolean list to track prime numbers
        is_prime = [True] * n
        is_prime[0] = is_prime[1] = False  # 0 and 1 are not prime
        
        # Step 2: Iterate over numbers and mark their multiples as non-prime
        for i in range(2, int(n ** 0.5) + 1):  # Only go up to sqrt(n) - start at 2, finish at sqrt(n) + 1
            if is_prime[i]:
                # Step 3: Mark all multiples of i as non-prime
                for j in range(i * i, n, i):  # Start from i^2 (smaller multiples already marked)
                    is_prime[j] = False
        
        # Step 4: Count the prime numbers
        return sum(is_prime)

# Example Usage
solution = Solution()
print(solution.countPrimes(10))  # Output: 4
```

---

## Complexity Analysis
- **Time Complexity:** `O(N∗Log(Log(N)))`, which is much faster than `O(n^2)` brute force methods.
- **Space Complexity:** `O(n)`, since we maintain a boolean array of size `n`.

---

## Interview Tips
- **Edge Cases to Consider:**
  - `n = 0` or `n = 1` (no primes exist, should return `0`).
  - `n = 2` (still no prime numbers **strictly** less than `2`).
  - Large inputs (`n = 5 * 10^6`): Ensure efficiency by using Sieve of Eratosthenes.

- **Common Mistakes:**
  - Forgetting to mark `0` and `1` as non-prime.
  - Iterating all the way to `n` instead of `sqrt(n)` in the sieve optimization.
  - Using an inefficient `O(n^2)` approach, which will time out on large inputs.

- **Follow-up Questions:**
  - Can you solve this problem using a different approach? (e.g., Trial division, Linear sieve)
  - How can you optimize this algorithm for memory efficiency?
  - How would you modify the algorithm to return a list of prime numbers instead of just a count?
