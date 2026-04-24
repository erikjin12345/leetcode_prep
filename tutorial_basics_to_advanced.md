# From Basics to LeetCode — A Progressive Python Tutorial

A tutorial that starts from the ground up and gradually builds toward the patterns you'll need for a quant-trading coding assessment. Each topic goes through three levels:

- **🟢 Basic** — the concept in its simplest form.
- **🟡 Intermediate** — real-world use and common patterns.
- **🔴 Advanced** — LeetCode-level tricks and optimizations.

Work through this end to end. Try to run every example yourself in a Python file or Jupyter notebook before moving on. Typing the code matters more than reading it.

---

# Table of Contents

1. [Variables, Types, and Basic Operations](#1-variables-types-and-basic-operations)
2. [Control Flow: if, for, while](#2-control-flow-if-for-while)
3. [Functions](#3-functions)
4. [Lists](#4-lists)
5. [Strings](#5-strings)
6. [Dictionaries and Sets](#6-dictionaries-and-sets)
7. [Tuples and Unpacking](#7-tuples-and-unpacking)
8. [List Comprehensions](#8-list-comprehensions)
9. [Working with Files and Input](#9-working-with-files-and-input)
10. [Classes and Objects](#10-classes-and-objects)
11. [Recursion](#11-recursion)
12. [Time Complexity (Big-O)](#12-time-complexity-big-o)
13. [Two Pointers](#13-two-pointers)
14. [Sliding Window](#14-sliding-window)
15. [Hash Maps for LeetCode](#15-hash-maps-for-leetcode)
16. [Stacks and Queues](#16-stacks-and-queues)
17. [Binary Search](#17-binary-search)
18. [Sorting](#18-sorting)
19. [Trees and Graphs](#19-trees-and-graphs)
20. [BFS and DFS](#20-bfs-and-dfs)
21. [Dynamic Programming](#21-dynamic-programming)
22. [Heaps / Priority Queues](#22-heaps--priority-queues)
23. [Probability Problems](#23-probability-problems)
24. [Putting It All Together](#24-putting-it-all-together)

---

# 1. Variables, Types, and Basic Operations

## 🟢 Basic

A variable is a name that points to a value. In Python, you don't declare types — they're inferred.

```python
x = 5           # integer
y = 3.14        # float
name = "Erik"   # string
is_valid = True # boolean
nothing = None  # special "absence of value"
```

You can check a type with `type()`:

```python
print(type(x))      # <class 'int'>
print(type(name))   # <class 'str'>
```

Basic arithmetic:

```python
a, b = 7, 3
print(a + b)   # 10
print(a - b)   # 4
print(a * b)   # 21
print(a / b)   # 2.333...  (regular division, always float)
print(a // b)  # 2         (integer division, floor)
print(a % b)   # 1         (modulo / remainder)
print(a ** b)  # 343       (power: 7^3)
```

## 🟡 Intermediate

**Type conversion** is explicit in Python:

```python
s = "42"
n = int(s)          # 42
f = float("3.14")   # 3.14
back_to_s = str(n)  # "42"
```

**Comparisons** return booleans:

```python
print(5 > 3)      # True
print(5 == 5)     # True
print(5 != 3)     # True
```

**Python supports chained comparisons** (unusual among languages):

```python
x = 7
if 0 < x < 10:  # means 0 < x AND x < 10
    print("In range")
```

**Logical operators** are words, not symbols:

```python
a, b = True, False
print(a and b)   # False
print(a or b)    # True
print(not a)     # False
```

## 🔴 Advanced

**Integers in Python have unlimited precision** — no overflow. This is a huge advantage over C++/Java for LeetCode:

```python
huge = 2 ** 1000  # works fine, gives a 302-digit number
```

**Floating-point precision is still finite**, and equality checks can bite you:

```python
print(0.1 + 0.2 == 0.3)  # False!
print(0.1 + 0.2)          # 0.30000000000000004
```

Use `math.isclose()` for float comparisons, or work with integers/Decimals for exact math.

**The walrus operator `:=` (Python 3.8+)** assigns inside expressions:

```python
nums = [1, 2, 3, 4, 5]
if (n := len(nums)) > 3:
    print(f"List has {n} items (more than 3)")
```

---

# 2. Control Flow: if, for, while

## 🟢 Basic

**Indentation matters in Python** — it defines code blocks. Use 4 spaces consistently.

```python
age = 18
if age >= 18:
    print("Adult")
else:
    print("Minor")
```

**for loop** iterates over a sequence:

```python
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

for name in ["Alice", "Bob", "Charlie"]:
    print(name)
```

**while loop** runs as long as a condition is true:

```python
count = 0
while count < 3:
    print(count)
    count += 1  # there's no count++ in Python
```

## 🟡 Intermediate

**`elif`** for multiple conditions:

```python
score = 85
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"
```

**`break` and `continue`** control loop flow:

```python
for i in range(10):
    if i == 5:
        break  # exit the loop entirely
    if i % 2 == 0:
        continue  # skip to next iteration
    print(i)  # prints 1, 3
```

**`for` with `else`** — the `else` runs if the loop completes without `break`:

```python
for x in nums:
    if x < 0:
        print("Found negative")
        break
else:
    print("No negatives")
```

This is unusual but occasionally useful.

## 🔴 Advanced

**Ternary expressions** for one-line conditionals:

```python
x = 10
label = "positive" if x > 0 else "non-positive"
```

**Loop with index using `enumerate`** — much cleaner than `range(len(arr))`:

```python
for i, value in enumerate(nums):
    print(f"Index {i}: {value}")
```

**Iterate two sequences in parallel with `zip`**:

```python
names = ["Alice", "Bob"]
ages = [25, 30]
for name, age in zip(names, ages):
    print(f"{name} is {age}")
```

**Reverse iteration**:

```python
for x in reversed(nums):
    print(x)
```

---

# 3. Functions

## 🟢 Basic

A function groups reusable code:

```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Erik"))  # Hello, Erik!
```

Functions can have multiple parameters and a return value:

```python
def add(a, b):
    return a + b

result = add(3, 5)  # 8
```

Functions without `return` return `None` implicitly.

## 🟡 Intermediate

**Default parameters**:

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Erik")           # "Hello, Erik!"
greet("Erik", "Hej")    # "Hej, Erik!"
```

**Keyword arguments**:

```python
greet(name="Erik", greeting="Hi")  # order doesn't matter when named
```

**Returning multiple values** (actually returns a tuple):

```python
def min_max(nums):
    return min(nums), max(nums)

lo, hi = min_max([3, 1, 4, 1, 5])  # lo=1, hi=5
```

**⚠️ Mutable default argument trap:**

```python
def bad(x, items=[]):  # DON'T DO THIS
    items.append(x)
    return items

print(bad(1))  # [1]
print(bad(2))  # [1, 2]  — the same list is reused!

def good(x, items=None):  # DO THIS
    if items is None:
        items = []
    items.append(x)
    return items
```

## 🔴 Advanced

**`*args` and `**kwargs`** for variable-length arguments:

```python
def flexible(*args, **kwargs):
    print("positional:", args)
    print("keyword:", kwargs)

flexible(1, 2, 3, name="Erik", age=30)
# positional: (1, 2, 3)
# keyword: {'name': 'Erik', 'age': 30}
```

**Lambda expressions** — one-line anonymous functions:

```python
square = lambda x: x ** 2
print(square(5))  # 25

# More commonly used inline:
nums = [3, 1, 4, 1, 5, 9]
sorted_by_distance_to_3 = sorted(nums, key=lambda x: abs(x - 3))
```

**Higher-order functions** — functions that take or return other functions:

```python
def apply_twice(f, x):
    return f(f(x))

print(apply_twice(lambda x: x + 1, 5))  # 7
```

**Memoization with `@functools.cache`** — critical for dynamic programming:

```python
from functools import cache

@cache
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

print(fib(100))  # instant, even though the naive version would take forever
```

---

# 4. Lists

## 🟢 Basic

A list is an ordered, mutable collection:

```python
nums = [3, 1, 4, 1, 5, 9, 2, 6]
print(nums[0])   # 3 (first element)
print(nums[-1])  # 6 (last element)
print(len(nums)) # 8
```

Modify elements by index:

```python
nums[0] = 100
print(nums)  # [100, 1, 4, 1, 5, 9, 2, 6]
```

## 🟡 Intermediate

**Slicing** — extract subsections:

```python
nums = [10, 20, 30, 40, 50]
print(nums[1:4])   # [20, 30, 40]  — indices 1, 2, 3
print(nums[:3])    # [10, 20, 30]  — first three
print(nums[2:])    # [30, 40, 50]  — from index 2 onward
print(nums[-2:])   # [40, 50]      — last two
print(nums[::-1])  # [50, 40, 30, 20, 10]  — reversed
```

**Common methods**:

```python
nums = [3, 1, 4]
nums.append(5)      # [3, 1, 4, 5]
nums.pop()          # returns 5, nums is now [3, 1, 4]
nums.pop(0)         # returns 3, nums is now [1, 4]
nums.insert(0, 99)  # [99, 1, 4]
nums.sort()         # [1, 4, 99] — sorts in place
```

**Checking membership**:

```python
if 4 in nums:
    print("Found 4")
```

## 🔴 Advanced

**⚠️ List aliasing** — assignment doesn't copy:

```python
a = [1, 2, 3]
b = a        # b and a point to the SAME list
b.append(4)
print(a)     # [1, 2, 3, 4]  — a changed too!

# To actually copy:
b = a.copy()    # or a[:], or list(a)
```

**Creating 2D arrays (grids)**:

```python
# WRONG — all rows share the same inner list!
grid = [[0] * 3] * 4  # modifying grid[0][0] changes all rows

# RIGHT — each row is independent
grid = [[0] * 3 for _ in range(4)]
```

**Performance gotchas**:

| Operation | Complexity | Notes |
|---|---|---|
| `arr[i]` | O(1) | Fast random access |
| `arr.append(x)` | O(1) amortized | Fast |
| `arr.pop()` | O(1) | From end |
| `arr.pop(0)` | O(n) | Avoid! Use `deque` |
| `arr.insert(0, x)` | O(n) | Avoid! Use `deque` |
| `x in arr` | O(n) | Use `set` for fast lookup |
| `len(arr)` | O(1) | Fast |

---

# 5. Strings

## 🟢 Basic

Strings are sequences of characters, immutable:

```python
s = "hello"
print(s[0])      # 'h'
print(len(s))    # 5
print(s.upper()) # 'HELLO' — returns a NEW string
```

**Concatenation** with `+`:

```python
full = "hello" + " " + "world"  # "hello world"
```

## 🟡 Intermediate

**Useful methods**:

```python
s = "  Hello, World!  "
print(s.strip())           # "Hello, World!" — removes leading/trailing whitespace
print(s.lower())           # "  hello, world!  "
print(s.replace("World", "Erik"))  # "  Hello, Erik!  "
print(s.split(","))        # ["  Hello", " World!  "]
print(",".join(["a","b","c"]))  # "a,b,c"
```

**Character checks**:

```python
ch = "5"
print(ch.isdigit())  # True
print(ch.isalpha())  # False

ch = "a"
print(ch.isalpha())  # True
```

**f-strings (formatted strings)** — the modern way to build strings:

```python
name = "Erik"
age = 30
print(f"{name} is {age} years old")
print(f"{3.14159:.2f}")  # "3.14"
print(f"{42:05d}")        # "00042"
```

## 🔴 Advanced

**⚠️ String concatenation in loops is O(n²)**:

```python
# BAD — creates a new string each iteration
s = ""
for word in words:
    s += word  # O(n²) total

# GOOD — O(n) total
s = "".join(words)
```

Strings are immutable, so `s += word` creates a brand new string every time.

**Character arithmetic using `ord` and `chr`**:

```python
# Shift each letter by 1 (Caesar cipher)
s = "abc"
shifted = "".join(chr(ord(c) + 1) for c in s)  # "bcd"

# Convert letter to 0-indexed position
def letter_to_index(c):
    return ord(c) - ord('a')  # 'a' -> 0, 'b' -> 1, ...
```

**String slicing is O(k)** where k is the slice length:

```python
s = "abcdefg"
print(s[::-1])     # reverse
print(s[2:5])      # "cde"
```

---

# 6. Dictionaries and Sets

## 🟢 Basic

A **dictionary** (`dict`) maps keys to values:

```python
ages = {"Alice": 30, "Bob": 25}
print(ages["Alice"])  # 30

ages["Charlie"] = 35  # add new entry
print(ages)  # {'Alice': 30, 'Bob': 25, 'Charlie': 35}
```

A **set** stores unique values with fast membership checking:

```python
unique = {1, 2, 3, 3, 2}  # {1, 2, 3}
print(2 in unique)  # True (O(1) lookup)
```

## 🟡 Intermediate

**Iterating dictionaries**:

```python
for key in ages:
    print(key, ages[key])

for key, value in ages.items():
    print(key, value)  # preferred

for value in ages.values():
    print(value)
```

**Checking for a key safely**:

```python
# Raises KeyError if missing
age = ages["Dave"]  # ERROR

# Returns default if missing
age = ages.get("Dave", 0)  # 0

# Check first
if "Dave" in ages:
    age = ages["Dave"]
```

**Common LeetCode pattern — counting frequencies**:

```python
s = "banana"
freq = {}
for c in s:
    freq[c] = freq.get(c, 0) + 1
print(freq)  # {'b': 1, 'a': 3, 'n': 2}
```

## 🔴 Advanced

**`collections.Counter`** — the professional way to count:

```python
from collections import Counter
freq = Counter("banana")  # Counter({'a': 3, 'n': 2, 'b': 1})
print(freq.most_common(1))  # [('a', 3)]
```

**`collections.defaultdict`** — automatic default values:

```python
from collections import defaultdict

# Instead of:
groups = {}
for word in words:
    key = "".join(sorted(word))
    if key not in groups:
        groups[key] = []
    groups[key].append(word)

# Use:
groups = defaultdict(list)
for word in words:
    key = "".join(sorted(word))
    groups[key].append(word)  # auto-creates the list
```

**Set operations**:

```python
a = {1, 2, 3}
b = {2, 3, 4}
print(a | b)  # {1, 2, 3, 4} — union
print(a & b)  # {2, 3} — intersection
print(a - b)  # {1} — difference
print(a ^ b)  # {1, 4} — symmetric difference
```

**When to use what**:
- Need fast lookup? → `set` or `dict`
- Need order + uniqueness? → `dict.fromkeys(iterable)` then list it
- Need to count? → `Counter`
- Need grouping? → `defaultdict(list)`

---

# 7. Tuples and Unpacking

## 🟢 Basic

A tuple is like a list but **immutable** — you can't change it after creation:

```python
point = (3, 5)
print(point[0])  # 3
# point[0] = 10  # ERROR — can't modify
```

Tuples use parentheses but they're optional:

```python
point = 3, 5  # also a tuple
```

## 🟡 Intermediate

**Unpacking** is a key Python feature:

```python
x, y = (3, 5)
print(x, y)  # 3 5

# Swap without a temporary variable
a, b = 1, 2
a, b = b, a
print(a, b)  # 2 1
```

**Unpacking in loops**:

```python
points = [(1, 2), (3, 4), (5, 6)]
for x, y in points:
    print(x, y)
```

## 🔴 Advanced

**Star unpacking** for flexible splits:

```python
nums = [1, 2, 3, 4, 5]
first, *rest = nums
print(first)  # 1
print(rest)   # [2, 3, 4, 5]

*init, last = nums
print(init)   # [1, 2, 3, 4]
print(last)   # 5

first, *middle, last = nums
# first=1, middle=[2,3,4], last=5
```

**Tuples as dict keys** (since they're hashable):

```python
# Great for coordinate-based state
visited = set()
visited.add((3, 5))
if (3, 5) in visited:
    print("already visited")
```

**Named tuples** for readable code:

```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 5)
print(p.x, p.y)  # 3 5
```

---

# 8. List Comprehensions

## 🟢 Basic

A list comprehension builds a list in one expression:

```python
# Instead of:
squares = []
for x in range(10):
    squares.append(x ** 2)

# Write:
squares = [x ** 2 for x in range(10)]
```

## 🟡 Intermediate

**With a filter**:

```python
evens = [x for x in range(20) if x % 2 == 0]
```

**Nested loops**:

```python
pairs = [(x, y) for x in range(3) for y in range(3)]
# [(0,0), (0,1), (0,2), (1,0), (1,1), (1,2), (2,0), (2,1), (2,2)]
```

**Flattening a 2D list**:

```python
grid = [[1, 2, 3], [4, 5, 6]]
flat = [x for row in grid for x in row]
# [1, 2, 3, 4, 5, 6]
```

## 🔴 Advanced

**Dict and set comprehensions**:

```python
square_map = {x: x**2 for x in range(5)}  # {0:0, 1:1, 2:4, 3:9, 4:16}
unique_squares = {x**2 for x in [-2, -1, 0, 1, 2]}  # {0, 1, 4}
```

**Generator expressions** — like list comprehensions but lazy (no memory overhead):

```python
# This doesn't build a list — it computes on demand
total = sum(x**2 for x in range(1_000_000))
```

Use generators with `sum`, `max`, `min`, `any`, `all` when you don't need the whole list.

**Conditional expressions inside comprehensions**:

```python
labels = ["positive" if x > 0 else "non-positive" for x in nums]
```

Note: the `if` here is *inside* the output expression. Compare to filtering:

```python
positive_nums = [x for x in nums if x > 0]  # filter
labels = ["pos" if x > 0 else "neg" for x in nums]  # map
```

---

# 9. Working with Files and Input

## 🟢 Basic

Read input in a LeetCode context: usually the problem provides parameters directly, so you rarely read from stdin. But it's good to know:

```python
x = input()  # reads a line as a string
n = int(input())  # convert to int
```

## 🟡 Intermediate

Reading multiple space-separated values:

```python
a, b = map(int, input().split())
```

Reading a whole list:

```python
nums = list(map(int, input().split()))
```

## 🔴 Advanced

For LeetCode specifically, you rarely need file I/O — functions receive data as arguments. But if you ever practice offline:

```python
with open("input.txt") as f:
    lines = f.readlines()
```

---

# 10. Classes and Objects

## 🟢 Basic

A class is a blueprint for creating objects:

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        return f"{self.name} says woof!"

my_dog = Dog("Rex", 3)
print(my_dog.bark())  # Rex says woof!
```

- `__init__` is the constructor — runs when you create an instance.
- `self` refers to the instance being operated on. Always the first parameter of methods.

## 🟡 Intermediate

**Linked list node — the LeetCode classic**:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# Build a linked list: 1 -> 2 -> 3
head = ListNode(1, ListNode(2, ListNode(3)))

# Traverse
node = head
while node:
    print(node.val)
    node = node.next
```

**Binary tree node** (you'll use this constantly):

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

## 🔴 Advanced

**Dataclasses** — less boilerplate for simple classes (Python 3.7+):

```python
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int

p = Point(3, 5)
print(p)  # Point(x=3, y=5) — __repr__ is automatic
```

**Custom sorting** via comparison methods:

```python
class Task:
    def __init__(self, priority, name):
        self.priority = priority
        self.name = name
    def __lt__(self, other):  # less-than, for sort/heap
        return self.priority < other.priority

tasks = [Task(3, "A"), Task(1, "B"), Task(2, "C")]
tasks.sort()  # sorts by priority
```

---

# 11. Recursion

## 🟢 Basic

A function that calls itself. Every recursion needs a **base case** (to stop) and a **recursive case** (to shrink the problem):

```python
def factorial(n):
    if n == 0:
        return 1          # base case
    return n * factorial(n - 1)  # recursive case

print(factorial(5))  # 120
```

## 🟡 Intermediate

**Fibonacci (naive — exponential time!)**:

```python
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

# fib(40) takes seconds; fib(100) would take eons
```

**Fibonacci with memoization — linear time**:

```python
from functools import cache

@cache
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

print(fib(100))  # instant
```

The `@cache` decorator remembers results — each `fib(k)` is computed once.

**Tree traversal via recursion**:

```python
def inorder(node):
    if not node:
        return
    inorder(node.left)
    print(node.val)
    inorder(node.right)
```

## 🔴 Advanced

**Recursion converts to DP** — the pattern is always:

1. Write the naive recursion.
2. Add `@cache`.
3. (Optional) Rewrite iteratively as "bottom-up" DP for speed.

**Recursion depth limit** — Python's default is 1000. Deep recursion on linked lists or trees can crash:

```python
import sys
sys.setrecursionlimit(10**6)
```

On LeetCode this is usually already handled, but know it exists.

**Tail recursion is NOT optimized** in Python. If recursion depth would be a problem, convert to an iterative loop.

---

# 12. Time Complexity (Big-O)

## 🟢 Basic

Big-O describes how running time grows with input size `n`. A rough ranking:

```
O(1)       — constant, independent of n
O(log n)   — logarithmic (binary search)
O(n)       — linear
O(n log n) — sorting
O(n^2)     — nested loops
O(2^n)     — exponential (brute force subsets)
O(n!)      — factorial (brute force permutations)
```

## 🟡 Intermediate

**Rules of thumb**:

- Dropping an item from the **end** of a list: O(1).
- Dropping from the **front**: O(n) — everything shifts.
- Hash map lookup: O(1) average.
- Binary search: O(log n).
- Sorting: O(n log n).

**What's acceptable for each input size** (in ~1 second at 10^7–10^8 operations/sec):

| n | Acceptable complexity |
|---|---|
| ≤ 10 | O(n!) — full brute force |
| ≤ 20 | O(2^n) — subsets |
| ≤ 500 | O(n^3) |
| ≤ 5,000 | O(n^2) |
| ≤ 10^6 | O(n log n) |
| ≤ 10^8 | O(n) |
| huge (10^18) | O(log n) |

## 🔴 Advanced

**Amortized complexity** — `list.append` is O(1) on average because resizing (when the internal array fills) is rare enough.

**Space complexity** matters too. A 2D DP array for n=10^4 is 10^8 cells — too much memory.

**Constant factors matter in practice.** O(n log n) with small constants often beats O(n) with big ones for small n. But on LeetCode, asymptotic analysis is usually what separates Accepted from Time Limit Exceeded.

---

# 13. Two Pointers

## 🟢 Basic

Use two indices that move through the array, often from opposite ends:

```python
# Check if a string is a palindrome
def is_palindrome(s):
    i, j = 0, len(s) - 1
    while i < j:
        if s[i] != s[j]:
            return False
        i += 1
        j -= 1
    return True
```

## 🟡 Intermediate

**Two Sum (sorted array)** — the classic:

```python
def two_sum(arr, target):
    i, j = 0, len(arr) - 1
    while i < j:
        s = arr[i] + arr[j]
        if s == target:
            return (i, j)
        elif s < target:
            i += 1  # need a larger number
        else:
            j -= 1  # need a smaller number
    return None
```

Why this works: if the current sum is too small, moving `j` left only makes it smaller, so we must move `i` right.

## 🔴 Advanced

**3Sum** builds on 2Sum:

```python
def three_sum(nums):
    nums.sort()
    result = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i-1]:
            continue  # skip duplicates
        j, k = i + 1, len(nums) - 1
        while j < k:
            total = nums[i] + nums[j] + nums[k]
            if total == 0:
                result.append([nums[i], nums[j], nums[k]])
                j += 1
                k -= 1
                while j < k and nums[j] == nums[j-1]: j += 1  # skip dups
            elif total < 0:
                j += 1
            else:
                k -= 1
    return result
```

Complexity: O(n^2) — the outer loop is O(n), the two-pointer sweep inside is O(n).

---

# 14. Sliding Window

## 🟢 Basic

Maintain a window of elements that slides over the array. Two pointers `left` and `right`.

**Fixed-size window — max sum of any k consecutive elements**:

```python
def max_sum_window(arr, k):
    s = sum(arr[:k])  # initial window
    best = s
    for right in range(k, len(arr)):
        s += arr[right] - arr[right - k]  # slide
        best = max(best, s)
    return best
```

## 🟡 Intermediate

**Variable-size window** — expand right, contract left when a condition fails.

**Longest substring without repeating characters**:

```python
def longest_unique(s):
    seen = {}  # char -> last index
    left = 0
    best = 0
    for right, c in enumerate(s):
        if c in seen and seen[c] >= left:
            left = seen[c] + 1  # jump past the repeat
        seen[c] = right
        best = max(best, right - left + 1)
    return best
```

Why O(n): each character is processed once by `right`, and `left` only moves forward.

## 🔴 Advanced

**Smallest subarray with sum ≥ target**:

```python
def min_subarray_len(target, nums):
    left = 0
    total = 0
    best = float('inf')
    for right, x in enumerate(nums):
        total += x
        while total >= target:
            best = min(best, right - left + 1)
            total -= nums[left]
            left += 1
    return 0 if best == float('inf') else best
```

Pattern: expand `right` to accumulate, contract `left` to minimize.

---

# 15. Hash Maps for LeetCode

## 🟢 Basic

**Two Sum (unsorted)** — the most iconic LeetCode problem:

```python
def two_sum(nums, target):
    seen = {}
    for i, x in enumerate(nums):
        if target - x in seen:
            return [seen[target - x], i]
        seen[x] = i
    return []
```

Trick: for each number, check if its complement has been seen. O(n) instead of O(n^2).

## 🟡 Intermediate

**Group anagrams**:

```python
from collections import defaultdict

def group_anagrams(words):
    groups = defaultdict(list)
    for w in words:
        key = "".join(sorted(w))  # anagrams share this key
        groups[key].append(w)
    return list(groups.values())
```

**Subarray sum equals K** — prefix sum + hash map:

```python
def subarray_sum(nums, k):
    count = 0
    prefix_sum = 0
    seen = {0: 1}  # empty prefix has sum 0
    for x in nums:
        prefix_sum += x
        # if (prefix_sum - k) was seen, there's a subarray ending here with sum k
        count += seen.get(prefix_sum - k, 0)
        seen[prefix_sum] = seen.get(prefix_sum, 0) + 1
    return count
```

## 🔴 Advanced

**Hash map + data structure combo** — LRU Cache uses a hash map plus a doubly linked list for O(1) get/put. In Python this is `OrderedDict`:

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.cache = OrderedDict()
        self.cap = capacity
    def get(self, key):
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)  # mark as recently used
        return self.cache[key]
    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.cap:
            self.cache.popitem(last=False)  # evict least recently used
```

---

# 16. Stacks and Queues

## 🟢 Basic

A **stack** is LIFO (last in, first out). Use a Python list:

```python
stack = []
stack.append(1)   # push
stack.append(2)
top = stack.pop() # pop — returns 2
```

A **queue** is FIFO. Use `collections.deque`:

```python
from collections import deque
q = deque()
q.append(1)       # enqueue at right
q.append(2)
front = q.popleft()  # dequeue from left — returns 1
```

## 🟡 Intermediate

**Valid parentheses**:

```python
def is_valid(s):
    stack = []
    pairs = {')': '(', ']': '[', '}': '{'}
    for c in s:
        if c in "([{":
            stack.append(c)
        else:
            if not stack or stack[-1] != pairs[c]:
                return False
            stack.pop()
    return not stack
```

## 🔴 Advanced

**Monotonic stack — Daily Temperatures**:

Given `temps = [73, 74, 75, 71, 69, 72, 76, 73]`, for each day find how many days until a warmer temperature.

```python
def daily_temperatures(temps):
    n = len(temps)
    ans = [0] * n
    stack = []  # indices, temps are monotonically decreasing
    for i, t in enumerate(temps):
        while stack and temps[stack[-1]] < t:
            j = stack.pop()
            ans[j] = i - j
        stack.append(i)
    return ans
```

Each index enters and exits the stack at most once → O(n).

**Monotonic deque — sliding window maximum**:

```python
from collections import deque

def max_sliding_window(nums, k):
    dq = deque()  # indices, values decreasing
    result = []
    for i, x in enumerate(nums):
        while dq and nums[dq[-1]] <= x:
            dq.pop()
        dq.append(i)
        if dq[0] <= i - k:
            dq.popleft()
        if i >= k - 1:
            result.append(nums[dq[0]])
    return result
```

---

# 17. Binary Search

## 🟢 Basic

Only works on **sorted** data. Halves the search space each step.

```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

## 🟡 Intermediate

**`bisect` module** — Python's built-in binary search:

```python
from bisect import bisect_left, bisect_right

arr = [1, 3, 3, 5, 7]
print(bisect_left(arr, 3))   # 1 — first position for 3
print(bisect_right(arr, 3))  # 3 — one past last position for 3
# count of 3's: bisect_right - bisect_left = 2
```

## 🔴 Advanced

**Binary search on the answer** — most powerful LeetCode technique.

Pattern: "Find the smallest X such that [predicate(X) is True]."

**Koko Eating Bananas**: given piles and h hours, find min speed to finish in time.

```python
def min_eating_speed(piles, h):
    def feasible(k):
        return sum((p + k - 1) // k for p in piles) <= h
    
    lo, hi = 1, max(piles)
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid  # try slower
        else:
            lo = mid + 1
    return lo
```

Why it works: feasibility is **monotonic** — if speed k works, any speed > k also works. So we binary search on the smallest working speed.

Recognize this pattern when you see "minimize the maximum..." or "find smallest X such that...".

---

# 18. Sorting

## 🟢 Basic

```python
arr = [3, 1, 4, 1, 5]
arr.sort()              # in-place, arr becomes [1, 1, 3, 4, 5]
new_arr = sorted(arr)   # returns new list, arr unchanged
```

Descending:

```python
arr.sort(reverse=True)
```

## 🟡 Intermediate

**Custom key**:

```python
words = ["apple", "fig", "banana"]
words.sort(key=len)  # by length: ['fig', 'apple', 'banana']

# Sort points by distance from origin
points = [(1,2), (3,4), (0,1)]
points.sort(key=lambda p: p[0]**2 + p[1]**2)
```

**Sort by multiple keys** (tuple comparison):

```python
# First by age ascending, then by name alphabetically
people.sort(key=lambda p: (p.age, p.name))
```

## 🔴 Advanced

**Custom comparators** — when a key function isn't enough:

```python
from functools import cmp_to_key

def compare(a, b):
    # Sort numbers so concatenation is largest: e.g. [3, 30] -> "330" not "303"
    if a + b > b + a:
        return -1  # a before b
    else:
        return 1

nums = ["3", "30", "34", "5", "9"]
nums.sort(key=cmp_to_key(compare))
print("".join(nums))  # "9534330"
```

**Stability** — Python's sort is stable: equal elements keep their original order. Useful for multi-pass sorts.

---

# 19. Trees and Graphs

## 🟢 Basic

**Binary tree node**:

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

**Graph as adjacency list**:

```python
from collections import defaultdict
graph = defaultdict(list)
edges = [(0, 1), (0, 2), (1, 3)]
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)  # undirected
```

## 🟡 Intermediate

**Tree traversals**:

```python
def inorder(node):
    if not node: return
    inorder(node.left)
    print(node.val)
    inorder(node.right)

def preorder(node):
    if not node: return
    print(node.val)
    preorder(node.left)
    preorder(node.right)

def postorder(node):
    if not node: return
    postorder(node.left)
    postorder(node.right)
    print(node.val)
```

**Key fact**: inorder traversal of a BST yields sorted output.

## 🔴 Advanced

**Tree problems often need two return values** — parent needs info from both children:

```python
def diameter(root):
    best = [0]  # list so inner function can mutate
    
    def depth(node):
        if not node: return 0
        l = depth(node.left)
        r = depth(node.right)
        best[0] = max(best[0], l + r)  # path through this node
        return 1 + max(l, r)            # depth for parent
    
    depth(root)
    return best[0]
```

---

# 20. BFS and DFS

## 🟢 Basic

**BFS (breadth-first search)** uses a queue. Explores level by level.

```python
from collections import deque

def bfs(start, graph):
    visited = {start}
    q = deque([start])
    while q:
        node = q.popleft()
        print(node)
        for nb in graph[node]:
            if nb not in visited:
                visited.add(nb)
                q.append(nb)
```

**DFS (depth-first search)** uses recursion (or a stack). Goes deep first.

```python
def dfs(node, graph, visited=None):
    if visited is None:
        visited = set()
    if node in visited: return
    visited.add(node)
    print(node)
    for nb in graph[node]:
        dfs(nb, graph, visited)
```

## 🟡 Intermediate

**BFS for shortest path in unweighted graph**:

```python
def shortest_path(start, target, graph):
    if start == target: return 0
    visited = {start}
    q = deque([(start, 0)])
    while q:
        node, dist = q.popleft()
        for nb in graph[node]:
            if nb == target:
                return dist + 1
            if nb not in visited:
                visited.add(nb)
                q.append((nb, dist + 1))
    return -1
```

**DFS on a grid (Number of Islands)**:

```python
def num_islands(grid):
    if not grid: return 0
    m, n = len(grid), len(grid[0])
    count = 0
    
    def dfs(i, j):
        if i < 0 or i >= m or j < 0 or j >= n: return
        if grid[i][j] != '1': return
        grid[i][j] = '0'  # mark visited
        dfs(i+1, j); dfs(i-1, j); dfs(i, j+1); dfs(i, j-1)
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                count += 1
                dfs(i, j)
    return count
```

## 🔴 Advanced

**Multi-source BFS** — start from many sources simultaneously:

```python
def rotting_oranges(grid):
    m, n = len(grid), len(grid[0])
    q = deque()
    fresh = 0
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 2:
                q.append((i, j, 0))
            elif grid[i][j] == 1:
                fresh += 1
    
    max_time = 0
    while q:
        i, j, t = q.popleft()
        max_time = max(max_time, t)
        for di, dj in [(1,0),(-1,0),(0,1),(0,-1)]:
            ni, nj = i+di, j+dj
            if 0 <= ni < m and 0 <= nj < n and grid[ni][nj] == 1:
                grid[ni][nj] = 2
                fresh -= 1
                q.append((ni, nj, t+1))
    
    return max_time if fresh == 0 else -1
```

---

# 21. Dynamic Programming

## 🟢 Basic

DP breaks a problem into overlapping subproblems and stores their solutions to avoid recomputation.

**Climbing stairs**: you can take 1 or 2 steps at a time. How many ways to reach step n?

```python
def climb(n):
    if n <= 2: return n
    a, b = 1, 2  # ways to reach step 1, step 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b
```

Recurrence: `ways(n) = ways(n-1) + ways(n-2)`.

## 🟡 Intermediate

**House Robber** — can't rob two adjacent houses:

```python
def rob(nums):
    prev2, prev1 = 0, 0
    for x in nums:
        prev2, prev1 = prev1, max(prev1, prev2 + x)
    return prev1
```

Recurrence: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`. Either skip house i or rob it.

**Best Time to Buy and Sell Stock** (one transaction):

```python
def max_profit(prices):
    min_price = float('inf')
    best = 0
    for p in prices:
        min_price = min(min_price, p)
        best = max(best, p - min_price)
    return best
```

## 🔴 Advanced

**Top-down DP with `@cache`** — often clearer than bottom-up:

```python
from functools import cache

def coin_change(coins, amount):
    @cache
    def dp(remaining):
        if remaining == 0: return 0
        if remaining < 0: return float('inf')
        best = float('inf')
        for c in coins:
            best = min(best, 1 + dp(remaining - c))
        return best
    
    result = dp(amount)
    return result if result != float('inf') else -1
```

**State-machine DP** — Best Time to Buy and Sell Stock with Cooldown:

States at end of each day:
- `hold`: holding a share
- `sold`: just sold (tomorrow is cooldown)
- `rest`: not holding, can buy

```python
def max_profit_cooldown(prices):
    hold = -prices[0]
    sold = 0
    rest = 0
    for p in prices[1:]:
        prev_sold = sold
        sold = hold + p
        hold = max(hold, rest - p)
        rest = max(rest, prev_sold)
    return max(sold, rest)
```

---

# 22. Heaps / Priority Queues

## 🟢 Basic

Python's `heapq` is a min-heap (smallest element at top):

```python
import heapq

h = []
heapq.heappush(h, 3)
heapq.heappush(h, 1)
heapq.heappush(h, 4)
print(heapq.heappop(h))  # 1 — smallest
```

## 🟡 Intermediate

**K smallest/largest**:

```python
nums = [3, 1, 4, 1, 5, 9, 2, 6]
print(heapq.nsmallest(3, nums))  # [1, 1, 2]
print(heapq.nlargest(3, nums))   # [9, 6, 5]
```

For top-k streaming: keep a min-heap of size k; if new element beats min, replace.

```python
def kth_largest(nums, k):
    heap = []
    for x in nums:
        heapq.heappush(heap, x)
        if len(heap) > k:
            heapq.heappop(heap)
    return heap[0]  # the smallest of the top-k is the kth largest
```

## 🔴 Advanced

**Max-heap** — Python only has min-heap. Negate values:

```python
h = []
heapq.heappush(h, -5)
heapq.heappush(h, -2)
largest = -heapq.heappop(h)  # 5
```

**Two heaps for median of a stream**:
- Max-heap for the lower half.
- Min-heap for the upper half.
- Rebalance so sizes differ by ≤ 1.

```python
class MedianFinder:
    def __init__(self):
        self.low = []   # max-heap (negated)
        self.high = []  # min-heap

    def addNum(self, num):
        heapq.heappush(self.low, -num)
        heapq.heappush(self.high, -heapq.heappop(self.low))
        if len(self.high) > len(self.low):
            heapq.heappush(self.low, -heapq.heappop(self.high))

    def findMedian(self):
        if len(self.low) > len(self.high):
            return -self.low[0]
        return (-self.low[0] + self.high[0]) / 2
```

---

# 23. Probability Problems

## 🟢 Basic

**Expected value of a die roll**:

```python
E = sum(face / 6 for face in range(1, 7))  # 3.5
```

**Monte Carlo estimation** — simulate many trials:

```python
import random

def estimate_pi(trials=100000):
    inside = 0
    for _ in range(trials):
        x, y = random.random(), random.random()
        if x**2 + y**2 <= 1:
            inside += 1
    return 4 * inside / trials
```

## 🟡 Intermediate

**Probability DP — Knight Probability in Chessboard**:

A knight on an N×N board makes k moves, each uniform over 8 knight moves. Probability it stays on the board?

```python
def knight_prob(n, k, r, c):
    moves = [(2,1),(2,-1),(-2,1),(-2,-1),(1,2),(1,-2),(-1,2),(-1,-2)]
    P = [[0]*n for _ in range(n)]
    P[r][c] = 1.0
    for _ in range(k):
        new_P = [[0]*n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if P[i][j] == 0: continue
                for di, dj in moves:
                    ni, nj = i+di, j+dj
                    if 0 <= ni < n and 0 <= nj < n:
                        new_P[ni][nj] += P[i][j] / 8
        P = new_P
    return sum(sum(row) for row in P)
```

## 🔴 Advanced

**Expected value DP, computed backward**:

Roll a die until running sum exceeds N. Expected final sum?

```python
def expected_sum(N):
    E = [0.0] * (N + 7)
    for s in range(N + 6, N, -1):
        E[s] = s  # if we've already exceeded N, we stop here
    for s in range(N, -1, -1):
        E[s] = sum(E[s + k] for k in range(1, 7)) / 6
    return E[0]

print(expected_sum(0))   # 3.5
print(expected_sum(10))  # ~12.17
```

Recurrence: `E[s] = (1/6) · Σ E[s+k]` for k=1..6. Compute from high s down to 0.

**Indicator variables + linearity of expectation**: for "expected number of events," define an indicator for each event and sum the probabilities. Works even when events are dependent.

---

# 24. Putting It All Together

## A full LeetCode problem, solved top to bottom

**Problem:** Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

**Walk-through of the reasoning**:

1. **Understand:** We want two indices i, j with nums[i] + nums[j] = target.

2. **Brute force:** try every pair — O(n^2). Correct but too slow for large n.

3. **Optimize:** for each number, we want to know if its complement `target - x` is in the array. Set or dict gives O(1) lookup.

4. **Key insight:** one pass suffices if we check as we go and remember what we've seen.

5. **Code**:

```python
def two_sum(nums, target):
    seen = {}  # number -> index
    for i, x in enumerate(nums):
        complement = target - x
        if complement in seen:
            return [seen[complement], i]
        seen[x] = i
    return []
```

6. **Complexity:** O(n) time, O(n) space.

7. **Edge cases:**
   - Empty array → returns [] (loop doesn't execute).
   - No solution → returns [].
   - Duplicate values: we check `seen` before adding, so we don't accidentally match a number with itself.

8. **Test it yourself** before submitting:

```python
print(two_sum([2, 7, 11, 15], 9))   # [0, 1]
print(two_sum([3, 2, 4], 6))        # [1, 2]
print(two_sum([3, 3], 6))           # [0, 1]
print(two_sum([1, 2, 3], 10))       # []
```

## The general workflow for any LeetCode problem

1. **Read the problem twice.** Note the constraints — they hint at the expected complexity.
2. **Think of a brute force first.** It proves you understand the problem.
3. **Look for the bottleneck.** Where's the wasted work in brute force?
4. **Pattern-match.** Does this look like two pointers? Sliding window? DP? BFS?
5. **Write pseudocode or a recurrence before touching the keyboard.**
6. **Implement.**
7. **Test** with the given example, then edge cases (empty, single element, duplicates, negatives, max size).
8. **Analyze complexity.** Will it fit in the limits?

---

# Where to go from here

1. **Read the existing prep docs** in this repo: the topic-by-topic list, the deep-dive on algorithms, and the Python functions reference.

2. **Pick one topic per day** and do 3-5 problems on it. Consistency > cramming.

3. **Use LeetCode.cn for the real assessment environment.** Practice in Chinese UI even if you solve problems mostly in English.

4. **Start every problem from scratch.** Don't read editorials until you've spent 25 minutes trying.

5. **Keep a "patterns notebook"** — after each problem, write one line: "Key insight was X." Review before the real test.

Good luck with the Haoxing assessment — the combination of your math background and the preparation you're putting in is a strong one. 加油!
