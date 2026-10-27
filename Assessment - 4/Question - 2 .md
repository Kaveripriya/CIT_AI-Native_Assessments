# Q2. Library Book Lending

**Topics**

- **Task 1:** Loop, dictionary counting
- **Task 2:** Class, default argument, f-string
- **Task 3:** `raise ValueError`, `try/except`, building two lists

## Problem statement

The department library wants a small program to track how many books each student has borrowed and how many copies of each book are left on the shelf.

Complete the code below. The pre-written `solve()` function calls your code for each task. Do not edit `solve()`.

### Task 1

Complete `count_borrowed(records)`. `records` is a list of `[student, book_title]` pairs. Return a dictionary showing how many books each student borrowed.

**Example:** `[["Asha", "Python 101"], ["Ravi", "DBMS"], ["Asha", "Networks"]]` → `{"Asha": 2, "Ravi": 1}`

### Task 2

Complete the `Book` class:

- `__init__(title, copies=1)` stores the title and the number of copies.
- `add_copies(n)` adds `n` copies and returns the new count.
- `info()` returns a string like `"DBMS (3 copies)"`.

### Task 3

- Complete `issue()`. If there are no copies left, raise `ValueError("No copies left")`. Otherwise reduce the copies by 1 and return the copies left.
- Complete `handle_requests(book, students)`. For each student, try `book.issue()`. Students who get the book go into an `issued` list; students who don't go into a `waitlist` list. Return `{"issued": [...], "waitlist": [...]}`.

**Execution mode:** Write a function  
**Entry point:** `solve`

## Starter code

```python
def count_borrowed(records):
    # use a loop and a dictionary to count
    pass


class Book:
    def __init__(self, title, copies=1):
        # Task 2: store title and copies
        pass

    def add_copies(self, n):
        # Task 2: add n copies, return the new count
        pass

    def info(self):
        # Task 2: return a string like "DBMS (3 copies)"
        pass

    def issue(self):
        # Task 3: raise ValueError("No copies left") when copies is 0
        pass


def handle_requests(book, students):
    # Task 3: use try/except around book.issue()
    pass


# Do not edit below this line
def solve(task, data):
    if task == "task1":
        return count_borrowed(data)
    if task == "task2":
        book = Book(data["title"], data["copies"])
        for n in data["added"]:
            book.add_copies(n)
        return book.info()
    if task == "task3a":
        book = Book("Test Book", data["copies"])
        try:
            return book.issue()
        except ValueError as e:
            return "ERROR: " + str(e)
    if task == "task3b":
        book = Book("Test Book", data["copies"])
        return handle_requests(book, data["students"])
```

## Reference solution (author only)

```python
def count_borrowed(records):
    counts = {}
    for student, title in records:
        if student in counts:
            counts[student] += 1
        else:
            counts[student] = 1
    return counts


class Book:
    def __init__(self, title, copies=1):
        self.title = title
        self.copies = copies

    def add_copies(self, n):
        self.copies = self.copies + n
        return self.copies

    def info(self):
        return f"{self.title} ({self.copies} copies)"

    def issue(self):
        if self.copies == 0:
            raise ValueError("No copies left")
        self.copies = self.copies - 1
        return self.copies


def handle_requests(book, students):
    issued = []
    waitlist = []
    for student in students:
        try:
            book.issue()
            issued.append(student)
        except ValueError:
            waitlist.append(student)
    return {"issued": issued, "waitlist": waitlist}
```

## Test cases (each worth 1 of 12)

| # | Task | Arguments (JSON) | Expected (JSON) | Visibility |
|---|---|---|---|---|
| 1 | Task 1 | `["task1",[["Asha","Python 101"],["Ravi","DBMS"],["Asha","Networks"]]]` | `{"Asha":2,"Ravi":1}` | Shown |
| 2 | Task 1 | `["task1",[["Meena","OS"]]]` | `{"Meena":1}` | Hidden |
| 3 | Task 1 | `["task1",[]]` | `{}` | Hidden |
| 4 | Task 1 | `["task1",[["Divya","A"],["Karthik","B"],["Divya","C"],["Divya","D"],["Karthik","E"]]]` | `{"Divya":3,"Karthik":2}` | Hidden |
| 5 | Task 2 | `["task2",{"title":"DBMS","copies":1,"added":[2]}]` | `"DBMS (3 copies)"` | Shown |
| 6 | Task 2 | `["task2",{"title":"Python 101","copies":4,"added":[]}]` | `"Python 101 (4 copies)"` | Hidden |
| 7 | Task 2 | `["task2",{"title":"Networks","copies":0,"added":[1,1,3]}]` | `"Networks (5 copies)"` | Hidden |
| 8 | Task 2 | `["task2",{"title":"OS","copies":2,"added":[10]}]` | `"OS (12 copies)"` | Hidden |
| 9 | Task 3 | `["task3a",{"copies":3}]` | `2` | Shown |
| 10 | Task 3 | `["task3a",{"copies":0}]` | `"ERROR: No copies left"` | Hidden |
| 11 | Task 3 | `["task3b",{"copies":2,"students":["Asha","Ravi","Meena"]}]` | `{"issued":["Asha","Ravi"],"waitlist":["Meena"]}` | Hidden |
| 12 | Task 3 | `["task3b",{"copies":0,"students":["Divya","Karthik"]}]` | `{"issued":[],"waitlist":["Divya","Karthik"]}` | Hidden |
