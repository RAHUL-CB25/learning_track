# Python Built-in Methods — Quick Interview Cheat Sheet

## STRING `str`

| Method | Functionality |
|---|---|
| `upper()` | Converts string to uppercase |
| `lower()` | Converts string to lowercase |
| `title()` | Makes first letter of each word uppercase |
| `capitalize()` | Makes first character uppercase |
| `strip()` | Removes spaces from both ends |
| `lstrip()` | Removes spaces from left |
| `rstrip()` | Removes spaces from right |
| `split()` | Splits string into a list |
| `join()` | Joins iterable elements into a string |
| `replace()` | Replaces part of a string |
| `find()` | Returns index; `-1` if not found |
| `index()` | Returns index; raises error if not found |
| `count()` | Counts occurrences |
| `startswith()` | Checks beginning of string |
| `endswith()` | Checks ending of string |
| `isalpha()` | Checks whether all characters are alphabets |
| `isdigit()` | Checks whether all characters are digits |
| `isalnum()` | Checks alphabets + digits |
| `isspace()` | Checks whether all characters are whitespace |

---

## LIST `list`

| Method | Functionality |
|---|---|
| `append(x)` | Adds one element at the end |
| `extend(x)` | Adds multiple elements |
| `insert(i, x)` | Inserts element at index |
| `remove(x)` | Removes first matching value |
| `pop(i)` | Removes and returns element at index |
| `clear()` | Removes all elements |
| `index(x)` | Returns first index of value |
| `count(x)` | Counts occurrences |
| `sort()` | Sorts list in-place |
| `reverse()` | Reverses list in-place |
| `copy()` | Creates a shallow copy |

---

## TUPLE `tuple`

Tuple is **ordered + immutable**.

| Method | Functionality |
|---|---|
| `count(x)` | Counts occurrences |
| `index(x)` | Returns first index of value |

Common built-ins:
```python
len(t)       # number of elements
min(t)       # smallest value
max(t)       # largest value
sum(t)       # total
sorted(t)    # returns sorted list
```

---

## SET `set`

| Method | Functionality |
|---|---|
| `add(x)` | Adds one element |
| `update(x)` | Adds multiple elements |
| `remove(x)` | Removes element; error if absent |
| `discard(x)` | Removes element; no error if absent |
| `pop()` | Removes and returns an arbitrary element |
| `clear()` | Removes all elements |
| `union()` | Combines unique elements |
| `intersection()` | Returns common elements |
| `difference()` | Returns elements only in first set |
| `symmetric_difference()` | Returns elements in either set, not both |
| `issubset()` | Checks if set is contained in another |
| `issuperset()` | Checks if set contains another |
| `isdisjoint()` | Checks if sets have no common elements |
| `copy()` | Creates a shallow copy |

Operators:
```python
a | b     # union
a & b     # intersection
a - b     # difference
a ^ b     # symmetric difference
```

---

## DICTIONARY `dict`

| Method | Functionality |
|---|---|
| `get(key)` | Gets value safely |
| `keys()` | Returns all keys |
| `values()` | Returns all values |
| `items()` | Returns key-value pairs |
| `update()` | Adds/updates key-value pairs |
| `pop(key)` | Removes key and returns value |
| `popitem()` | Removes and returns last key-value pair |
| `setdefault()` | Gets key; inserts default if missing |
| `clear()` | Removes all items |
| `copy()` | Creates a shallow copy |

Example:
```python
d = {"name": "Rahul", "age": 22}

d.get("name")          # Rahul
d.keys()               # all keys
d.values()             # all values
d.items()              # key-value pairs
d.update({"age": 23})  # update value
d.pop("age")           # remove key
```

---

# IMPORTANT BUILT-IN FUNCTIONS

| Function | Functionality |
|---|---|
| `len()` | Returns number of elements |
| `type()` | Returns data type |
| `isinstance()` | Checks object type |
| `sum()` | Returns total |
| `min()` | Returns smallest value |
| `max()` | Returns largest value |
| `abs()` | Returns absolute value |
| `round()` | Rounds a number |
| `sorted()` | Returns a new sorted list |
| `reversed()` | Returns reversed iterator |
| `enumerate()` | Gives index + value |
| `zip()` | Combines iterables |
| `map()` | Applies function to each element |
| `filter()` | Filters elements by condition |
| `any()` | `True` if at least one item is true |
| `all()` | `True` if every item is true |
| `range()` | Generates number sequence |

---

# FUNCTIONS

```python
def add(a, b):
    return a + b

def test(*args):
    # args → tuple of positional arguments
    pass

def test(**kwargs):
    # kwargs → dictionary of keyword arguments
    pass

lambda x: x * x
```

```python
list(map(lambda x: x * 2, data))
list(filter(lambda x: x % 2 == 0, data))
```

---

# OOP QUICK REVISION

```text
Class          → Blueprint for objects
Object         → Instance of class
Encapsulation  → Controls access to data
Inheritance    → Reuses parent class functionality
Polymorphism   → Same interface, different behavior
Abstraction    → Hides implementation details
```

### Method Types

```python
def show(self):
    pass                  # Instance method

@classmethod
def show(cls):
    pass                  # Class method

@staticmethod
def add(a, b):
    return a + b          # Static method
```

### Access Modifiers

```python
self.name       # Public
self._name      # Protected convention
self.__name     # Private/name mangling
```

### Inheritance

```python
class Child(Parent):
    def __init__(self):
        super().__init__()
```

### Abstraction

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass
```

---

# ADVANCED PYTHON

```text
Comprehension → Short way to create collections
Iterator      → Object used with next()
Generator     → Uses yield; produces values lazily
Decorator     → Modifies/enhances a function
Exception     → Handles runtime errors
```

### Comprehension

```python
[x*x for x in nums]
{x*x for x in nums}
{x:x*x for x in nums}
[x for x in nums if x % 2 == 0]
```

### Exception

```python
try:
    ...
except ValueError:
    ...
else:
    ...
finally:
    ...
```

### Generator

```python
def numbers():
    for i in range(5):
        yield i
```

---

# DSA / COLLECTIONS

```python
from collections import Counter, defaultdict, deque
```

```python
Counter(arr)                    # Frequency count
Counter(arr).most_common()      # Most frequent elements

d = defaultdict(list)            # Default value for missing key
d["a"].append(1)

q = deque()
q.append(x)
q.popleft()                      # Queue → FIFO

stack = []
stack.append(x)
stack.pop()                      # Stack → LIFO
```

### Heap

```python
import heapq

heapq.heappush(heap, x)          # Insert
heapq.heappop(heap)              # Remove minimum
heapq.heapify(arr)               # Convert to min-heap
heapq.nlargest(k, arr)           # k largest
heapq.nsmallest(k, arr)          # k smallest
```

### Binary Search

```python
import bisect

bisect.bisect_left(arr, x)
bisect.bisect_right(arr, x)
```

---

# COMMON INTERVIEW DIFFERENCES

```text
==          → Value comparison
is          → Object identity

append()    → Adds one object
extend()    → Adds multiple elements

remove()    → Removes by value
pop()       → Removes by index / last

sort()      → Modifies original list
sorted()    → Returns new sorted list

list        → Mutable
tuple       → Immutable

set         → Unique values
dict        → Key-value pairs

*args       → Tuple
**kwargs    → Dictionary
```

---

# SOLID PRINCIPLES

```text
S → Single Responsibility
    One class should have one main responsibility.

O → Open/Closed
    Open for extension, closed for modification.

L → Liskov Substitution
    Child should safely replace parent.

I → Interface Segregation
    Don't force unused methods on a class.

D → Dependency Inversion
    Depend on abstractions, not concrete implementations.
```

# 1-MINUTE REVISION

```text
LIST       → []       → Ordered, Mutable, Duplicates
TUPLE      → ()       → Ordered, Immutable
SET        → set()    → Unique values
DICT       → {}       → Key : Value

STACK      → append/pop       → LIFO
QUEUE      → deque/popleft    → FIFO
HEAP       → heapq            → Priority Queue
COUNTER    → Frequency count
```
