# Q3. Hostel Room Allocation

**Topics**

- **Task 1:** String indexing, dictionary counting
- **Task 2:** Class with a list attribute, f-string
- **Task 3:** Two raise conditions, `try/except` in a loop

## Problem statement

The hostel warden allocates students to rooms. Room numbers start with the block letter; for example, A101 is in Block A. Each room has a fixed capacity.

Complete the code below. The pre-written `solve()` function calls your code for each task. Do not edit `solve()`.

### Task 1

Complete `count_by_block(rooms)`. `rooms` is a list of room numbers like `"A101"`. Return a dictionary showing how many rooms are in each block (the first letter).

**Example:** `["A101", "B202", "A102"]` → `{"A": 2, "B": 1}`

### Task 2

Complete the `Room` class:

- `__init__(number, capacity)` stores both and creates an empty `students` list.
- `add_student(name)` adds the name to the list and returns how many students are now in the room.
- `status()` returns a string like `"A101: 2/3"` (students / capacity).

### Task 3

- Complete `allocate(name)`. If the room is full, raise `ValueError("Room full")`. If the name is already in the room, raise `ValueError("Already allocated")`. Otherwise add the student and return the number of students.
- Complete `allocate_all(room, names)`. Try `room.allocate()` for each name. Return `{"allocated": <count>, "rejected": [names that failed]}`.

**Execution mode:** Write a function  
**Entry point:** `solve`

## Starter code

```python
def count_by_block(rooms):
    # take the first character of each room number
    pass


class Room:
    def __init__(self, number, capacity):
        # Task 2: store number and capacity, create an empty students list
        pass

    def add_student(self, name):
        # Task 2: add the name, return the number of students
        pass

    def status(self):
        # Task 2: return a string like "A101: 2/3"
        pass

    def allocate(self, name):
        # Task 3: raise ValueError("Room full") or ValueError("Already allocated")
        pass


def allocate_all(room, names):
    # Task 3: use try/except around room.allocate()
    pass


# Do not edit below this line
def solve(task, data):
    if task == "task1":
        return count_by_block(data)
    if task == "task2":
        room = Room(data["number"], data["capacity"])
        for name in data["names"]:
            room.add_student(name)
        return room.status()
    if task == "task3a":
        room = Room("T1", data["capacity"])
        room.students = list(data["students"])
        try:
            return room.allocate(data["name"])
        except ValueError as e:
            return "ERROR: " + str(e)
    if task == "task3b":
        room = Room("T1", data["capacity"])
        room.students = list(data["students"])
        return allocate_all(room, data["names"])
```

## Reference solution (author only)

```python
def count_by_block(rooms):
    counts = {}
    for room in rooms:
        block = room[0]
        if block in counts:
            counts[block] += 1
        else:
            counts[block] = 1
    return counts


class Room:
    def __init__(self, number, capacity):
        self.number = number
        self.capacity = capacity
        self.students = []

    def add_student(self, name):
        self.students.append(name)
        return len(self.students)

    def status(self):
        return f"{self.number}: {len(self.students)}/{self.capacity}"

    def allocate(self, name):
        if len(self.students) >= self.capacity:
            raise ValueError("Room full")
        if name in self.students:
            raise ValueError("Already allocated")
        self.students.append(name)
        return len(self.students)


def allocate_all(room, names):
    allocated = 0
    rejected = []
    for name in names:
        try:
            room.allocate(name)
            allocated += 1
        except ValueError:
            rejected.append(name)
    return {"allocated": allocated, "rejected": rejected}
```

## Test cases (each worth 1 of 12)

| # | Task | Arguments (JSON) | Expected (JSON) | Visibility |
|---|---|---|---|---|
| 1 | Task 1 | `["task1",["A101","B202","A102"]]` | `{"A":2,"B":1}` | Shown |
| 2 | Task 1 | `["task1",["C301"]]` | `{"C":1}` | Hidden |
| 3 | Task 1 | `["task1",[]]` | `{}` | Hidden |
| 4 | Task 1 | `["task1",["A1","B1","C1","B2","C2","C3"]]` | `{"A":1,"B":2,"C":3}` | Hidden |
| 5 | Task 2 | `["task2",{"number":"A101","capacity":3,"names":["Asha","Meena"]}]` | `"A101: 2/3"` | Shown |
| 6 | Task 2 | `["task2",{"number":"B202","capacity":2,"names":[]}]` | `"B202: 0/2"` | Hidden |
| 7 | Task 2 | `["task2",{"number":"C301","capacity":4,"names":["Ravi"]}]` | `"C301: 1/4"` | Hidden |
| 8 | Task 2 | `["task2",{"number":"D110","capacity":3,"names":["Divya","Priya","Kavya"]}]` | `"D110: 3/3"` | Hidden |
| 9 | Task 3 | `["task3a",{"capacity":3,"students":["Asha"],"name":"Meena"}]` | `2` | Shown |
| 10 | Task 3 | `["task3a",{"capacity":2,"students":["Asha","Meena"],"name":"Ravi"}]` | `"ERROR: Room full"` | Hidden |
| 11 | Task 3 | `["task3b",{"capacity":3,"students":["Asha"],"names":["Ravi","Asha","Divya","Kavya"]}]` | `{"allocated":2,"rejected":["Asha","Kavya"]}` | Hidden |
| 12 | Task 3 | `["task3b",{"capacity":2,"students":[],"names":["Ravi","Ravi","Arun"]}]` | `{"allocated":2,"rejected":["Ravi"]}` | Hidden |
