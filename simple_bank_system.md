# Simple Bank System

## Problem Statement
You have been tasked with writing a program for a popular bank that will automate all its incoming transactions (**transfer, deposit, and withdraw**). The bank has `n` accounts numbered from **1 to n**. The initial balance of each account is stored in a **0-indexed** integer array `balance`, with the `(i + 1)th` account having an initial balance of `balance[i]`.

A **transaction is valid** if:
1. The given account number(s) are between **1 and n**.
2. The amount of money withdrawn or transferred is **less than or equal** to the balance of the account.

### **Operations to Implement:**
- `transfer(account1, account2, money)`: Transfer `money` dollars from `account1` to `account2`. Return `True` if successful, otherwise `False`.
- `deposit(account, money)`: Deposit `money` dollars into `account`. Return `True` if successful, otherwise `False`.
- `withdraw(account, money)`: Withdraw `money` dollars from `account`. Return `True` if successful, otherwise `False`.

### Example Cases
#### Example 1
**Input:**
```python
["Bank", "withdraw", "transfer", "deposit", "transfer", "withdraw"]
[[[10, 100, 20, 50, 30]], [3, 10], [5, 1, 20], [5, 20], [3, 4, 15], [10, 50]]
```
**Output:**
```python
[null, true, true, true, false, false]
```

**Explanation:**
```python
bank = Bank([10, 100, 20, 50, 30])
bank.withdraw(3, 10)    # return True  (Account 3 has balance 20, withdrawing 10 is valid)
bank.transfer(5, 1, 20) # return True  (Account 5 transfers 20 to account 1)
bank.deposit(5, 20)     # return True  (Deposit 20 to account 5)
bank.transfer(3, 4, 15) # return False (Account 3 has insufficient balance for transfer)
bank.withdraw(10, 50)   # return False (Account 10 does not exist)
```

### Constraints
- `n == balance.length`
- `1 <= n, account, account1, account2 <= 10^5`
- `0 <= balance[i], money <= 10^12`
- At most `10^4` calls will be made to each function.

---

## Solution Approach
To efficiently implement the banking system, we:
1. **Store balances** in a list `self.balance`, where `balance[i]` corresponds to account `(i+1)`.
2. **Ensure valid transactions** by checking if accounts exist and whether the balance is sufficient.
3. **Perform transactions** only when all validity checks pass.

### **Algorithm Steps:**
- `transfer(account1, account2, money)`: 
  1. Validate that both accounts exist.
  2. Ensure `account1` has enough balance.
  3. Deduct money from `account1` and add it to `account2`.
  4. Return `True` if successful.
- `deposit(account, money)`: 
  1. Validate that the account exists.
  2. Add `money` to the balance.
  3. Return `True` if successful.
- `withdraw(account, money)`: 
  1. Validate that the account exists.
  2. Ensure the account has enough balance.
  3. Deduct `money` from the balance.
  4. Return `True` if successful.

---

## Optimized Python Solution
```python
from typing import List

class Bank:
    def __init__(self, balance: List[int]):
        """
        Initialize the bank with given balances.
        """
        self.balance = balance  # Store balances for all accounts
        self.n = len(balance)   # Number of accounts

    def valid_account(self, account: int) -> bool:
        """
        Check if the given account number is valid.
        """
        return 1 <= account <= self.n

    def transfer(self, account1: int, account2: int, money: int) -> bool:
        """
        Transfers `money` from `account1` to `account2` if valid.
        """
        if self.valid_account(account1) and self.valid_account(account2) and self.balance[account1 - 1] >= money:
            self.balance[account1 - 1] -= money  # Deduct from account1
            self.balance[account2 - 1] += money  # Add to account2
            return True
        return False

    def deposit(self, account: int, money: int) -> bool:
        """
        Deposits `money` into `account` if valid.
        """
        if self.valid_account(account):
            self.balance[account - 1] += money  # Add money to the account
            return True
        return False

    def withdraw(self, account: int, money: int) -> bool:
        """
        Withdraws `money` from `account` if valid and sufficient balance.
        """
        if self.valid_account(account) and self.balance[account - 1] >= money:
            self.balance[account - 1] -= money  # Deduct money from the account
            return True
        return False

# Example Usage
bank = Bank([10, 100, 20, 50, 30])
print(bank.withdraw(3, 10))    # Output: True
print(bank.transfer(5, 1, 20)) # Output: True
print(bank.deposit(5, 20))     # Output: True
print(bank.transfer(3, 4, 15)) # Output: False
print(bank.withdraw(10, 50))   # Output: False
```

---

## Complexity Analysis
- **Time Complexity:** `O(1)` per operation since all lookups and modifications are constant-time.
- **Space Complexity:** `O(n)`, where `n` is the number of accounts.

---

## Interview Tips
- **Edge Cases to Consider:**
  - Transactions involving non-existent accounts.
  - Withdrawals exceeding account balances.
  - Deposits with large values (`10^12`).
  - Handling high-frequency transactions (`10^4` operations).

- **Common Mistakes:**
  - Using `0-based` indexing for accounts without adjusting.
  - Forgetting to validate account existence before transactions.
  - Not handling large money transactions efficiently.

- **Follow-up Questions:**
  - How would you modify the system to support interest accumulation?
  - How would you handle concurrent transactions in a multi-threaded system?
  - What optimizations can be made for a **high-frequency trading** scenario?

## Alternative Implementation Based on Interview Experience
```python
from typing import List

class Bank:
    def __init__(self, balance: List[int]):
        """
        Initialize the bank with given balances.
        """
        self.balance = balance  # Store balances for all accounts
        self.n = len(balance)   # Number of accounts

    def process_transaction(self, transaction: List):
        """
        Processes a transaction request.
        """
        operation, *params = transaction
        if operation == "withdraw":
            return self.withdraw(*params)
        elif operation == "transfer":
            return self.transfer(*params)
        elif operation == "deposit":
            return self.deposit(*params)
        return False
    
    def valid_account(self, account: int) -> bool:
        """
        Check if the given account number is valid.
        """
        return 1 <= account <= self.n

    def transfer(self, account1: int, account2: int, money: int) -> bool:
        """
        Transfers `money` from `account1` to `account2` if valid.
        """
        if self.valid_account(account1) and self.valid_account(account2) and self.balance[account1 - 1] >= money:
            self.balance[account1 - 1] -= money  # Deduct from account1
            self.balance[account2 - 1] += money  # Add to account2
            return True
        return False

    def deposit(self, account: int, money: int) -> bool:
        """
        Deposits `money` into `account` if valid.
        """
        if self.valid_account(account):
            self.balance[account - 1] += money  # Add money to the account
            return True
        return False

    def withdraw(self, account: int, money: int) -> bool:
        """
        Withdraws `money` from `account` if valid and sufficient balance.
        """
        if self.valid_account(account) and self.balance[account - 1] >= money:
            self.balance[account - 1] -= money  # Deduct money from the account
            return True
        return False

# Example Usage
bank = Bank([10, 100, 20, 50, 30])
transactions = [
    ["withdraw", 3, 10],
    ["transfer", 5, 1, 20],
    ["deposit", 5, 20],
    ["transfer", 3, 4, 15],
    ["withdraw", 10, 50]
]
results = [bank.process_transaction(t) for t in transactions]
print(results)  # Output: [True, True, True, False, False]
```

---

## Additional Considerations
If asked about the following, here are some potential implementations:

- **Concurrent Transactions:** Use locks (`threading.Lock()`) to synchronize balance modifications.
- **Logging of Transactions:** Implement a transaction log (`list` or `database`) to track all actions.
- **Audit Feature & Rollback:** Store previous balances before a transaction to allow reversal.
- **Handling Overdraft Protection:** Add an overdraft policy with defined limits before allowing withdrawals.
- **Interest Accumulation:** Implement a scheduled job that applies interest based on account balances over time.
- **Handling Large Transaction Volume:** Use efficient data structures such as `OrderedDict` or `SQLite` for scalability.


## Alternative Implementation Based on Interview Experience
```python
from typing import List

class Bank:
    def __init__(self, accounts: dict[int, int]):
        """
        Initialize the bank with given balances.
        """
        self.accounts = accounts  # Store account ID and balance

    def process_transaction(self, transaction: List):
        """
        Processes a transaction request.
        """
        operation, *params = transaction
        if operation == "withdraw":
            return self.withdraw(*params)
        elif operation == "transfer":
            return self.transfer(*params)
        elif operation == "deposit":
            return self.deposit(*params)
        return False
    
    def valid_account(self, account: int) -> bool:
        """
        Check if the given account number is valid.
        """
        return account in self.accounts

    def transfer(self, account1: int, account2: int, money: int) -> bool:
        """
        Transfers `money` from `account1` to `account2` if valid.
        """
        if self.valid_account(account1) and self.valid_account(account2) and self.accounts[account1] >= money:
            self.accounts[account1] -= money  # Deduct from account1
            self.accounts[account2] += money  # Add to account2
            return True
        return False

    def deposit(self, account: int, money: int) -> bool:
        """
        Deposits `money` into `account` if valid.
        """
        if self.valid_account(account):
            self.accounts[account] += money  # Add money to the account
            return True
        return False

    def withdraw(self, account: int, money: int) -> bool:
        """
        Withdraws `money` from `account` if valid and sufficient balance.
        """
        if self.valid_account(account) and self.accounts[account] >= money:
            self.accounts[account] -= money  # Deduct money from the account
            return True
        return False

# Example Usage
bank = Bank({101: 10, 102: 100, 103: 20, 104: 50, 105: 30})
transactions = [
    ["withdraw", 103, 10],
    ["transfer", 105, 101, 20],
    ["deposit", 105, 20],
    ["transfer", 103, 104, 15],
    ["withdraw", 110, 50]
]
results = [bank.process_transaction(t) for t in transactions]
print(results)  # Output: [True, True, True, False, False]
```