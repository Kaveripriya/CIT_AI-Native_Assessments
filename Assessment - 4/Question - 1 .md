# Q1. Campus Wallet

**Topics**

- **Task 1:** Loop, tuples, dictionary
- **Task 2:** Class, methods, f-string
- **Task 3:** `raise ValueError`, `try/except` in a loop

## Problem statement

The college canteen is moving to a cashless campus wallet. Students top up their wallets and pay for food from them. A payment larger than the balance must be rejected.

Complete the code below. The pre-written `solve()` function calls your code for each task. Do not edit `solve()`.

### Task 1

Complete `total_topups(topups)`. `topups` is a list of `[name, amount]` pairs, and a student can appear more than once. Return a dictionary of each student's total top-up.

**Example:** `[["Asha", 100], ["Ravi", 50], ["Asha", 20]]` → `{"Asha": 120, "Ravi": 50}`

### Task 2

Complete the `Wallet` class:

- `__init__(owner, balance=0)` stores the owner and the balance.
- `add_money(amount)` adds to the balance and returns the new balance.
- `summary()` returns a string like `"Asha: Rs.120"`.

### Task 3

- Complete `pay(amount)`. If `amount` is more than the balance, raise `ValueError("Insufficient balance")`. Otherwise subtract it and return the new balance.
- Complete `process_payments(wallet, amounts)`. Call `wallet.pay()` for each amount inside `try/except`, so one failed payment does not stop the rest. Return `{"paid": ..., "failed": ..., "balance": ...}`.

**Execution mode:** Write a function  
**Entry point:** `solve`

## Starter code

```python
def total_topups(topups):
    # use a loop and a dictionary
    pass


class Wallet:
    def __init__(self, owner, balance=0):
        # Task 2: store owner and balance
        pass

    def add_money(self, amount):
        # Task 2: add to balance, return the new balance
        pass

    def summary(self):
        # Task 2: return a string like "Asha: Rs.120"
        pass

    def pay(self, amount):
        # Task 3: raise ValueError("Insufficient balance") when amount is more than balance
        pass


def process_payments(wallet, amounts):
    # Task 3: use try/except around wallet.pay()
    pass


# Do not edit below this line
def solve(task, data):
    if task == "task1":
        return total_topups(data)
    if task == "task2":
        wallet = Wallet(data["owner"], data["balance"])
        for amount in data["topups"]:
            wallet.add_money(amount)
        return wallet.summary()
    if task == "task3a":
        wallet = Wallet("Student", data["balance"])
        try:
            return wallet.pay(data["amount"])
        except ValueError as e:
            return "ERROR: " + str(e)
    if task == "task3b":
        wallet = Wallet("Student", data["balance"])
        return process_payments(wallet, data["amounts"])
```

## Reference solution (author only)

```python
def total_topups(topups):
    totals = {}
    for name, amount in topups:
        if name in totals:
            totals[name] = totals[name] + amount
        else:
            totals[name] = amount
    return totals


class Wallet:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def add_money(self, amount):
        self.balance = self.balance + amount
        return self.balance

    def summary(self):
        return f"{self.owner}: Rs.{self.balance}"

    def pay(self, amount):
        if amount > self.balance:
            raise ValueError("Insufficient balance")
        self.balance = self.balance - amount
        return self.balance


def process_payments(wallet, amounts):
    paid = 0
    failed = 0
    for amount in amounts:
        try:
            wallet.pay(amount)
            paid += 1
        except ValueError:
            failed += 1
    return {"paid": paid, "failed": failed, "balance": wallet.balance}
```

## Test cases (each worth 1 of 12)

| # | Task | Arguments (JSON) | Expected (JSON) | Visibility |
|---|---|---|---|---|
| 1 | Task 1 | `["task1",[["Asha",100],["Ravi",50],["Asha",20]]]` | `{"Asha":120,"Ravi":50}` | Shown |
| 2 | Task 1 | `["task1",[["Meena",200]]]` | `{"Meena":200}` | Hidden |
| 3 | Task 1 | `["task1",[]]` | `{}` | Hidden |
| 4 | Task 1 | `["task1",[["Karthik",10],["Karthik",15],["Divya",30],["Karthik",5]]]` | `{"Karthik":30,"Divya":30}` | Hidden |
| 5 | Task 2 | `["task2",{"owner":"Asha","balance":0,"topups":[100]}]` | `"Asha: Rs.100"` | Shown |
| 6 | Task 2 | `["task2",{"owner":"Ravi","balance":50,"topups":[20,30]}]` | `"Ravi: Rs.100"` | Hidden |
| 7 | Task 2 | `["task2",{"owner":"Meena","balance":75,"topups":[]}]` | `"Meena: Rs.75"` | Hidden |
| 8 | Task 2 | `["task2",{"owner":"Divya","balance":10,"topups":[5,5,5]}]` | `"Divya: Rs.25"` | Hidden |
| 9 | Task 3 | `["task3a",{"balance":100,"amount":40}]` | `60` | Shown |
| 10 | Task 3 | `["task3a",{"balance":50,"amount":80}]` | `"ERROR: Insufficient balance"` | Hidden |
| 11 | Task 3 | `["task3b",{"balance":100,"amounts":[30,50,40]}]` | `{"paid":2,"failed":1,"balance":20}` | Hidden |
| 12 | Task 3 | `["task3b",{"balance":20,"amounts":[50,10,60]}]` | `{"paid":1,"failed":2,"balance":10}` | Hidden |
