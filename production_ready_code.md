# Making Your Interview Code "Production-Ready"

Whether you’re coding a banking system, an inventory tracker, or any other service in an interview, you can demonstrate a “production mindset” by incorporating the following practices. These suggestions are purposely generic, so you can adapt them to nearly any coding question.

---

## 1. Validation & Error Handling

### Key Points
- **Validate Inputs**: Reject invalid or unexpected data before processing.
- **Use Domain-Specific Exceptions**: Differentiate between errors like `AccountNotFoundError` vs. `InsufficientFundsError` (for banking), or `ItemOutOfStockError` vs. `InvalidQuantityError` (for e-commerce).
- **Graceful Error Messages**: Provide useful messages that indicate *why* something failed.

### Example

```python
class NegativeAmountError(Exception):
    """Raised when the input amount is negative."""

class Bank:
    def deposit(self, account_id: int, amount: float) -> None:
        """
        Deposits a positive `amount` into an existing account.
        Raises NegativeAmountError if the amount is invalid.
        Raises KeyError if the account doesn't exist.
        """
        if amount <= 0:
            raise NegativeAmountError("Deposit amount must be positive.")
        if account_id not in self.accounts:
            raise KeyError(f"No account found with ID {account_id}.")
        
        self.accounts[account_id] += amount
```

> **Tip:** In an interview, simply adding `if amount < 0: raise ValueError(...)` can be enough to show you’ve considered invalid inputs.

---

## 2. Thread-Safety & Concurrency

### Key Points
- **Race Conditions**: If multiple threads or processes can call your code simultaneously, you need to ensure data consistency.
- **Synchronization**: Use locks or transactions to avoid partial updates.
- **Database Transactions**: In real systems, most concurrency control is handled via database transactions. In a simpler in-memory approach, use language-level locks.

### Example

```python
import threading

class Bank:
    def __init__(self):
        self.accounts = {}
        self._lock = threading.Lock()

    def transfer(self, from_id: int, to_id: int, amount: float) -> None:
        """Thread-safe transfer method."""
        with self._lock:
            if from_id not in self.accounts or to_id not in self.accounts:
                raise KeyError("Account not found.")
            if self.accounts[from_id] < amount:
                raise ValueError("Insufficient funds.")
            self.accounts[from_id] -= amount
            self.accounts[to_id] += amount
```

> **Tip:** In high-level system design, you’d rely on ACID transactions in a database instead of manual locks.

---

## 3. Logging & Monitoring

### Key Points
- **Audit Trails**: Especially in financial or sensitive domains, you need an audit log of all transactions or critical events.
- **Granular Logging Levels**: `DEBUG` for development details, `INFO` for normal events, `WARN` or `ERROR` for critical failures.
- **Monitoring & Alerts**: In real production systems, track metrics and trigger alerts for anomalies.

### Example

```python
import logging

logging.basicConfig(level=logging.INFO)

class Bank:
    def withdraw(self, account_id: int, amount: float) -> None:
        if account_id not in self.accounts:
            logging.error(f"Withdrawal failed: account {account_id} not found.")
            raise KeyError("Account not found.")
        if amount <= 0:
            logging.warning("Attempted to withdraw non-positive amount.")
            raise ValueError("Withdraw amount must be positive.")
        if self.accounts[account_id] < amount:
            logging.warning(f"Insufficient funds in account {account_id}.")
            raise ValueError("Insufficient funds.")
        
        self.accounts[account_id] -= amount
        logging.info(f"Account {account_id} withdrew {amount}. New balance: {self.accounts[account_id]}")
```

> **Tip:** In interviews, even a single log statement can show you care about observability.

---

## 4. Testing

### Key Points
- **Unit Tests**: Verify each function’s behavior (including edge cases).
- **Integration Tests**: Check how components work together (e.g., deposit + transfer flow).
- **Edge Cases**: Nonexistent IDs, negative amounts, zero amounts, concurrent requests.

### Example Using `pytest`

```python
# test_bank.py
import pytest
from bank import Bank, NegativeAmountError

def test_deposit_valid():
    bank = Bank()
    bank.accounts[1] = 100
    bank.deposit(1, 50)
    assert bank.accounts[1] == 150

def test_deposit_negative_amount():
    bank = Bank()
    bank.accounts[1] = 100
    with pytest.raises(NegativeAmountError):
        bank.deposit(1, -10)
```

> **Tip:** In a fast interview environment, even a few quick tests demonstrate you value correctness.

---

## 5. Documentation & Clean Code

### Key Points
- **Docstrings**: Clearly describe parameter types, return values, and exceptions raised.
- **PEP 8 Conventions** (for Python): Spaces around operators, consistent indentation, etc.
- **In-Line Comments**: Use them sparingly for tricky logic, but prefer self-descriptive code and docstrings.

### Example

```python
class Bank:
    def deposit(self, account_id: int, amount: float) -> None:
        """
        Deposit an amount into the specified account.

        :param account_id: Unique identifier of the account.
        :param amount: The positive amount to deposit.
        :raises NegativeAmountError: if amount <= 0
        :raises KeyError: if the account_id does not exist
        """
        ...
```

> **Tip:** Clarity and simplicity usually trump fancy structures in an interview setting.

---

## 6. Configuration / Environment

### Key Points
- **Database Persistence**: If your system is persistent, think about how you’d store data in a real DB and handle migrations.
- **Secrets Management**: For production, keep credentials out of source control; store them in environment variables or a secrets manager.
- **Environment Files**: For instance, `.env` or config files to specify DB connection details.

### Example Snippet (Pseudo-Code)

```python
import os
import psycopg2

DB_URL = os.environ.get("DB_URL", "postgresql://user:pass@localhost:5432/bank_db")

def get_connection():
    return psycopg2.connect(DB_URL)
```

> **Tip:** In an interview, a short mention of “we’d store real data in a DB with transactions” is typically enough.

---

## Final Thoughts

In an interview, you don’t need to show every single production feature. Instead, demonstrate that you **recognize** these dimensions of production readiness and can quickly implement a few:

- Validate inputs and raise domain-specific errors
- Concurrency or transaction notes (locks or DB commits)
- Logging or basic instrumentation
- Some test coverage
- Clear docstrings or code layout

This typically shows you’re both **a strong problem solver** and **capable of shipping safe, maintainable production code** when needed.

