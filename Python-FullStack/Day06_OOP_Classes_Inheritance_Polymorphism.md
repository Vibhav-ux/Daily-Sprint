# 🐍 Python Full Stack — Day 06: Object-Oriented Programming (OOP)
### Phase 0: Foundations | Classes, Encapsulation, Inheritance, Polymorphism & Dunder Methods

---

## 📌 1. The Core 4 Pillars of OOP

1. **Encapsulation**: Bundling state and methods together while restricting direct access to internals.
2. **Abstraction**: Hiding complex internal logic behind clean public interfaces.
3. **Inheritance**: Deriving new classes from existing ones to maximize code reusability.
4. **Polymorphism**: Treating different objects through a common interface.

---

## 📌 2. Defining Classes & The `self` Parameter

```python
class BankAccount:
    # Class-level attribute (shared across all instances)
    bank_name = "Global Sprint Bank"

    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner
        self._balance = balance  # Protected convention (single underscore)
        self.__pin = "1234"      # Private (mangled name: _BankAccount__pin)

    # Method
    def deposit(self, amount: float):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self._balance += amount
        return self._balance

    # Getter property
    @property
    def balance(self) -> float:
        return self._balance

    # Dunder / Magic method for readable print output
    def __str__(self):
        return f"Account(owner={self.owner}, balance=${self._balance:.2f})"
```

---

## 📌 3. Inheritance & `super()`

```python
class SavingsAccount(BankAccount):
    def __init__(self, owner: str, balance: float = 0.0, interest_rate: float = 0.05):
        # Call the parent constructor
        super().__init__(owner, balance)
        self.interest_rate = interest_rate

    def apply_interest(self):
        interest = self._balance * self.interest_rate
        self.deposit(interest)
        return self._balance
```

---

## 📌 4. Magic / Dunder Methods Overview

| Method | Triggered by | Example |
|---|---|---|
| `__init__` | Object instantiation | `acc = BankAccount("Vibhav")` |
| `__str__` | `print(obj)`, `str(obj)` | User-friendly string |
| `__repr__` | Interactive console / debugging | Developer representation |
| `__len__` | `len(obj)` | Length of collection |
| `__eq__` | `obj1 == obj2` | Custom equality check |
| `__getitem__` | `obj[key]` | Indexing support |
