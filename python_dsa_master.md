# 🐍 Python + DSA Master Reference Guide
## Complete FAANG Preparation — Language Mastery + Every Data Structure & Algorithm

---

## 📂 Table of Contents

### Part 1 — Python Language Mastery
1. [Python Basics & Built-in Types](#1-python-basics--built-in-types)
2. [Strings — All Methods & Patterns](#2-strings--all-methods--patterns)
3. [Lists — All Operations & Complexity](#3-lists--all-operations--complexity)
4. [Tuples](#4-tuples)
5. [Dictionaries — All Operations & Complexity](#5-dictionaries--all-operations--complexity)
6. [Sets — All Operations & Complexity](#6-sets--all-operations--complexity)
7. [Collections Module](#7-collections-module)
8. [Comprehensions & Generators](#8-comprehensions--generators)
9. [Functions — args, kwargs, lambdas, decorators](#9-functions--args-kwargs-lambdas-decorators)
10. [OOP — Classes, Inheritance, Dunder Methods](#10-oop--classes-inheritance-dunder-methods)
11. [itertools & functools](#11-itertools--functools)
12. [File I/O & Exception Handling](#12-file-io--exception-handling)
13. [Type Hints & Typing Module](#13-type-hints--typing-module)

### Part 2 — DSA in Python
14. [Big-O Complexity Cheat Sheet](#14-big-o-complexity-cheat-sheet)
15. [Arrays & Lists — Patterns](#15-arrays--lists--patterns)
16. [Strings — Interview Patterns](#16-strings--interview-patterns)
17. [Linked Lists](#17-linked-lists)
18. [Stacks](#18-stacks)
19. [Queues & Deque](#19-queues--deque)
20. [Hash Maps & Hash Sets](#20-hash-maps--hash-sets)
21. [Binary Trees & Traversals](#21-binary-trees--traversals)
22. [Binary Search Trees (BST)](#22-binary-search-trees-bst)
23. [Heaps & Priority Queues](#23-heaps--priority-queues)
24. [Graphs — All Algorithms](#24-graphs--all-algorithms)
25. [Tries](#25-tries)
26. [Sorting Algorithms](#26-sorting-algorithms)
27. [Binary Search — All Variants](#27-binary-search--all-variants)
28. [Dynamic Programming](#28-dynamic-programming)
29. [Recursion & Backtracking](#29-recursion--backtracking)
30. [Greedy Algorithms](#30-greedy-algorithms)

---

# PART 1 — Python Language Mastery

---

## 1. Python Basics & Built-in Types

### Numbers: int, float, complex

```python
# int: arbitrary precision (no overflow in Python!)
x = 10
y = -5
big = 10 ** 100          # Works fine — Python int is unlimited precision

# Useful int operations
abs(-7)                  # 7
pow(2, 10)               # 1024
pow(2, 10, 1000)         # 24  (modular exponentiation: (2^10) % 1000)
divmod(17, 5)            # (3, 2) — (quotient, remainder)
bin(10)                  # '0b1010'   (binary string)
oct(10)                  # '0o12'     (octal string)
hex(255)                 # '0xff'     (hex string)
int('1010', 2)           # 10         (parse binary string to int)
int('ff', 16)            # 255        (parse hex string to int)

# float
f = 3.14
round(3.14159, 2)        # 3.14
import math
math.floor(3.9)          # 3
math.ceil(3.1)           # 4
math.sqrt(16)            # 4.0
math.log(100, 10)        # 2.0
math.log2(8)             # 3.0
math.log10(1000)         # 3.0
math.inf                 # positive infinity
-math.inf                # negative infinity
float('inf')             # same as math.inf

# Infinity tricks for FAANG problems
min_val = float('inf')   # Use as initial "minimum" in search problems
max_val = float('-inf')  # Use as initial "maximum" in search problems
```

---

### Boolean

```python
# bool is a subclass of int in Python!
True + True              # 2
True * 5                 # 5
int(True)                # 1
int(False)               # 0

# Truthy / Falsy values
# FALSY: 0, 0.0, "", [], {}, set(), None, False
# TRUTHY: everything else
bool([])                 # False
bool([0])                # True  (non-empty list is truthy even if contains 0)
bool("")                 # False
bool("0")                # True  (non-empty string)
```

---

### Variables & Assignment

```python
# Multiple assignment
a, b = 1, 2
a, b = b, a              # Swap without temp variable — Pythonic!

# Augmented assignment
x += 1; x -= 1; x *= 2; x //= 3; x **= 2; x %= 5

# Walrus operator := (Python 3.8+)
# Assigns and returns value in one expression
import re
if m := re.search(r'\d+', 'abc123'):
    print(m.group())     # '123' — m is usable here

# Walrus in while loop
while chunk := file.read(1024):
    process(chunk)
```

---

### Built-in Functions Reference

```python
# Input / Output
print("hello", "world", sep=", ", end="!\n")  # hello, world!
x = int(input("Enter: "))

# Type checking & conversion
type(3.14)               # <class 'float'>
isinstance(3, int)       # True
isinstance(3, (int, float))  # True — checks multiple types

int("42")                # 42
float("3.14")            # 3.14
str(100)                 # "100"
bool(0)                  # False
list("abc")              # ['a', 'b', 'c']
tuple([1, 2, 3])         # (1, 2, 3)
set([1, 1, 2, 3])        # {1, 2, 3}

# Sequence functions
len([1, 2, 3])           # 3
min([3, 1, 4, 1, 5])     # 1
max([3, 1, 4, 1, 5])     # 5
sum([1, 2, 3, 4])        # 10
sorted([3, 1, 2])        # [1, 2, 3]  — returns NEW list
sorted([3, 1, 2], reverse=True)   # [3, 2, 1]
sorted(["banana","apple","cherry"], key=len)  # sorted by string length

# enumerate: get index AND value
for i, val in enumerate(['a', 'b', 'c'], start=1):
    print(i, val)        # 1 a, 2 b, 3 c

# zip: pair multiple iterables
for a, b in zip([1, 2, 3], ['x', 'y', 'z']):
    print(a, b)          # 1 x, 2 y, 3 z
list(zip([1,2], [3,4], [5,6]))    # [(1,3,5), (2,4,6)]

# map: apply function to every element
list(map(int, ['1', '2', '3']))   # [1, 2, 3]
list(map(lambda x: x**2, [1,2,3]))  # [1, 4, 9]

# filter: keep elements where function returns True
list(filter(lambda x: x % 2 == 0, [1,2,3,4,5]))  # [2, 4]

# any / all
any([0, 0, 1])           # True  (at least one truthy)
all([1, 1, 1])           # True  (all truthy)
any([0, 0, 0])           # False
all([1, 0, 1])           # False

# range
list(range(5))           # [0, 1, 2, 3, 4]
list(range(2, 8))        # [2, 3, 4, 5, 6, 7]
list(range(0, 10, 2))    # [0, 2, 4, 6, 8]
list(range(10, 0, -1))   # [10, 9, ..., 1]

# reversed
list(reversed([1, 2, 3]))  # [3, 2, 1]

# id: memory address of object (useful for understanding mutability)
a = [1, 2, 3]
b = a
id(a) == id(b)           # True — same object in memory
```

---

## 2. Strings — All Methods & Patterns

### String Basics

```python
s = "Hello, World!"

# Immutable — every operation returns a NEW string
len(s)                   # 13
s[0]                     # 'H'
s[-1]                    # '!'
s[0:5]                   # 'Hello'
s[7:]                    # 'World!'
s[::-1]                  # '!dlroW ,olleH'  (reverse string)
s[::2]                   # 'Hlo ol'         (every 2nd char)

# String is iterable
for char in s:
    print(char)

# Check containment
'World' in s             # True
'xyz' not in s           # True
```

---

### All String Methods

```python
s = "  Hello, World!  "

# Case methods
s.upper()                # '  HELLO, WORLD!  '
s.lower()                # '  hello, world!  '
s.title()                # '  Hello, World!  '
s.swapcase()             # '  hELLO, wORLD!  '
s.capitalize()           # '  hello, world!  ' (only first char upper)

# Whitespace / stripping
s.strip()                # 'Hello, World!'     (both sides)
s.lstrip()               # 'Hello, World!  '   (left only)
s.rstrip()               # '  Hello, World!'   (right only)
s.strip('!H')            # strips chars from both ends until char not in set

# Search methods
s2 = "Hello, World!"
s2.find('o')             # 4   (first occurrence index, -1 if not found)
s2.rfind('o')            # 8   (last occurrence index)
s2.index('o')            # 4   (like find but raises ValueError if not found)
s2.count('l')            # 3   (count occurrences)
s2.startswith('Hello')   # True
s2.endswith('!')         # True

# Replace / split / join
s2.replace('World', 'Python')     # 'Hello, Python!'
s2.replace('l', 'L', 2)           # 'HeLLo, World!'  (max 2 replacements)
s2.split(', ')           # ['Hello', 'World!']
s2.split()               # ['Hello,', 'World!']  (splits on whitespace)
'a,b,c'.split(',', 1)    # ['a', 'b,c']  (max 1 split)
', '.join(['Hello', 'World'])  # 'Hello, World'

# Check type of content
"abc".isalpha()          # True  (all alphabetic)
"123".isdigit()          # True  (all digits)
"abc123".isalnum()       # True  (all alphanumeric)
"   ".isspace()          # True  (all whitespace)
"HELLO".isupper()        # True
"hello".islower()        # True

# Alignment / padding
"hello".center(11, '-')  # '---hello---'
"hello".ljust(10, '.')   # 'hello.....'
"hello".rjust(10, '.')   # '.....hello'
"42".zfill(5)            # '00042'  (zero-pad)

# Encoding
"hello".encode('utf-8')  # b'hello'  (bytes)
b'hello'.decode('utf-8') # 'hello'   (string)

# Partition: splits into exactly 3 parts (before, sep, after)
"hello=world".partition('=')   # ('hello', '=', 'world')
"hello=world".rpartition('=')  # ('hello', '=', 'world')

# Translate / maketrans (character-level replacement)
table = str.maketrans('aeiou', '*****')  # replace vowels with *
"hello world".translate(table)            # 'h*ll* w*rld'
```

---

### String Formatting

```python
name, age, score = "Alice", 25, 98.6

# f-strings (preferred — fastest)
f"Name: {name}, Age: {age}, Score: {score:.2f}"
f"{age:05d}"             # '00025' (zero-padded width 5)
f"{score:>10.2f}"        # right-align, 2 decimal places
f"{1000000:,}"           # '1,000,000' (comma separator)
f"{'hello':^20}"         # centered in 20 chars
f"{255:#x}"              # '0xff' (hex with prefix)
f"{0b1010}"              # '10' (binary literal evaluates to int)

# .format()
"{0} is {1} years old".format(name, age)

# % formatting (old style)
"%s is %d years old" % (name, age)
```

---

### Common String Patterns in FAANG

```python
from collections import Counter

# Frequency counter — used in anagram / window problems
s = "anagram"
freq = Counter(s)        # Counter({'a': 3, 'n': 1, 'g': 1, 'r': 1, 'm': 1})

# Check anagram
def is_anagram(s1, s2):
    return Counter(s1) == Counter(s2)

# Reverse words
"Hello World".split()[::-1]          # ['World', 'Hello']
' '.join("Hello World".split()[::-1])  # 'World Hello'

# Check palindrome
def is_palindrome(s):
    s = s.lower()
    s = ''.join(c for c in s if c.isalnum())
    return s == s[::-1]

# ASCII value
ord('A')                 # 65
ord('a')                 # 97
chr(65)                  # 'A'
chr(97)                  # 'a'
# Trick: normalize letters
ord('c') - ord('a')      # 2  (0-indexed position in alphabet)
```

---

## 3. Lists — All Operations & Complexity

### List Basics

```python
lst = [1, 2, 3, 4, 5]

# Creation
lst1 = []                          # empty
lst2 = [0] * 5                     # [0, 0, 0, 0, 0]
lst3 = list(range(1, 6))           # [1, 2, 3, 4, 5]
lst4 = [i**2 for i in range(5)]    # [0, 1, 4, 9, 16]

# 2D list (matrix) — CORRECT way
matrix = [[0] * 3 for _ in range(3)]   # 3x3 zero matrix
# WRONG: matrix = [[0]*3]*3            # All rows are same object!

# Indexing & slicing
lst[0]                   # 1      O(1)
lst[-1]                  # 5      O(1)
lst[1:3]                 # [2, 3] O(k) where k = slice length
lst[::-1]                # [5, 4, 3, 2, 1]  reverse
lst[::2]                 # [1, 3, 5]  every 2nd
```

---

### All List Methods with Time Complexity

| Method | Description | Time Complexity |
| :--- | :--- | :--- |
| `lst.append(x)` | Add element to END | **O(1)** amortized |
| `lst.insert(i, x)` | Insert at position i | **O(n)** (shifts elements) |
| `lst.extend(iterable)` | Add all from iterable | **O(k)** k = len(iterable) |
| `lst.pop()` | Remove & return LAST element | **O(1)** |
| `lst.pop(i)` | Remove & return element at i | **O(n)** (shifts elements) |
| `lst.remove(x)` | Remove FIRST occurrence of x | **O(n)** |
| `lst.index(x)` | Find index of FIRST x | **O(n)** |
| `lst.count(x)` | Count occurrences of x | **O(n)** |
| `lst.sort()` | Sort IN PLACE (Timsort) | **O(n log n)** |
| `sorted(lst)` | Return new sorted list | **O(n log n)** |
| `lst.reverse()` | Reverse IN PLACE | **O(n)** |
| `reversed(lst)` | Return reverse iterator | **O(1)** |
| `lst.copy()` | Shallow copy | **O(n)** |
| `lst.clear()` | Remove all elements | **O(n)** |
| `x in lst` | Check membership | **O(n)** |
| `len(lst)` | Get length | **O(1)** |
| `lst[i] = x` | Set element | **O(1)** |
| `lst[i]` | Get element | **O(1)** |

```python
lst = [3, 1, 4, 1, 5, 9, 2, 6]

lst.append(7)            # [3, 1, 4, 1, 5, 9, 2, 6, 7]
lst.insert(0, 0)         # [0, 3, 1, 4, 1, 5, 9, 2, 6, 7]
lst.extend([8, 9])       # adds 8, 9 to end
lst.pop()                # removes and returns 9 (last)
lst.pop(0)               # removes and returns 0 (index 0)
lst.remove(1)            # removes first occurrence of 1
lst.index(4)             # returns index of first 4
lst.count(1)             # count of 1s
lst.sort()               # in-place sort: [1, 2, 3, 4, 5, 6, 9]
lst.sort(reverse=True)   # descending
lst.sort(key=lambda x: -x)  # custom sort key
lst.reverse()            # in-place reverse
lst.copy()               # shallow copy — changes to nested objects are shared!
import copy
deep = copy.deepcopy(lst)  # fully independent copy

# Unpacking
a, *b, c = [1, 2, 3, 4, 5]  # a=1, b=[2,3,4], c=5
first, *rest = [1, 2, 3]     # first=1, rest=[2,3]
```

---

### List Slicing — Full Reference

```python
lst = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

lst[2:5]                 # [2, 3, 4]          (start inclusive, stop exclusive)
lst[:3]                  # [0, 1, 2]           (from beginning)
lst[7:]                  # [7, 8, 9]           (to end)
lst[::2]                 # [0, 2, 4, 6, 8]     (step 2)
lst[::-1]                # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]  (reverse)
lst[1::2]                # [1, 3, 5, 7, 9]     (odd indices)

# Slice assignment
lst[2:5] = [20, 30, 40]  # replace a slice
lst[::2] = [0]*5         # replace every 2nd element
```

---

## 4. Tuples

```python
# Immutable ordered sequence
t = (1, 2, 3)
t2 = 1, 2, 3             # parentheses optional
single = (42,)           # MUST have trailing comma for single-element tuple
single = 42,             # same thing

# Tuple methods (only 2!)
t.count(2)               # count occurrences — O(n)
t.index(2)               # find first index — O(n)

# Tuples are hashable (can be dict keys / set members)
d = {(1, 2): "point A", (3, 4): "point B"}
s = {(1,2), (3,4)}

# Tuple unpacking
x, y, z = (1, 2, 3)
(a, b), c = (1, 2), 3   # nested unpacking

# Named tuples (see Collections section for detailed namedtuple)
# Tuples are faster than lists for read operations
# Use tuples for fixed data, lists for mutable data

# Converting
list((1,2,3))            # [1, 2, 3]
tuple([1,2,3])           # (1, 2, 3)
```

---

## 5. Dictionaries — All Operations & Complexity

### Dict Basics

```python
# Creation
d = {}
d = dict()
d = {'a': 1, 'b': 2}
d = dict(a=1, b=2)
d = dict([('a', 1), ('b', 2)])   # from list of tuples
d = {k: v for k, v in [('a', 1), ('b', 2)]}  # dict comprehension

# From keys with default value
d = dict.fromkeys(['a', 'b', 'c'], 0)  # {'a': 0, 'b': 0, 'c': 0}
```

---

### All Dict Methods with Time Complexity

| Operation | Time Complexity |
| :--- | :--- |
| `d[key]` | **O(1)** average, O(n) worst (hash collision) |
| `d[key] = val` | **O(1)** average |
| `del d[key]` | **O(1)** average |
| `key in d` | **O(1)** average |
| `d.get(key)` | **O(1)** average |
| `d.keys()` | **O(1)** (view object) |
| `d.values()` | **O(1)** (view object) |
| `d.items()` | **O(1)** (view object) |
| `len(d)` | **O(1)** |
| `d.update(other)` | **O(len(other))** |
| Iteration | **O(n)** |

```python
d = {'a': 1, 'b': 2, 'c': 3}

# Access
d['a']                   # 1
d.get('z')               # None  (no KeyError!)
d.get('z', 0)            # 0     (default value)

# Modify
d['d'] = 4               # add new key
d['a'] = 10              # update existing
del d['b']               # delete key

# Check existence
'a' in d                 # True
'z' not in d             # True

# Iteration
for k in d:              # iterate keys
    print(k)
for v in d.values():     # iterate values
    print(v)
for k, v in d.items():   # iterate key-value pairs
    print(k, v)

# pop: remove and return value
d.pop('c')               # 3, removes 'c'
d.pop('z', -1)           # -1, no KeyError (default returned)

# popitem: remove and return arbitrary (last inserted in Python 3.7+) (key, val) pair
d.popitem()              # ('d', 4)

# setdefault: get value if key exists, else set and return default
d.setdefault('x', 0)     # 0 (sets 'x': 0 if not present)
d.setdefault('a', 99)    # 10 (returns existing value, does NOT overwrite)

# update: merge dicts
d.update({'e': 5, 'f': 6})
d.update([('g', 7)])     # from iterable of pairs
merged = {**d, 'h': 8}   # spread operator merge (Python 3.5+)
merged = d | {'h': 8}    # dict union operator (Python 3.9+)

# copy
d_copy = d.copy()        # shallow copy
d2 = {**d}               # also shallow copy via spread

# clear
d.clear()                # {}

# Sorting a dict
sorted_d = dict(sorted(d.items()))                  # sort by key
sorted_d = dict(sorted(d.items(), key=lambda x: x[1]))  # sort by value
sorted_d = dict(sorted(d.items(), key=lambda x: -x[1])) # sort by value desc

# Dict maintains insertion order (Python 3.7+)
```

---

## 6. Sets — All Operations & Complexity

```python
# Sets: unordered, no duplicates, elements must be hashable
s = {1, 2, 3}
s = set([1, 1, 2, 3])   # {1, 2, 3} — deduplication
empty = set()            # NOT {}  (that's an empty dict!)
```

### Set Operations with Time Complexity

| Operation | Method | Operator | Time |
| :--- | :--- | :--- | :--- |
| Add element | `s.add(x)` | | **O(1)** avg |
| Remove (KeyError) | `s.remove(x)` | | **O(1)** avg |
| Remove (no error) | `s.discard(x)` | | **O(1)** avg |
| Pop arbitrary | `s.pop()` | | **O(1)** |
| Membership | `x in s` | | **O(1)** avg |
| Union | `s.union(t)` | `s \| t` | **O(len(s)+len(t))** |
| Intersection | `s.intersection(t)` | `s & t` | **O(min(len(s),len(t)))** |
| Difference | `s.difference(t)` | `s - t` | **O(len(s))** |
| Symmetric diff | `s.symmetric_difference(t)` | `s ^ t` | **O(len(s)+len(t))** |
| Subset | `s.issubset(t)` | `s <= t` | **O(len(s))** |
| Superset | `s.issuperset(t)` | `s >= t` | **O(len(t))** |
| Disjoint | `s.isdisjoint(t)` | | **O(min(s,t))** |

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

a | b                    # {1, 2, 3, 4, 5, 6}  union
a & b                    # {3, 4}              intersection
a - b                    # {1, 2}              elements in a but not b
b - a                    # {5, 6}              elements in b but not a
a ^ b                    # {1, 2, 5, 6}        elements in either but not both

# In-place updates
a |= b                   # a = a | b
a &= b
a -= b
a ^= b

a.add(7)
a.remove(7)              # raises KeyError if not found
a.discard(99)            # safe, no error
a.pop()                  # remove arbitrary element

# Frozenset: immutable set (hashable → can be dict key or set member)
fs = frozenset([1, 2, 3])
```

---

## 7. Collections Module

### Counter

```python
from collections import Counter

# Count frequencies of any iterable
c = Counter("aabbcccdd")          # Counter({'c': 3, 'a': 2, 'b': 2, 'd': 2})
c = Counter([1, 1, 2, 2, 2, 3])  # Counter({2: 3, 1: 2, 3: 1})
c = Counter({'a': 3, 'b': 2})    # from dict

# Access counts
c['a']                   # 3
c['z']                   # 0  (no KeyError for missing keys!)

# Most common
c.most_common(2)         # [('c', 3), ('a', 2)]  — top 2
c.most_common()          # all elements sorted by count desc

# Arithmetic
c1 = Counter(a=3, b=2)
c2 = Counter(a=1, b=4, c=5)
c1 + c2                  # Counter({'b': 6, 'c': 5, 'a': 4})  (add counts)
c1 - c2                  # Counter({'a': 2})  (subtract, keep only positives)
c1 & c2                  # Counter({'a': 1, 'b': 2})  (min of each)
c1 | c2                  # Counter({'c': 5, 'b': 4, 'a': 3})  (max of each)

# Total count
sum(c.values())          # total of all counts
list(c.elements())       # expand: ['a','a','a','b','b',...]
c.update("aab")          # add more counts
c.subtract("aab")        # subtract counts

# Use case: Check if s2 is anagram of s1
Counter("listen") == Counter("silent")   # True

# Use case: Find first non-repeating character
s = "aabbc"
c = Counter(s)
next((ch for ch in s if c[ch] == 1), None)  # 'c'
```

---

### defaultdict

```python
from collections import defaultdict

# Like a dict, but auto-initializes missing keys with a default factory
d = defaultdict(int)       # default value: 0
d = defaultdict(list)      # default value: []
d = defaultdict(set)       # default value: set()
d = defaultdict(lambda: 'N/A')  # custom default

# Use case: group elements
from collections import defaultdict
graph = defaultdict(list)
edges = [(1,2), (1,3), (2,4)]
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)
# graph[1] = [2, 3], graph[2] = [1, 4], graph[3] = [1], graph[4] = [2]

# Use case: frequency count (alternative to Counter)
freq = defaultdict(int)
for char in "hello":
    freq[char] += 1
# {'h': 1, 'e': 1, 'l': 2, 'o': 1}

# Use case: group anagrams
words = ["eat","tea","tan","ate","nat","bat"]
groups = defaultdict(list)
for w in words:
    groups[tuple(sorted(w))].append(w)
# {('a','e','t'): ['eat','tea','ate'], ...}
```

---

### OrderedDict

```python
from collections import OrderedDict

# Remembers insertion order (all dicts do in Python 3.7+, but OrderedDict has extra features)
od = OrderedDict()
od['first'] = 1
od['second'] = 2
od['third'] = 3

od.move_to_end('first')          # move key to last position
od.move_to_end('third', last=False)  # move to first position
od.popitem(last=True)            # pop last inserted
od.popitem(last=False)           # pop first inserted

# LRU Cache implementation using OrderedDict
class LRUCache:
    def __init__(self, capacity):
        self.cap = capacity
        self.cache = OrderedDict()

    def get(self, key):
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)   # mark as recently used
        return self.cache[key]

    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.cap:
            self.cache.popitem(last=False)  # evict least recently used
```

---

### deque (Double-Ended Queue)

```python
from collections import deque

# O(1) append/pop from BOTH ends (unlike list which is O(n) at front)
d = deque()
d = deque([1, 2, 3])
d = deque([1, 2, 3], maxlen=5)  # auto-evict oldest when full

# Operations
d.append(4)              # add to RIGHT  — O(1)
d.appendleft(0)          # add to LEFT   — O(1)
d.pop()                  # remove from RIGHT — O(1)
d.popleft()              # remove from LEFT  — O(1)

d.extend([5, 6])         # extend RIGHT
d.extendleft([0, -1])    # extend LEFT (each element appended to left, so order reverses)

d.rotate(2)              # rotate right by 2 (last 2 go to front)
d.rotate(-2)             # rotate left by 2  (first 2 go to back)

len(d)                   # O(1)
d[0]; d[-1]              # O(1) — random access at ends
d[3]                     # O(n) — random access at middle (unlike list)

# Use case: BFS queue
from collections import deque
def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# Use case: Sliding window maximum (monotonic deque)
def max_sliding_window(nums, k):
    d = deque()   # stores indices, front is always max
    result = []
    for i, n in enumerate(nums):
        while d and nums[d[-1]] < n:
            d.pop()
        d.append(i)
        if d[0] == i - k:
            d.popleft()    # remove out-of-window index
        if i >= k - 1:
            result.append(nums[d[0]])
    return result
```

---

### namedtuple

```python
from collections import namedtuple

# Tuple with named fields — immutable, memory-efficient, readable
Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 4)
p.x                      # 3
p.y                      # 4
p[0]                     # 3 — still supports index access
p._asdict()              # {'x': 3, 'y': 4}
p._replace(x=10)         # Point(x=10, y=4) — returns new tuple

# Use case: return multiple values from function with names
TreeNode = namedtuple('TreeNode', ['val', 'left', 'right'])
```

---

### heapq (Min Heap)

```python
import heapq

# Python only has min-heap!
# For max-heap: negate all values

lst = [3, 1, 4, 1, 5, 9, 2, 6]
heapq.heapify(lst)       # convert list to min-heap IN PLACE — O(n)

heapq.heappush(lst, 0)   # push element — O(log n)
heapq.heappop(lst)       # pop SMALLEST — O(log n)
lst[0]                   # peek smallest WITHOUT removing — O(1)

# Push + pop in one operation (more efficient than separate calls)
heapq.heappushpop(lst, 0)    # push then pop smallest
heapq.heapreplace(lst, 0)    # pop smallest then push (raises if empty)

# N largest/smallest (uses heap internally)
heapq.nlargest(3, lst)       # [9, 6, 5]
heapq.nsmallest(3, lst)      # [0, 1, 1]

# Max heap trick: negate values
max_heap = []
heapq.heappush(max_heap, -5)
heapq.heappush(max_heap, -3)
heapq.heappush(max_heap, -8)
max_val = -heapq.heappop(max_heap)  # 8

# Heap with tuples: sorts by first element
tasks = []
heapq.heappush(tasks, (3, "low priority"))
heapq.heappush(tasks, (1, "high priority"))
heapq.heappush(tasks, (2, "medium priority"))
heapq.heappop(tasks)     # (1, 'high priority')

# Merge sorted iterables
list(heapq.merge([1,3,5], [2,4,6]))  # [1, 2, 3, 4, 5, 6]
```

---

## 8. Comprehensions & Generators

### List, Dict, Set Comprehensions

```python
# List comprehension: [expression for item in iterable if condition]
squares = [x**2 for x in range(10)]
evens   = [x for x in range(20) if x % 2 == 0]
flat    = [x for row in [[1,2],[3,4],[5,6]] for x in row]  # flatten nested list

# Nested comprehension (matrix transpose)
matrix = [[1,2,3],[4,5,6],[7,8,9]]
transposed = [[row[i] for row in matrix] for i in range(3)]

# Dict comprehension
d = {k: v for k, v in zip('abc', [1,2,3])}  # {'a':1, 'b':2, 'c':3}
inverted = {v: k for k, v in d.items()}       # invert a dict

# Set comprehension
unique_lens = {len(w) for w in ['hello', 'world', 'hi']}  # {2, 5}

# Conditional expression (ternary)
x = 5
result = "even" if x % 2 == 0 else "odd"
```

---

### Generators

```python
# Generator expression: like list comprehension but LAZY (evaluates on demand)
# Uses () instead of []
gen = (x**2 for x in range(10))   # Generator object, nothing computed yet
next(gen)                          # 0  (compute first value)
next(gen)                          # 1  (compute second value)
list(gen)                          # [4, 9, 16, ...] (consume rest)

# Generator function: uses yield
def fibonacci():
    a, b = 0, 1
    while True:
        yield a            # pause here, return a, resume on next()
        a, b = b, a + b

fib = fibonacci()
[next(fib) for _ in range(8)]     # [0, 1, 1, 2, 3, 5, 8, 13]

# Generator for reading large files without loading into memory
def read_large_file(filepath):
    with open(filepath) as f:
        for line in f:
            yield line.strip()

# yield from: delegate to another generator/iterable
def chain(*iterables):
    for it in iterables:
        yield from it

list(chain([1,2], [3,4], [5]))    # [1, 2, 3, 4, 5]

# Key advantage: O(1) memory regardless of data size
# sum of first million squares — only ONE value in memory at a time
total = sum(x**2 for x in range(1_000_000))
```

---

## 9. Functions — args, kwargs, lambdas, decorators

### *args and **kwargs

```python
# *args: accept any number of positional arguments as a tuple
def add(*args):
    return sum(args)

add(1, 2, 3)             # 6
add(1, 2, 3, 4, 5)       # 15

# **kwargs: accept any number of keyword arguments as a dict
def greet(**kwargs):
    for key, val in kwargs.items():
        print(f"{key}: {val}")

greet(name="Alice", age=25)   # name: Alice  age: 25

# Combined: positional, *args, keyword-only, **kwargs
def func(a, b, *args, key=None, **kwargs):
    print(a, b, args, key, kwargs)

func(1, 2, 3, 4, key='x', extra=99)
# 1 2 (3, 4) x {'extra': 99}

# Unpacking with * and **
def add(a, b, c):
    return a + b + c

nums = [1, 2, 3]
add(*nums)               # unpack list as positional args

params = {'a': 1, 'b': 2, 'c': 3}
add(**params)            # unpack dict as keyword args
```

---

### Lambda Functions

```python
# Anonymous one-line functions
square = lambda x: x**2
square(5)                # 25

add = lambda x, y: x + y
add(3, 4)               # 7

# Common uses:
# Sort by custom key
people = [{'name': 'Bob', 'age': 30}, {'name': 'Alice', 'age': 25}]
sorted(people, key=lambda p: p['age'])

# Sort by multiple criteria
data = [(1, 'b'), (2, 'a'), (1, 'a')]
sorted(data, key=lambda x: (x[0], x[1]))  # sort by first, then second

# With map/filter
list(map(lambda x: x*2, [1, 2, 3]))    # [2, 4, 6]
list(filter(lambda x: x > 2, [1,2,3,4]))  # [3, 4]
```

---

### Decorators

```python
import functools
import time

# A decorator is a function that takes a function and returns a modified function

# Basic decorator pattern
def my_decorator(func):
    @functools.wraps(func)    # preserves original function metadata
    def wrapper(*args, **kwargs):
        print("Before")
        result = func(*args, **kwargs)
        print("After")
        return result
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Before
# Hello!
# After

# Timing decorator
def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time()-start:.4f}s")
        return result
    return wrapper

# Decorator with arguments
def repeat(n):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def greet():
    print("Hello!")

greet()  # prints Hello! 3 times

# Memoization decorator (cache results of expensive function calls)
def memoize(func):
    cache = {}
    @functools.wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

# Python built-in: @functools.lru_cache (thread-safe, optimized)
from functools import lru_cache

@lru_cache(maxsize=None)   # None = unlimited cache
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)

fib(100)                  # Fast! Cached intermediate results
fib.cache_info()          # CacheInfo(hits=98, misses=101, maxsize=None, currsize=101)
fib.cache_clear()         # clear the cache
```

---

## 10. OOP — Classes, Inheritance, Dunder Methods

### Classes & Instances

```python
class Animal:
    # Class variable: shared by ALL instances
    kingdom = "Animalia"
    count = 0

    def __init__(self, name, sound):
        # Instance variables: unique to each instance
        self.name = name
        self.sound = sound
        Animal.count += 1

    # Instance method: has access to self (instance)
    def speak(self):
        return f"{self.name} says {self.sound}"

    # Class method: has access to cls (class), NOT instance
    @classmethod
    def get_count(cls):
        return f"Total animals: {cls.count}"

    # Static method: no access to instance or class
    @staticmethod
    def is_valid_name(name):
        return isinstance(name, str) and len(name) > 0

    def __repr__(self):   # developer-friendly string representation
        return f"Animal(name={self.name!r})"

    def __str__(self):    # user-friendly string representation
        return self.name

dog = Animal("Rex", "Woof")
dog.speak()              # 'Rex says Woof'
Animal.get_count()       # 'Total animals: 1'
Animal.is_valid_name("Rex")  # True
repr(dog)                # "Animal(name='Rex')"
str(dog)                 # "Rex"
```

---

### Inheritance & MRO

```python
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "Woof")   # call parent __init__
        self.breed = breed

    def speak(self):     # override parent method
        return f"{self.name} ({self.breed}) barks!"

    def fetch(self):
        return f"{self.name} fetches the ball!"

d = Dog("Rex", "Labrador")
d.speak()                # "Rex (Labrador) barks!"  — uses overridden method
d.kingdom                # "Animalia"  — inherits class variable
isinstance(d, Dog)       # True
isinstance(d, Animal)    # True  — is-a relationship
issubclass(Dog, Animal)  # True

# Multiple inheritance & MRO (Method Resolution Order)
class A:
    def hello(self): return "A"

class B(A):
    def hello(self): return "B"

class C(A):
    def hello(self): return "C"

class D(B, C):           # Diamond inheritance
    pass

D.__mro__                # (D, B, C, A, object) — MRO uses C3 linearization
D().hello()              # "B" — follows MRO left to right
```

---

### Dunder (Magic) Methods

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    # String representation
    def __repr__(self):  return f"Vector({self.x}, {self.y})"
    def __str__(self):   return f"({self.x}, {self.y})"

    # Arithmetic operators
    def __add__(self, other):  return Vector(self.x + other.x, self.y + other.y)
    def __sub__(self, other):  return Vector(self.x - other.x, self.y - other.y)
    def __mul__(self, scalar): return Vector(self.x * scalar, self.y * scalar)
    def __rmul__(self, scalar): return self.__mul__(scalar)  # 3 * v

    # Comparison operators
    def __eq__(self, other):   return self.x == other.x and self.y == other.y
    def __lt__(self, other):   return abs(self) < abs(other)
    def __le__(self, other):   return abs(self) <= abs(other)

    # Length / absolute value
    def __abs__(self):         return (self.x**2 + self.y**2) ** 0.5
    def __len__(self):         return 2   # number of components

    # Boolean
    def __bool__(self):        return self.x != 0 or self.y != 0

    # Hashing (required if __eq__ is defined, for use in sets/dicts)
    def __hash__(self):        return hash((self.x, self.y))

    # Container protocol
    def __getitem__(self, idx):
        return (self.x, self.y)[idx]

    def __iter__(self):        # makes Vector iterable
        yield self.x
        yield self.y

    def __contains__(self, val): return val in (self.x, self.y)

    # Context manager protocol
    def __enter__(self): return self
    def __exit__(self, *args): pass

    # Callable
    def __call__(self, scale): return Vector(self.x*scale, self.y*scale)

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v1 + v2                  # Vector(4, 6)
v1 * 3                   # Vector(3, 6)
abs(v1)                  # 2.236...
list(v1)                 # [1, 2]
1 in v1                  # True
v1 == Vector(1, 2)       # True
{v1, v2}                 # can add to set because __hash__ defined
v1(2)                    # Vector(2, 4) — callable instance
```

---

### Properties & Dataclasses

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius

    @property
    def celsius(self):           # getter
        return self._celsius

    @celsius.setter
    def celsius(self, value):    # setter
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value

    @property
    def fahrenheit(self):        # computed property (no setter)
        return self._celsius * 9/5 + 32

t = Temperature(100)
t.celsius                # 100
t.fahrenheit             # 212.0
t.celsius = 25           # calls setter
t.celsius = -300         # raises ValueError

# Dataclasses (Python 3.7+) — auto-generate __init__, __repr__, __eq__
from dataclasses import dataclass, field

@dataclass
class Point:
    x: float
    y: float
    z: float = 0.0          # default value
    tags: list = field(default_factory=list)  # mutable default MUST use field()

    def distance(self):
        return (self.x**2 + self.y**2 + self.z**2) ** 0.5

p = Point(1.0, 2.0)
repr(p)                  # "Point(x=1.0, y=2.0, z=0.0, tags=[])"
p == Point(1.0, 2.0)     # True (auto __eq__)

@dataclass(frozen=True)  # immutable + hashable
class FrozenPoint:
    x: float
    y: float
```

---

## 11. itertools & functools

### itertools

```python
import itertools

# --- Infinite iterators ---
itertools.count(10, 2)          # 10, 12, 14, 16, ... (infinite)
itertools.cycle([1, 2, 3])      # 1, 2, 3, 1, 2, 3, ... (infinite)
itertools.repeat(5, 3)          # 5, 5, 5  (3 times)

# --- Combinatoric iterators ---
# Permutations: ordered arrangements (order matters)
list(itertools.permutations('ABC', 2))
# [('A','B'),('A','C'),('B','A'),('B','C'),('C','A'),('C','B')]  — n!/(n-r)!

# Combinations: unordered selections (order doesn't matter)
list(itertools.combinations('ABC', 2))
# [('A','B'),('A','C'),('B','C')]  — n! / (r! * (n-r)!)

# Combinations with replacement: can repeat elements
list(itertools.combinations_with_replacement('AB', 2))
# [('A','A'),('A','B'),('B','B')]

# Cartesian product (like nested for loops)
list(itertools.product('AB', repeat=2))
# [('A','A'),('A','B'),('B','A'),('B','B')]
list(itertools.product([1,2], [3,4]))
# [(1,3),(1,4),(2,3),(2,4)]

# --- Sequence iterators ---
# chain: flatten multiple iterables
list(itertools.chain([1,2], [3,4], [5]))  # [1, 2, 3, 4, 5]
list(itertools.chain.from_iterable([[1,2],[3,4]]))  # [1, 2, 3, 4]

# islice: slice an iterator without materializing it
list(itertools.islice(range(100), 5, 20, 3))  # [5, 8, 11, 14, 17]

# groupby: group consecutive elements with same key
data = [('A', 1), ('A', 2), ('B', 3), ('B', 4), ('A', 5)]
for key, group in itertools.groupby(data, key=lambda x: x[0]):
    print(key, list(group))
# A [('A',1),('A',2)]  B [('B',3),('B',4)]  A [('A',5)]
# NOTE: only groups CONSECUTIVE equal keys — sort first if needed!

# accumulate: running aggregate
list(itertools.accumulate([1,2,3,4,5]))         # [1, 3, 6, 10, 15] running sum
list(itertools.accumulate([1,2,3,4,5], max))    # [1, 2, 3, 4, 5] running max
import operator
list(itertools.accumulate([1,2,3,4,5], operator.mul))  # [1,2,6,24,120] factorial

# takewhile / dropwhile
list(itertools.takewhile(lambda x: x < 5, [1,3,5,2,6]))  # [1, 3]  (stop at first fail)
list(itertools.dropwhile(lambda x: x < 5, [1,3,5,2,6]))  # [5, 2, 6]  (drop until first pass)

# zip_longest: zip with fill value for unequal lengths
list(itertools.zip_longest([1,2,3], [4,5], fillvalue=0))  # [(1,4),(2,5),(3,0)]

# starmap: map with tuple unpacking
list(itertools.starmap(pow, [(2,3),(3,2),(4,2)]))  # [8, 9, 16]
```

---

### functools

```python
from functools import reduce, partial, lru_cache, total_ordering, cached_property

# reduce: apply function cumulatively (left fold)
reduce(lambda a, b: a + b, [1, 2, 3, 4, 5])  # 15
reduce(lambda a, b: a * b, [1, 2, 3, 4, 5])  # 120  (factorial)

# partial: freeze some arguments of a function
def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
cube   = partial(power, exp=3)
square(5)                # 25
cube(3)                  # 27

# lru_cache: memoize function results (covered in decorators)

# total_ordering: define __eq__ and ONE comparison, get the rest for free
@total_ordering
class Student:
    def __init__(self, name, gpa):
        self.name = name
        self.gpa = gpa
    def __eq__(self, other): return self.gpa == other.gpa
    def __lt__(self, other): return self.gpa < other.gpa
    # auto-generates: __le__, __gt__, __ge__

# cached_property: compute once, cache forever (property + memoize)
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @cached_property
    def area(self):
        import math
        return math.pi * self.radius ** 2

c = Circle(5)
c.area                   # computed and cached
c.area                   # returned from cache
```

---

## 12. File I/O & Exception Handling

```python
# File reading (context manager automatically closes file)
with open('data.txt', 'r', encoding='utf-8') as f:
    content = f.read()           # entire file as string
    lines = f.readlines()        # list of lines (including \n)
    for line in f:               # iterate line by line (memory efficient)
        print(line.strip())

# File writing
with open('output.txt', 'w') as f:    # 'w' overwrites, 'a' appends
    f.write("Hello\n")
    f.writelines(["line1\n", "line2\n"])

# JSON
import json
data = {'name': 'Alice', 'scores': [95, 87, 92]}
with open('data.json', 'w') as f:
    json.dump(data, f, indent=2)
with open('data.json') as f:
    loaded = json.load(f)

json.dumps(data)                 # dict → JSON string
json.loads('{"a": 1}')           # JSON string → dict

# Exception Handling
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except (TypeError, ValueError) as e:   # multiple exception types
    print(f"Type or Value Error: {e}")
except Exception as e:                 # catch all
    print(f"Unexpected: {e}")
else:
    print("No exception occurred")     # runs if try block succeeded
finally:
    print("Always runs")               # cleanup code

# Common exceptions
# ValueError, TypeError, KeyError, IndexError, AttributeError
# FileNotFoundError, PermissionError, IOError
# ZeroDivisionError, OverflowError, RecursionError
# StopIteration, RuntimeError, NotImplementedError
# ImportError, ModuleNotFoundError

# Custom exception
class ValidationError(Exception):
    def __init__(self, field, message):
        self.field = field
        super().__init__(f"Validation failed for '{field}': {message}")

raise ValidationError("email", "invalid format")
```

---

## 13. Type Hints & Typing Module

```python
from typing import List, Dict, Tuple, Set, Optional, Union, Any
from typing import Callable, Iterator, Generator, TypeVar

# Basic type hints (Python 3.5+)
def greet(name: str) -> str:
    return f"Hello, {name}"

def process(items: list[int], multiplier: int = 1) -> list[int]:
    return [x * multiplier for x in items]

# Optional: value OR None
def find_user(id: int) -> Optional[str]:  # same as Union[str, None]
    return None

# Union: one of several types
def parse(val: Union[int, str]) -> str:
    return str(val)

# Callable
def apply(func: Callable[[int], int], x: int) -> int:
    return func(x)

# TypeVar for generics
T = TypeVar('T')
def first(lst: list[T]) -> T:
    return lst[0]

# Python 3.10+ syntax (simpler)
def greet(name: str | None = None) -> str:
    return f"Hello, {name or 'stranger'}"

# Variable annotations
count: int = 0
items: list[str] = []
mapping: dict[str, int] = {}
```

---

# PART 2 — DSA in Python

---

## 14. Big-O Complexity Cheat Sheet

### Time Complexity Comparison

| Complexity | Name | Example |
| :--- | :--- | :--- |
| O(1) | Constant | Hash map lookup, array index access |
| O(log n) | Logarithmic | Binary search, heap operations |
| O(n) | Linear | Linear search, single loop |
| O(n log n) | Log-linear | Merge sort, heap sort, Timsort |
| O(n²) | Quadratic | Bubble sort, nested loops |
| O(n³) | Cubic | Floyd-Warshall, 3-sum naive |
| O(2ⁿ) | Exponential | Subsets, recursive Fibonacci |
| O(n!) | Factorial | Generating all permutations |

### Space Complexity
- **O(1):** In-place algorithms, two-pointer on sorted array
- **O(log n):** Recursive binary search (call stack)
- **O(n):** Array copy, hash map, recursion stack depth
- **O(n²):** 2D DP table, adjacency matrix

---

## 15. Arrays & Lists — Patterns

### Two Pointers Pattern

```python
# Use when: sorted array, finding pairs/triplets with target sum

# 1. Two sum in sorted array
def two_sum_sorted(nums, target):
    left, right = 0, len(nums) - 1
    while left < right:
        s = nums[left] + nums[right]
        if s == target:
            return [left, right]
        elif s < target:
            left += 1
        else:
            right -= 1
    return []

# 2. Three Sum (find all unique triplets summing to 0)
def three_sum(nums):
    nums.sort()
    result = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i-1]:  # skip duplicates
            continue
        left, right = i + 1, len(nums) - 1
        while left < right:
            s = nums[i] + nums[left] + nums[right]
            if s == 0:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left+1]: left += 1
                while left < right and nums[right] == nums[right-1]: right -= 1
                left += 1; right -= 1
            elif s < 0: left += 1
            else: right -= 1
    return result

# 3. Container with most water
def max_water(height):
    left, right = 0, len(height) - 1
    max_area = 0
    while left < right:
        area = min(height[left], height[right]) * (right - left)
        max_area = max(max_area, area)
        if height[left] < height[right]: left += 1
        else: right -= 1
    return max_area
```

---

### Sliding Window Pattern

```python
# Use when: subarray/substring of fixed or variable size

# 1. Maximum sum subarray of size k (fixed window)
def max_sum_subarray(nums, k):
    window_sum = sum(nums[:k])
    max_sum = window_sum
    for i in range(k, len(nums)):
        window_sum += nums[i] - nums[i - k]  # slide: add right, remove left
        max_sum = max(max_sum, window_sum)
    return max_sum

# 2. Longest substring without repeating characters (variable window)
def length_of_longest_substring(s):
    char_index = {}
    left = 0
    max_len = 0
    for right, char in enumerate(s):
        if char in char_index and char_index[char] >= left:
            left = char_index[char] + 1   # shrink window from left
        char_index[char] = right
        max_len = max(max_len, right - left + 1)
    return max_len

# 3. Minimum window substring (contains all chars of pattern)
from collections import Counter
def min_window(s, t):
    need = Counter(t)
    missing = len(t)
    best_left, best_right = 0, float('inf')
    left = 0
    for right, char in enumerate(s, 1):  # 1-indexed right
        if need[char] > 0:
            missing -= 1
        need[char] -= 1
        if missing == 0:   # valid window found
            while need[s[left]] < 0:   # shrink from left
                need[s[left]] += 1
                left += 1
            if right - left < best_right - best_left:
                best_left, best_right = left, right
            need[s[left]] += 1   # prepare to move left
            missing += 1
            left += 1
    return s[best_left:best_right] if best_right < float('inf') else ""
```

---

### Prefix Sum Pattern

```python
# Precompute cumulative sums for O(1) range sum queries
def build_prefix(nums):
    prefix = [0] * (len(nums) + 1)
    for i, n in enumerate(nums):
        prefix[i+1] = prefix[i] + n
    return prefix

def range_sum(prefix, l, r):   # sum of nums[l..r] inclusive
    return prefix[r+1] - prefix[l]

# Example
nums = [1, 2, 3, 4, 5]
prefix = build_prefix(nums)    # [0, 1, 3, 6, 10, 15]
range_sum(prefix, 1, 3)        # 9 (2+3+4)

# Subarray sum equals k (count subarrays)
def subarray_sum(nums, k):
    count = 0
    current_sum = 0
    prefix_counts = {0: 1}    # prefix_sum: frequency
    for n in nums:
        current_sum += n
        # if (current_sum - k) appeared as prefix, we found a valid subarray
        count += prefix_counts.get(current_sum - k, 0)
        prefix_counts[current_sum] = prefix_counts.get(current_sum, 0) + 1
    return count
```

---

### Kadane's Algorithm (Maximum Subarray Sum)

```python
def max_subarray(nums):
    max_sum = current_sum = nums[0]
    for n in nums[1:]:
        current_sum = max(n, current_sum + n)   # extend or restart
        max_sum = max(max_sum, current_sum)
    return max_sum

# Time: O(n), Space: O(1)

# Track actual subarray
def max_subarray_with_indices(nums):
    max_sum = current_sum = nums[0]
    start = end = temp_start = 0
    for i in range(1, len(nums)):
        if nums[i] > current_sum + nums[i]:
            current_sum = nums[i]
            temp_start = i
        else:
            current_sum += nums[i]
        if current_sum > max_sum:
            max_sum = current_sum
            start = temp_start
            end = i
    return nums[start:end+1], max_sum
```

---

## 16. Strings — Interview Patterns

```python
# KMP (Knuth-Morris-Pratt) Pattern Matching — O(n+m)
def kmp_search(text, pattern):
    def build_lps(pattern):   # Longest Proper Prefix which is also Suffix
        lps = [0] * len(pattern)
        length = 0
        i = 1
        while i < len(pattern):
            if pattern[i] == pattern[length]:
                length += 1
                lps[i] = length
                i += 1
            elif length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
        return lps

    lps = build_lps(pattern)
    i = j = 0
    matches = []
    while i < len(text):
        if text[i] == pattern[j]:
            i += 1; j += 1
        if j == len(pattern):
            matches.append(i - j)
            j = lps[j - 1]
        elif i < len(text) and text[i] != pattern[j]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    return matches

# Valid Parentheses
def is_valid_parens(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    for ch in s:
        if ch in mapping:
            top = stack.pop() if stack else '#'
            if mapping[ch] != top:
                return False
        else:
            stack.append(ch)
    return not stack

# Longest Palindromic Substring (Expand Around Center)
def longest_palindrome(s):
    def expand(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1; right += 1
        return s[left+1:right]  # back up one step

    result = ""
    for i in range(len(s)):
        odd  = expand(i, i)      # odd length palindrome
        even = expand(i, i + 1)  # even length palindrome
        result = max(result, odd, even, key=len)
    return result
```

---

## 17. Linked Lists

### Node & Linked List Implementation

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

    def __repr__(self):
        return f"ListNode({self.val})"

class LinkedList:
    def __init__(self):
        self.head = None
        self.size = 0

    def __len__(self): return self.size

    # Convert list to linked list (helper)
    @classmethod
    def from_list(cls, lst):
        ll = cls()
        for val in reversed(lst):
            ll.prepend(val)
        return ll

    # Convert to list (helper)
    def to_list(self):
        result = []
        curr = self.head
        while curr:
            result.append(curr.val)
            curr = curr.next
        return result
```

---

### All Linked List Operations

```python
    # --- INSERTION ---

    # Prepend to front — O(1)
    def prepend(self, val):
        node = ListNode(val, self.head)
        self.head = node
        self.size += 1

    # Append to end — O(n)
    def append(self, val):
        node = ListNode(val)
        if not self.head:
            self.head = node
        else:
            curr = self.head
            while curr.next:
                curr = curr.next
            curr.next = node
        self.size += 1

    # Insert at index — O(n)
    def insert_at(self, index, val):
        if index == 0:
            return self.prepend(val)
        curr = self.head
        for _ in range(index - 1):
            if not curr: raise IndexError("Index out of range")
            curr = curr.next
        node = ListNode(val, curr.next)
        curr.next = node
        self.size += 1

    # --- DELETION ---

    # Delete front — O(1)
    def delete_front(self):
        if not self.head: raise IndexError("Empty list")
        val = self.head.val
        self.head = self.head.next
        self.size -= 1
        return val

    # Delete by value (first occurrence) — O(n)
    def delete_val(self, val):
        if not self.head: return False
        if self.head.val == val:
            self.head = self.head.next
            self.size -= 1
            return True
        curr = self.head
        while curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next
                self.size -= 1
                return True
            curr = curr.next
        return False

    # --- SEARCH ---

    # Find value — O(n)
    def find(self, val):
        curr = self.head
        index = 0
        while curr:
            if curr.val == val:
                return index
            curr = curr.next
            index += 1
        return -1
```

---

### Classic Linked List Algorithms

```python
# 1. Reverse a linked list — O(n) time, O(1) space
def reverse_list(head):
    prev, curr = None, head
    while curr:
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node
    return prev  # new head

# 2. Detect cycle (Floyd's Tortoise & Hare) — O(n) time, O(1) space
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False

# 3. Find cycle start node
def detect_cycle_start(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            # Reset one pointer to head; both move at same speed now
            slow = head
            while slow != fast:
                slow = slow.next
                fast = fast.next
            return slow   # cycle start
    return None

# 4. Middle of linked list (slow/fast pointers)
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow   # slow is at middle

# 5. Merge two sorted linked lists
def merge_sorted(l1, l2):
    dummy = ListNode(0)
    curr = dummy
    while l1 and l2:
        if l1.val <= l2.val:
            curr.next = l1; l1 = l1.next
        else:
            curr.next = l2; l2 = l2.next
        curr = curr.next
    curr.next = l1 or l2
    return dummy.next

# 6. Remove Nth node from end (two-pointer gap)
def remove_nth_from_end(head, n):
    dummy = ListNode(0, head)
    fast = slow = dummy
    for _ in range(n + 1):    # move fast n+1 ahead
        fast = fast.next
    while fast:                # move both until fast hits end
        slow = slow.next
        fast = fast.next
    slow.next = slow.next.next  # remove the node
    return dummy.next

# 7. Palindrome linked list — O(n) time, O(1) space
def is_palindrome(head):
    # Step 1: find middle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next; fast = fast.next.next
    # Step 2: reverse second half
    prev, curr = None, slow
    while curr:
        next_n = curr.next; curr.next = prev; prev = curr; curr = next_n
    # Step 3: compare
    left, right = head, prev
    while right:
        if left.val != right.val: return False
        left = left.next; right = right.next
    return True
```

---

### Doubly Linked List

```python
class DLLNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next

class DoublyLinkedList:
    def __init__(self):
        # Use sentinel nodes (dummy head & tail) to simplify edge cases
        self.head = DLLNode(float('-inf'))
        self.tail = DLLNode(float('inf'))
        self.head.next = self.tail
        self.tail.prev = self.head
        self.size = 0

    def _insert_after(self, node, val):   # O(1)
        new_node = DLLNode(val, node, node.next)
        node.next.prev = new_node
        node.next = new_node
        self.size += 1
        return new_node

    def _remove(self, node):               # O(1)
        node.prev.next = node.next
        node.next.prev = node.prev
        self.size -= 1
        return node.val

    def append(self, val):
        return self._insert_after(self.tail.prev, val)   # O(1)

    def prepend(self, val):
        return self._insert_after(self.head, val)         # O(1)

    def pop_front(self):
        return self._remove(self.head.next)               # O(1)

    def pop_back(self):
        return self._remove(self.tail.prev)               # O(1)
```

---

## 18. Stacks

### Stack Implementation & Operations

```python
# Python list as stack (append = push, pop = pop from end) — all O(1) amortized
stack = []

stack.append(1)          # push  — O(1)
stack.append(2)
stack.append(3)
stack[-1]                # peek  — O(1) (don't remove)
stack.pop()              # pop   — O(1)
len(stack)               # size  — O(1)
not stack                # is empty check

# Stack class
class Stack:
    def __init__(self):
        self._data = []
    def push(self, val): self._data.append(val)
    def pop(self):
        if self.is_empty(): raise IndexError("Stack is empty")
        return self._data.pop()
    def peek(self):
        if self.is_empty(): raise IndexError("Stack is empty")
        return self._data[-1]
    def is_empty(self): return len(self._data) == 0
    def size(self): return len(self._data)
```

---

### Stack Patterns

```python
# 1. Valid Parentheses (covered in strings section)

# 2. Min Stack — O(1) getMin at all times
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []    # tracks minimums

    def push(self, val):
        self.stack.append(val)
        min_val = min(val, self.min_stack[-1]) if self.min_stack else val
        self.min_stack.append(min_val)

    def pop(self):
        self.stack.pop()
        self.min_stack.pop()

    def top(self): return self.stack[-1]
    def get_min(self): return self.min_stack[-1]

# 3. Monotonic Stack (Next Greater Element)
# Maintain a stack where elements are always in decreasing order
def next_greater_element(nums):
    result = [-1] * len(nums)
    stack = []   # indices of elements waiting for their next greater
    for i, n in enumerate(nums):
        while stack and nums[stack[-1]] < n:
            idx = stack.pop()
            result[idx] = n   # n is the next greater for nums[idx]
        stack.append(i)
    return result

# Example: [2, 1, 2, 4, 3] → [4, 2, 4, -1, -1]

# 4. Largest Rectangle in Histogram — O(n)
def largest_rectangle(heights):
    stack = []   # monotonic increasing stack of indices
    max_area = 0
    heights.append(0)   # sentinel to flush remaining stack
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    heights.pop()
    return max_area

# 5. Evaluate Reverse Polish Notation
def eval_rpn(tokens):
    stack = []
    ops = {'+': lambda a,b: a+b, '-': lambda a,b: a-b,
           '*': lambda a,b: a*b, '/': lambda a,b: int(a/b)}
    for t in tokens:
        if t in ops:
            b, a = stack.pop(), stack.pop()
            stack.append(ops[t](a, b))
        else:
            stack.append(int(t))
    return stack[0]
```

---

## 19. Queues & Deque

```python
from collections import deque
import queue

# Queue (FIFO) using deque — O(1) for both enqueue and dequeue
q = deque()
q.append(1)              # enqueue (add to right)
q.append(2)
q.popleft()              # dequeue (remove from left) — O(1)
q[0]                     # peek front — O(1)
len(q)                   # size

# Thread-safe queue (for multithreading)
q = queue.Queue()
q.put(1)
q.get()
q.empty()

# Priority Queue (min-heap based)
import heapq
pq = []
heapq.heappush(pq, (1, "high"))
heapq.heappush(pq, (3, "low"))
heapq.heappush(pq, (2, "medium"))
heapq.heappop(pq)        # (1, 'high')

# queue.PriorityQueue (thread-safe)
pq = queue.PriorityQueue()
pq.put((1, "high"))
pq.get()

# Circular Queue (Ring Buffer)
class CircularQueue:
    def __init__(self, k):
        self.data = [None] * k
        self.head = self.tail = -1
        self.size = 0
        self.capacity = k

    def enqueue(self, val):
        if self.size == self.capacity: return False
        self.tail = (self.tail + 1) % self.capacity
        self.data[self.tail] = val
        if self.head == -1: self.head = 0
        self.size += 1
        return True

    def dequeue(self):
        if self.size == 0: return False
        self.head = (self.head + 1) % self.capacity
        self.size -= 1
        return True
```

---

## 20. Hash Maps & Hash Sets

```python
# Python dict is a hash map — O(1) average for get/set/delete

# Pattern: Frequency Map
from collections import Counter, defaultdict

# Two Sum (classic)
def two_sum(nums, target):
    seen = {}   # value → index
    for i, n in enumerate(nums):
        complement = target - n
        if complement in seen:
            return [seen[complement], i]
        seen[n] = i
    return []

# Group Anagrams
def group_anagrams(words):
    groups = defaultdict(list)
    for word in words:
        key = tuple(sorted(word))   # canonical form
        groups[key].append(word)
    return list(groups.values())

# Longest Consecutive Sequence — O(n)
def longest_consecutive(nums):
    num_set = set(nums)
    max_len = 0
    for n in num_set:
        if n - 1 not in num_set:   # start of a sequence
            length = 1
            while n + length in num_set:
                length += 1
            max_len = max(max_len, length)
    return max_len

# Implementing HashMap from scratch (for interview)
class HashMap:
    def __init__(self, size=1000):
        self.size = size
        self.buckets = [[] for _ in range(size)]   # chaining

    def _hash(self, key):
        return hash(key) % self.size

    def put(self, key, val):
        idx = self._hash(key)
        for pair in self.buckets[idx]:
            if pair[0] == key:
                pair[1] = val
                return
        self.buckets[idx].append([key, val])

    def get(self, key):
        idx = self._hash(key)
        for k, v in self.buckets[idx]:
            if k == key: return v
        return -1

    def remove(self, key):
        idx = self._hash(key)
        self.buckets[idx] = [(k,v) for k,v in self.buckets[idx] if k != key]
```

---

## 21. Binary Trees & Traversals

### TreeNode & Build

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

    def __repr__(self): return f"TreeNode({self.val})"

# Build tree from list (level-order, None = missing node)
from collections import deque
def build_tree(vals):
    if not vals or vals[0] is None: return None
    root = TreeNode(vals[0])
    queue = deque([root])
    i = 1
    while queue and i < len(vals):
        node = queue.popleft()
        if i < len(vals) and vals[i] is not None:
            node.left = TreeNode(vals[i])
            queue.append(node.left)
        i += 1
        if i < len(vals) and vals[i] is not None:
            node.right = TreeNode(vals[i])
            queue.append(node.right)
        i += 1
    return root
```

---

### All Tree Traversals

```python
# 1. Inorder: LEFT → ROOT → RIGHT (gives sorted order for BST)
def inorder(root):
    result = []
    def dfs(node):
        if not node: return
        dfs(node.left)
        result.append(node.val)
        dfs(node.right)
    dfs(root)
    return result

# Iterative inorder
def inorder_iter(root):
    result, stack = [], []
    curr = root
    while curr or stack:
        while curr:                  # go as left as possible
            stack.append(curr)
            curr = curr.left
        curr = stack.pop()           # process node
        result.append(curr.val)
        curr = curr.right            # go right
    return result

# 2. Preorder: ROOT → LEFT → RIGHT (used for tree serialization)
def preorder(root):
    if not root: return []
    return [root.val] + preorder(root.left) + preorder(root.right)

# Iterative preorder
def preorder_iter(root):
    if not root: return []
    result, stack = [], [root]
    while stack:
        node = stack.pop()
        result.append(node.val)
        if node.right: stack.append(node.right)   # right first (stack = LIFO)
        if node.left:  stack.append(node.left)
    return result

# 3. Postorder: LEFT → RIGHT → ROOT (used for deletion, size calculation)
def postorder(root):
    if not root: return []
    return postorder(root.left) + postorder(root.right) + [root.val]

# Iterative postorder (reverse of modified preorder)
def postorder_iter(root):
    if not root: return []
    result, stack = [], [root]
    while stack:
        node = stack.pop()
        result.append(node.val)
        if node.left:  stack.append(node.left)
        if node.right: stack.append(node.right)
    return result[::-1]   # reverse

# 4. Level-order BFS (breadth-first)
def level_order(root):
    if not root: return []
    result = []
    queue = deque([root])
    while queue:
        level = []
        for _ in range(len(queue)):   # process exactly one level
            node = queue.popleft()
            level.append(node.val)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result
# [[3], [9, 20], [15, 7]]

# 5. Zigzag level order
def zigzag_level_order(root):
    if not root: return []
    result = []
    queue = deque([root])
    left_to_right = True
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level if left_to_right else level[::-1])
        left_to_right = not left_to_right
    return result
```

---

### Tree Properties

```python
# Height of tree — O(n)
def height(root):
    if not root: return 0
    return 1 + max(height(root.left), height(root.right))

# Diameter (longest path between any two nodes)
def diameter(root):
    max_d = [0]
    def depth(node):
        if not node: return 0
        left = depth(node.left)
        right = depth(node.right)
        max_d[0] = max(max_d[0], left + right)
        return 1 + max(left, right)
    depth(root)
    return max_d[0]

# Balanced tree check
def is_balanced(root):
    def check(node):
        if not node: return 0
        left = check(node.left)
        right = check(node.right)
        if left == -1 or right == -1 or abs(left - right) > 1:
            return -1   # not balanced
        return 1 + max(left, right)
    return check(root) != -1

# Lowest Common Ancestor (LCA)
def lca(root, p, q):
    if not root or root == p or root == q:
        return root
    left  = lca(root.left, p, q)
    right = lca(root.right, p, q)
    if left and right: return root   # p and q are in different subtrees
    return left or right             # both in same subtree

# Max path sum (any path)
def max_path_sum(root):
    max_sum = [float('-inf')]
    def gain(node):
        if not node: return 0
        left_gain  = max(gain(node.left), 0)    # ignore negative paths
        right_gain = max(gain(node.right), 0)
        max_sum[0] = max(max_sum[0], node.val + left_gain + right_gain)
        return node.val + max(left_gain, right_gain)
    gain(root)
    return max_sum[0]

# Serialize / Deserialize binary tree
def serialize(root):
    if not root: return "null"
    return f"{root.val},{serialize(root.left)},{serialize(root.right)}"

def deserialize(data):
    vals = iter(data.split(','))
    def build():
        val = next(vals)
        if val == "null": return None
        node = TreeNode(int(val))
        node.left  = build()
        node.right = build()
        return node
    return build()
```

---

## 22. Binary Search Trees (BST)

```python
# BST property: left subtree < root < right subtree (for every node)

class BST:
    def __init__(self):
        self.root = None

    # Insert — O(h) average O(log n), worst O(n) if unbalanced
    def insert(self, val):
        self.root = self._insert(self.root, val)

    def _insert(self, node, val):
        if not node: return TreeNode(val)
        if val < node.val: node.left  = self._insert(node.left, val)
        elif val > node.val: node.right = self._insert(node.right, val)
        return node

    # Search — O(h)
    def search(self, val):
        curr = self.root
        while curr:
            if val == curr.val: return True
            curr = curr.left if val < curr.val else curr.right
        return False

    # Delete — O(h)
    def delete(self, val):
        self.root = self._delete(self.root, val)

    def _delete(self, node, val):
        if not node: return None
        if val < node.val:
            node.left  = self._delete(node.left, val)
        elif val > node.val:
            node.right = self._delete(node.right, val)
        else:
            if not node.left:  return node.right   # no left child
            if not node.right: return node.left    # no right child
            # Node has two children: replace with inorder successor (smallest in right subtree)
            successor = node.right
            while successor.left:
                successor = successor.left
            node.val = successor.val
            node.right = self._delete(node.right, successor.val)
        return node

    # Validate BST
    def is_valid_bst(self):
        def validate(node, min_val, max_val):
            if not node: return True
            if not (min_val < node.val < max_val): return False
            return (validate(node.left, min_val, node.val) and
                    validate(node.right, node.val, max_val))
        return validate(self.root, float('-inf'), float('inf'))

    # Kth smallest element (inorder is sorted for BST)
    def kth_smallest(self, k):
        count = [0]
        result = [None]
        def inorder(node):
            if not node or result[0]: return
            inorder(node.left)
            count[0] += 1
            if count[0] == k: result[0] = node.val; return
            inorder(node.right)
        inorder(self.root)
        return result[0]
```

---

## 23. Heaps & Priority Queues

```python
import heapq

# ==========================================
# MIN HEAP (default in Python)
# ==========================================
min_heap = []
heapq.heappush(min_heap, 5)
heapq.heappush(min_heap, 3)
heapq.heappush(min_heap, 8)
heapq.heappush(min_heap, 1)

min_heap[0]              # 1 — peek min O(1)
heapq.heappop(min_heap)  # 1 — pop min O(log n)

# ==========================================
# MAX HEAP (negate values)
# ==========================================
max_heap = []
for n in [5, 3, 8, 1]:
    heapq.heappush(max_heap, -n)

-max_heap[0]             # 8 — peek max
-heapq.heappop(max_heap) # 8 — pop max

# ==========================================
# HEAP WITH CUSTOM OBJECTS
# ==========================================
# Use tuple: (priority, tie_breaker, data)
import itertools
counter = itertools.count()

tasks = []
heapq.heappush(tasks, (1, next(counter), "high priority task"))
heapq.heappush(tasks, (3, next(counter), "low priority task"))
heapq.heappush(tasks, (1, next(counter), "another high priority"))

# ==========================================
# KEY ALGORITHMS
# ==========================================

# K Largest Elements
def k_largest(nums, k):
    return heapq.nlargest(k, nums)   # O(n log k)

# K Smallest Elements
def k_smallest(nums, k):
    return heapq.nsmallest(k, nums)  # O(n log k)

# Kth Largest Element using min-heap of size k
def kth_largest(nums, k):
    min_heap = []
    for n in nums:
        heapq.heappush(min_heap, n)
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    return min_heap[0]   # top of min-heap = kth largest

# Merge K Sorted Lists
def merge_k_sorted(lists):
    heap = []
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst[0], i, 0))   # (val, list_idx, elem_idx)
    result = []
    while heap:
        val, list_idx, elem_idx = heapq.heappop(heap)
        result.append(val)
        if elem_idx + 1 < len(lists[list_idx]):
            next_val = lists[list_idx][elem_idx + 1]
            heapq.heappush(heap, (next_val, list_idx, elem_idx + 1))
    return result

# Top K Frequent Elements
from collections import Counter
def top_k_frequent(nums, k):
    freq = Counter(nums)
    return heapq.nlargest(k, freq.keys(), key=freq.get)

# Find Median from Data Stream (two heaps)
class MedianFinder:
    def __init__(self):
        self.low  = []   # max-heap (negated) for lower half
        self.high = []   # min-heap for upper half

    def add_num(self, num):
        heapq.heappush(self.low, -num)           # push to max-heap
        heapq.heappush(self.high, -heapq.heappop(self.low))  # balance
        if len(self.low) < len(self.high):
            heapq.heappush(self.low, -heapq.heappop(self.high))

    def find_median(self):
        if len(self.low) > len(self.high):
            return -self.low[0]
        return (-self.low[0] + self.high[0]) / 2
```

---

## 24. Graphs — All Algorithms

### Graph Representation

```python
from collections import defaultdict, deque

# Adjacency List (most common — O(V+E) space)
graph = defaultdict(list)
edges = [(0,1), (0,2), (1,3), (2,3), (3,4)]
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)   # for undirected

# Weighted adjacency list
weighted_graph = defaultdict(list)
weighted_edges = [(0,1,4), (0,2,1), (1,3,1), (2,3,5), (3,4,3)]
for u, v, w in weighted_edges:
    weighted_graph[u].append((v, w))
    weighted_graph[v].append((u, w))

# Adjacency Matrix (O(V²) space — good for dense graphs or when edge exists check is critical)
V = 5
matrix = [[0]*V for _ in range(V)]
for u, v in edges:
    matrix[u][v] = 1
    matrix[v][u] = 1
```

---

### BFS (Breadth-First Search)

```python
# O(V + E) time, O(V) space
def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return order

# BFS Shortest Path (unweighted)
def shortest_path_bfs(graph, start, end):
    if start == end: return [start]
    visited = {start: None}   # node → parent
    queue = deque([start])
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited[neighbor] = node
                if neighbor == end:
                    # Reconstruct path
                    path = []
                    curr = end
                    while curr is not None:
                        path.append(curr)
                        curr = visited[curr]
                    return path[::-1]
                queue.append(neighbor)
    return []   # no path

# BFS on a grid (4-directional)
def bfs_grid(grid):
    rows, cols = len(grid), len(grid[0])
    visited = set()
    queue = deque([(0, 0, 0)])   # (row, col, distance)
    directions = [(0,1),(0,-1),(1,0),(-1,0)]
    while queue:
        r, c, dist = queue.popleft()
        if (r, c) in visited: continue
        visited.add((r, c))
        # Process (r, c)...
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and (nr, nc) not in visited:
                queue.append((nr, nc, dist + 1))
```

---

### DFS (Depth-First Search)

```python
# Recursive DFS — O(V + E) time, O(V) space (call stack)
def dfs_recursive(graph, node, visited=None):
    if visited is None: visited = set()
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
    return visited

# Iterative DFS (using explicit stack)
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    order = []
    while stack:
        node = stack.pop()
        if node in visited: continue
        visited.add(node)
        order.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                stack.append(neighbor)
    return order

# Count connected components
def count_components(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v); graph[v].append(u)
    visited = set()
    count = 0
    for node in range(n):
        if node not in visited:
            dfs_recursive(graph, node, visited)
            count += 1
    return count

# Number of Islands (DFS on grid)
def num_islands(grid):
    if not grid: return 0
    rows, cols = len(grid), len(grid[0])
    count = 0
    def dfs(r, c):
        if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] != '1':
            return
        grid[r][c] = '#'   # mark visited
        dfs(r+1,c); dfs(r-1,c); dfs(r,c+1); dfs(r,c-1)
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                dfs(r, c)
                count += 1
    return count
```

---

### Topological Sort

```python
# For Directed Acyclic Graphs (DAG) — schedule tasks with dependencies

# Method 1: DFS-based (reverse postorder)
def topo_sort_dfs(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
    visited = set()
    stack = []
    def dfs(node):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor)
        stack.append(node)   # add AFTER all descendants are added
    for node in range(n):
        if node not in visited:
            dfs(node)
    return stack[::-1]   # reverse gives topological order

# Method 2: Kahn's Algorithm (BFS-based, using in-degrees)
from collections import deque
def topo_sort_kahn(n, edges):
    graph = defaultdict(list)
    in_degree = [0] * n
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1
    queue = deque([i for i in range(n) if in_degree[i] == 0])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    if len(order) != n:
        return []   # cycle detected!
    return order

# Course Schedule (detect cycle)
def can_finish(n, prerequisites):
    return len(topo_sort_kahn(n, prerequisites)) == n
```

---

### Dijkstra's Shortest Path

```python
import heapq
from collections import defaultdict

# O((V + E) log V) — shortest path from source to all nodes (non-negative weights)
def dijkstra(graph, start):
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    pq = [(0, start)]   # (distance, node)
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]: continue   # stale entry
        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    return dist

# With path reconstruction
def dijkstra_path(graph, start, end):
    dist = defaultdict(lambda: float('inf'))
    dist[start] = 0
    prev = {}
    pq = [(0, start)]
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]: continue
        if node == end: break
        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                prev[neighbor] = node
                heapq.heappush(pq, (new_dist, neighbor))
    # Reconstruct path
    path = []
    curr = end
    while curr in prev:
        path.append(curr)
        curr = prev[curr]
    path.append(start)
    return path[::-1], dist[end]
```

---

### Union-Find (Disjoint Set Union — DSU)

```python
# O(α(n)) ≈ O(1) per operation with path compression + union by rank
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))  # each node is its own root
        self.rank   = [0] * n        # tree height (rank)
        self.count  = n              # number of connected components

    def find(self, x):               # with path compression
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # flatten tree
        return self.parent[x]

    def union(self, x, y):           # with union by rank
        rx, ry = self.find(x), self.find(y)
        if rx == ry: return False    # already connected
        if self.rank[rx] < self.rank[ry]:
            rx, ry = ry, rx          # merge smaller into larger
        self.parent[ry] = rx
        if self.rank[rx] == self.rank[ry]:
            self.rank[rx] += 1
        self.count -= 1
        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)

# Use cases:
# 1. Number of connected components
uf = UnionFind(5)
uf.union(0, 1); uf.union(2, 3)
uf.count                 # 3 (components: {0,1}, {2,3}, {4})

# 2. Detect cycle in undirected graph
def has_cycle(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        if not uf.union(u, v):   # already connected → cycle!
            return True
    return False

# 3. Minimum Spanning Tree (Kruskal's)
def kruskal_mst(n, edges):
    edges.sort(key=lambda e: e[2])   # sort by weight
    uf = UnionFind(n)
    mst = []
    for u, v, w in edges:
        if uf.union(u, v):
            mst.append((u, v, w))
    return mst if len(mst) == n - 1 else []
```

---

## 25. Tries

```python
class TrieNode:
    def __init__(self):
        self.children = {}        # char → TrieNode
        self.is_end   = False     # marks end of a complete word
        self.count    = 0         # number of words passing through this node

class Trie:
    def __init__(self):
        self.root = TrieNode()

    # Insert — O(m) where m = word length
    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
            node.count += 1
        node.is_end = True

    # Search — O(m)
    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children: return False
            node = node.children[ch]
        return node.is_end

    # Starts with (prefix search) — O(m)
    def starts_with(self, prefix):
        node = self.root
        for ch in prefix:
            if ch not in node.children: return False
            node = node.children[ch]
        return True

    # Count words with prefix — O(m)
    def count_prefix(self, prefix):
        node = self.root
        for ch in prefix:
            if ch not in node.children: return 0
            node = node.children[ch]
        return node.count

    # Delete — O(m)
    def delete(self, word):
        def _delete(node, word, depth):
            if depth == len(word):
                if node.is_end: node.is_end = False
                return len(node.children) == 0
            ch = word[depth]
            if ch not in node.children: return False
            should_delete = _delete(node.children[ch], word, depth + 1)
            if should_delete:
                del node.children[ch]
                return len(node.children) == 0 and not node.is_end
            return False
        _delete(self.root, word, 0)

    # Get all words with prefix (autocomplete)
    def autocomplete(self, prefix):
        node = self.root
        for ch in prefix:
            if ch not in node.children: return []
            node = node.children[ch]
        # DFS to collect all words from this node
        results = []
        def dfs(n, path):
            if n.is_end: results.append(prefix[:-len(path)] + path if path else prefix)
            for ch, child in n.children.items():
                dfs(child, path + ch)
        dfs(node, "")
        return results

# Use with array of booleans (faster for lowercase letters only)
class TrieNodeArray:
    def __init__(self):
        self.children = [None] * 26
        self.is_end = False

    def _idx(self, ch): return ord(ch) - ord('a')
```

---

## 26. Sorting Algorithms

### All Sorting Algorithms with Complexity

| Algorithm | Best | Average | Worst | Space | Stable? |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | ✅ |
| Radix Sort | O(nk) | O(nk) | O(nk) | O(n+k) | ✅ |
| Python Timsort | O(n) | O(n log n) | O(n log n) | O(n) | ✅ |

```python
# BUBBLE SORT — swap adjacent out-of-order elements
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(n - i - 1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped: break   # already sorted (O(n) best case)
    return arr

# SELECTION SORT — find minimum and put at front
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr

# INSERTION SORT — insert each element into sorted portion
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]    # shift right
            j -= 1
        arr[j+1] = key
    return arr

# MERGE SORT — divide and conquer, guaranteed O(n log n)
def merge_sort(arr):
    if len(arr) <= 1: return arr
    mid = len(arr) // 2
    left  = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

# QUICK SORT — partition around pivot
def quick_sort(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low < high:
        pivot_idx = partition(arr, low, high)
        quick_sort(arr, low, pivot_idx - 1)
        quick_sort(arr, pivot_idx + 1, high)
    return arr

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

# HEAP SORT — use max-heap property
def heap_sort(arr):
    n = len(arr)
    # Build max-heap
    for i in range(n//2 - 1, -1, -1):
        heapify(arr, n, i)
    # Extract max one by one
    for i in range(n-1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]   # move max to end
        heapify(arr, i, 0)
    return arr

def heapify(arr, n, i):
    largest = i
    left, right = 2*i+1, 2*i+2
    if left  < n and arr[left]  > arr[largest]: largest = left
    if right < n and arr[right] > arr[largest]: largest = right
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

# COUNTING SORT — O(n+k) for integer arrays in range [0, k]
def counting_sort(arr):
    if not arr: return arr
    max_val = max(arr)
    count = [0] * (max_val + 1)
    for n in arr: count[n] += 1
    result = []
    for val, freq in enumerate(count):
        result.extend([val] * freq)
    return result

# Python's built-in sort (Timsort — use this in interviews!)
arr = [3, 1, 4, 1, 5, 9, 2, 6]
arr.sort()                           # in-place, O(n log n)
sorted_arr = sorted(arr)             # returns new list
sorted(arr, key=lambda x: -x)       # descending
sorted(arr, reverse=True)            # descending
sorted(["banana","apple"], key=len)  # by custom key
```

---

## 27. Binary Search — All Variants

```python
import bisect

# CLASSIC BINARY SEARCH — O(log n)
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2   # avoid overflow (same as (l+r)//2 in Python)
        if nums[mid] == target:   return mid
        elif nums[mid] < target:  left = mid + 1
        else:                     right = mid - 1
    return -1

# FIND LEFTMOST (first occurrence) of target
def search_left(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < target: left = mid + 1
        else:                  right = mid
    return left if left < len(nums) and nums[left] == target else -1

# FIND RIGHTMOST (last occurrence) of target
def search_right(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] <= target: left = mid + 1
        else:                   right = mid
    return left - 1 if left > 0 and nums[left-1] == target else -1

# SEARCH IN ROTATED SORTED ARRAY
def search_rotated(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target: return mid
        if nums[left] <= nums[mid]:   # left half is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:                         # right half is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1

# FIND PEAK ELEMENT
def find_peak(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < nums[mid + 1]:
            left = mid + 1   # peak is to the right
        else:
            right = mid      # peak is to the left or at mid
    return left

# SEARCH IN 2D MATRIX (sorted row by row, each row's first > prev row's last)
def search_matrix(matrix, target):
    rows, cols = len(matrix), len(matrix[0])
    left, right = 0, rows * cols - 1
    while left <= right:
        mid = (left + right) // 2
        val = matrix[mid // cols][mid % cols]
        if val == target:   return True
        elif val < target:  left = mid + 1
        else:               right = mid - 1
    return False

# BINARY SEARCH ON ANSWER (bisect on result space)
# Example: find minimum capacity to ship packages in D days
def ship_within_days(weights, days):
    def can_ship(capacity):
        needed_days = 1
        current_load = 0
        for w in weights:
            if current_load + w > capacity:
                needed_days += 1
                current_load = 0
            current_load += w
        return needed_days <= days

    left, right = max(weights), sum(weights)
    while left < right:
        mid = (left + right) // 2
        if can_ship(mid):
            right = mid     # try smaller capacity
        else:
            left = mid + 1
    return left

# Python bisect module (binary search on sorted lists)
import bisect
arr = [1, 3, 5, 7, 9]
bisect.bisect_left(arr, 5)       # 2 — leftmost position for 5
bisect.bisect_right(arr, 5)      # 3 — rightmost position for 5
bisect.insort_left(arr, 4)       # insert 4 in sorted order → [1,3,4,5,7,9]
bisect.insort_right(arr, 5)      # insert 5 after existing 5s
```

---

## 28. Dynamic Programming

### Core Concepts

```python
# Memoization (Top-Down): cache recursive results
# Tabulation (Bottom-Up): fill DP table iteratively
# State Transition: dp[i] depends on previous dp states

# Template 1: Fibonacci (Memoization)
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)

# Template 2: Fibonacci (Tabulation)
def fib_tab(n):
    if n < 2: return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

# Template 3: Fibonacci (Space optimized)
def fib_opt(n):
    a, b = 0, 1
    for _ in range(n): a, b = b, a + b
    return a
```

---

### Classic DP Problems

```python
# 1. CLIMBING STAIRS (1 or 2 steps) — same as Fibonacci
def climb_stairs(n):
    if n <= 2: return n
    a, b = 1, 2
    for _ in range(3, n + 1): a, b = b, a + b
    return b

# 2. COIN CHANGE (minimum coins to make amount)
# dp[i] = min coins needed for amount i
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for amt in range(1, amount + 1):
        for coin in coins:
            if coin <= amt:
                dp[amt] = min(dp[amt], dp[amt - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1

# 3. COIN CHANGE — number of ways (combinations)
def change(amount, coins):
    dp = [0] * (amount + 1)
    dp[0] = 1
    for coin in coins:             # outer loop: coins (avoids counting permutations)
        for amt in range(coin, amount + 1):
            dp[amt] += dp[amt - coin]
    return dp[amount]

# 4. 0/1 KNAPSACK — max value with weight limit
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for w in range(capacity + 1):
            # Don't take item i-1
            dp[i][w] = dp[i-1][w]
            # Take item i-1 (if it fits)
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i][w], dp[i-1][w - weights[i-1]] + values[i-1])
    return dp[n][capacity]

# 5. LONGEST COMMON SUBSEQUENCE (LCS)
def lcs(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]

# 6. LONGEST INCREASING SUBSEQUENCE (LIS) — O(n log n)
def lis(nums):
    tails = []   # tails[i] = smallest tail of LIS of length i+1
    for n in nums:
        pos = bisect.bisect_left(tails, n)
        if pos == len(tails): tails.append(n)
        else:                 tails[pos] = n
    return len(tails)

# 7. EDIT DISTANCE (Levenshtein)
def edit_distance(word1, word2):
    m, n = len(word1), len(word2)
    dp = [[0]*(n+1) for _ in range(m+1)]
    for i in range(m+1): dp[i][0] = i  # delete all of word1
    for j in range(n+1): dp[0][j] = j  # insert all of word2
    for i in range(1, m+1):
        for j in range(1, n+1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]      # no operation needed
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],    # delete from word1
                    dp[i][j-1],    # insert into word1
                    dp[i-1][j-1]   # replace
                )
    return dp[m][n]

# 8. UNIQUE PATHS (grid, only right/down moves)
def unique_paths(m, n):
    dp = [[1]*n for _ in range(m)]
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    return dp[m-1][n-1]

# 9. HOUSE ROBBER (no two adjacent)
def rob(nums):
    prev2 = prev1 = 0
    for n in nums:
        prev2, prev1 = prev1, max(prev1, prev2 + n)
    return prev1

# 10. WORD BREAK
def word_break(s, word_dict):
    word_set = set(word_dict)
    dp = [False] * (len(s) + 1)
    dp[0] = True
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    return dp[len(s)]

# 11. LONGEST PALINDROMIC SUBSEQUENCE
def longest_palindromic_subseq(s):
    return lcs(s, s[::-1])   # LCS of string with its reverse!

# 12. MATRIX CHAIN MULTIPLICATION — O(n³)
def matrix_chain(dims):
    n = len(dims) - 1   # number of matrices
    dp = [[0]*n for _ in range(n)]
    for length in range(2, n+1):         # chain length
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = float('inf')
            for k in range(i, j):
                cost = dp[i][k] + dp[k+1][j] + dims[i]*dims[k+1]*dims[j+1]
                dp[i][j] = min(dp[i][j], cost)
    return dp[0][n-1]
```

---

## 29. Recursion & Backtracking

### Backtracking Template

```python
# Template:
# def backtrack(state, choices):
#     if is_solution(state):
#         record(state)
#         return
#     for choice in choices:
#         if is_valid(choice, state):
#             make_choice(choice, state)
#             backtrack(state, remaining_choices)
#             undo_choice(choice, state)   # BACKTRACK!

# 1. ALL SUBSETS (Power Set)
def subsets(nums):
    result = []
    def backtrack(start, current):
        result.append(current[:])   # add copy at every state
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()           # undo
    backtrack(0, [])
    return result

# 2. ALL PERMUTATIONS
def permutations(nums):
    result = []
    def backtrack(current, remaining):
        if not remaining:
            result.append(current[:])
            return
        for i in range(len(remaining)):
            current.append(remaining[i])
            backtrack(current, remaining[:i] + remaining[i+1:])
            current.pop()
    backtrack([], nums)
    return result

# Optimized permutations (in-place swap)
def permutations_swap(nums):
    result = []
    def backtrack(start):
        if start == len(nums):
            result.append(nums[:])
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]
            backtrack(start + 1)
            nums[start], nums[i] = nums[i], nums[start]   # undo
    backtrack(0)
    return result

# 3. COMBINATIONS (choose k from n)
def combinations(n, k):
    result = []
    def backtrack(start, current):
        if len(current) == k:
            result.append(current[:])
            return
        for i in range(start, n + 1):
            current.append(i)
            backtrack(i + 1, current)
            current.pop()
    backtrack(1, [])
    return result

# 4. COMBINATION SUM (can reuse elements)
def combination_sum(candidates, target):
    candidates.sort()
    result = []
    def backtrack(start, current, remaining):
        if remaining == 0:
            result.append(current[:])
            return
        for i in range(start, len(candidates)):
            if candidates[i] > remaining: break   # pruning
            current.append(candidates[i])
            backtrack(i, current, remaining - candidates[i])  # i (not i+1) → can reuse
            current.pop()
    backtrack(0, [], target)
    return result

# 5. N-QUEENS
def n_queens(n):
    result = []
    cols = set()
    diag1 = set()   # row - col (top-left to bottom-right diagonal)
    diag2 = set()   # row + col (top-right to bottom-left diagonal)
    board = [['.']*n for _ in range(n)]

    def backtrack(row):
        if row == n:
            result.append([''.join(r) for r in board])
            return
        for col in range(n):
            if col in cols or (row-col) in diag1 or (row+col) in diag2:
                continue
            board[row][col] = 'Q'
            cols.add(col); diag1.add(row-col); diag2.add(row+col)
            backtrack(row + 1)
            board[row][col] = '.'
            cols.remove(col); diag1.remove(row-col); diag2.remove(row+col)

    backtrack(0)
    return result

# 6. SUDOKU SOLVER
def solve_sudoku(board):
    empty = [(r,c) for r in range(9) for c in range(9) if board[r][c] == '.']

    def is_valid(r, c, ch):
        if ch in board[r]: return False
        if ch in [board[i][c] for i in range(9)]: return False
        box_r, box_c = (r//3)*3, (c//3)*3
        if ch in [board[i][j] for i in range(box_r, box_r+3) for j in range(box_c, box_c+3)]:
            return False
        return True

    def backtrack(idx):
        if idx == len(empty): return True
        r, c = empty[idx]
        for ch in '123456789':
            if is_valid(r, c, ch):
                board[r][c] = ch
                if backtrack(idx + 1): return True
                board[r][c] = '.'   # undo
        return False

    backtrack(0)
```

---

## 30. Greedy Algorithms

```python
# Greedy: make locally optimal choices at each step, hoping for global optimum
# Works when problem has: greedy choice property + optimal substructure

# 1. ACTIVITY SELECTION (interval scheduling)
# Select maximum non-overlapping activities
def activity_selection(activities):
    # Sort by end time
    activities.sort(key=lambda x: x[1])
    selected = [activities[0]]
    for start, end in activities[1:]:
        if start >= selected[-1][1]:   # no overlap
            selected.append((start, end))
    return selected

# 2. MEETING ROOMS II (minimum meeting rooms needed)
import heapq
def min_meeting_rooms(intervals):
    if not intervals: return 0
    intervals.sort(key=lambda x: x[0])
    rooms = []   # min-heap of end times
    for start, end in intervals:
        if rooms and rooms[0] <= start:
            heapq.heapreplace(rooms, end)   # reuse room
        else:
            heapq.heappush(rooms, end)      # new room
    return len(rooms)

# 3. JUMP GAME (can you reach the end?)
def can_jump(nums):
    max_reach = 0
    for i, n in enumerate(nums):
        if i > max_reach: return False   # can't reach this position
        max_reach = max(max_reach, i + n)
    return True

# 4. JUMP GAME II (minimum jumps)
def jump_min(nums):
    jumps = current_end = farthest = 0
    for i in range(len(nums) - 1):
        farthest = max(farthest, i + nums[i])
        if i == current_end:            # used up current jump
            jumps += 1
            current_end = farthest
    return jumps

# 5. GAS STATION (circular route)
def can_complete_circuit(gas, cost):
    if sum(gas) < sum(cost): return -1
    tank = start = 0
    for i in range(len(gas)):
        tank += gas[i] - cost[i]
        if tank < 0:       # can't continue from current start
            start = i + 1
            tank = 0
    return start

# 6. TASK SCHEDULER (CPU scheduling with cooldown)
from collections import Counter
import heapq
def least_interval(tasks, n):
    freq = Counter(tasks)
    max_heap = [-f for f in freq.values()]
    heapq.heapify(max_heap)
    time = 0
    queue = deque()   # (neg_freq, available_at_time)
    while max_heap or queue:
        time += 1
        if max_heap:
            freq = 1 + heapq.heappop(max_heap)   # decrement count
            if freq: queue.append((freq, time + n))
        if queue and queue[0][1] == time:
            heapq.heappush(max_heap, queue.popleft()[0])
    return time

# 7. FRACTIONAL KNAPSACK
def fractional_knapsack(items, capacity):
    items.sort(key=lambda x: x[1]/x[0], reverse=True)  # sort by value/weight ratio
    total_value = 0
    for weight, value in items:
        if capacity >= weight:
            total_value += value
            capacity -= weight
        else:
            total_value += value * (capacity / weight)  # take fraction
            break
    return total_value

# 8. HUFFMAN ENCODING (greedy tree building)
def huffman(chars, freqs):
    heap = list(zip(freqs, chars))
    heapq.heapify(heap)
    while len(heap) > 1:
        f1, c1 = heapq.heappop(heap)
        f2, c2 = heapq.heappop(heap)
        heapq.heappush(heap, (f1 + f2, c1 + c2))
    return heap[0]
```

---

## 🔑 Quick Reference: Complexity Summary

### Data Structure Operations

| Structure | Access | Search | Insert | Delete | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Array (List)** | O(1) | O(n) | O(n) | O(n) | O(1) at end (amortized) |
| **Linked List** | O(n) | O(n) | O(1) | O(1) | Given node reference |
| **Stack** | O(n) | O(n) | O(1) | O(1) | LIFO |
| **Queue** | O(n) | O(n) | O(1) | O(1) | FIFO, use deque |
| **Hash Map** | — | O(1) avg | O(1) avg | O(1) avg | O(n) worst |
| **Hash Set** | — | O(1) avg | O(1) avg | O(1) avg | |
| **BST** | O(h) | O(h) | O(h) | O(h) | h=O(log n) if balanced |
| **Heap** | O(1) min/max | O(n) | O(log n) | O(log n) | |
| **Trie** | O(m) | O(m) | O(m) | O(m) | m = key length |

### Algorithm Patterns vs. Problem Types

| Pattern | When to Use | Key Structures |
| :--- | :--- | :--- |
| Two Pointers | Sorted array, pairs/triplets | Array |
| Sliding Window | Subarray/substring problems | Array, HashMap |
| Fast & Slow Pointers | Cycle detection, middle of list | Linked List |
| BFS | Shortest path (unweighted), level order | Queue, Graph |
| DFS | Connected components, paths, cycle detection | Stack/Recursion, Graph |
| Binary Search | Sorted array, answer space search | Sorted Array |
| Monotonic Stack | Next greater/smaller element | Stack |
| Union-Find | Connected components, cycle detection | Array |
| Heap | Top K elements, median stream, Dijkstra | Heap |
| DP | Optimal substructure + overlapping subproblems | Array/Dict |
| Backtracking | Enumerate all solutions (subsets, permutations) | Recursion |
| Greedy | Locally optimal → globally optimal | Sorted Array, Heap |
| Trie | Prefix matching, autocomplete | Trie Node |
| Topological Sort | Task scheduling, dependency ordering | Graph + Queue |

---

# PART 3 — Advanced Python

---

## 31. Closures & Scope

```python
# Closure: a function that remembers variables from its enclosing scope
# even after the enclosing function has returned.

def make_counter(start=0):
    count = start               # This variable is "enclosed"
    def increment(step=1):
        nonlocal count          # nonlocal: modify enclosing scope variable
        count += step
        return count
    def reset():
        nonlocal count
        count = start
    return increment, reset

inc, rst = make_counter(10)
inc()    # 11
inc(5)   # 16
rst()
inc()    # 11

# --- SCOPE RULES: LEGB ---
# L - Local:    inside the current function
# E - Enclosing: inside enclosing functions (for closures)
# G - Global:   module-level
# B - Built-in: Python built-ins (len, print, etc.)

x = "global"

def outer():
    x = "enclosing"
    def inner():
        x = "local"
        print(x)          # "local"
    inner()
    print(x)              # "enclosing"

outer()
print(x)                  # "global"

# global keyword: modify module-level variable from inside a function
total = 0
def add(n):
    global total
    total += n

add(5); add(3)
print(total)              # 8

# Common closure gotcha: late binding in loops
funcs = [lambda: i for i in range(3)]
funcs[0]()               # 2  (all closures capture same 'i', which ends at 2!)

# Fix: use default argument to capture value at definition time
funcs = [lambda i=i: i for i in range(3)]
funcs[0]()               # 0 ✅
funcs[1]()               # 1 ✅
```

---

## 32. Context Managers

```python
# Context managers handle resource setup/teardown automatically via with statement

# Using __enter__ and __exit__
class ManagedFile:
    def __init__(self, filename, mode='r'):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file          # value bound to 'as' variable

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        # Return False (or None) to propagate exceptions
        # Return True to suppress exceptions
        return False

with ManagedFile('test.txt', 'r') as f:
    content = f.read()
# file is closed here automatically, even if an exception occurred

# --- Using contextlib.contextmanager decorator ---
from contextlib import contextmanager

@contextmanager
def timer(label=""):
    import time
    start = time.perf_counter()
    try:
        yield                      # everything before yield = __enter__
    finally:
        elapsed = time.perf_counter() - start
        print(f"{label}: {elapsed:.4f}s")  # everything after yield = __exit__

with timer("sort operation"):
    sorted([3,1,2,5,4] * 10000)

# Suppress exceptions with contextlib.suppress
from contextlib import suppress
with suppress(FileNotFoundError):
    open("nonexistent_file.txt")   # no error raised

# Multiple context managers on one line
with open('in.txt') as fin, open('out.txt', 'w') as fout:
    fout.write(fin.read())

# contextlib.ExitStack: dynamic number of context managers
from contextlib import ExitStack
files = ['a.txt', 'b.txt', 'c.txt']
with ExitStack() as stack:
    opened = [stack.enter_context(open(f)) for f in files]
```

---

## 33. Abstract Base Classes (abc module)

```python
from abc import ABC, abstractmethod

# ABC enforces that subclasses implement required methods
class Shape(ABC):
    @abstractmethod
    def area(self) -> float:      # must be implemented by subclasses
        pass

    @abstractmethod
    def perimeter(self) -> float:
        pass

    def describe(self):           # concrete method (shared implementation)
        return f"Area={self.area():.2f}, Perimeter={self.perimeter():.2f}"

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    def area(self):
        import math; return math.pi * self.radius ** 2
    def perimeter(self):
        import math; return 2 * math.pi * self.radius

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w = w; self.h = h
    def area(self):       return self.w * self.h
    def perimeter(self):  return 2 * (self.w + self.h)

# Shape()           # TypeError: Can't instantiate abstract class
Circle(5).area()    # 78.53...
Circle(5).describe()

# Abstract class method and property
class Animal(ABC):
    @property
    @abstractmethod
    def sound(self) -> str: pass

    @classmethod
    @abstractmethod
    def kingdom(cls) -> str: pass

# Check if a class implements an ABC without inheriting
from abc import ABCMeta
class MySeq(metaclass=ABCMeta):
    @abstractmethod
    def __len__(self): pass

isinstance([], MySeq)      # True — list implements __len__
```

---

## 34. `__slots__`

```python
# By default, each instance stores its attributes in a __dict__ (a hash map)
# __slots__ replaces __dict__ with a fixed array of attributes
# Result: less memory usage, faster attribute access

class WithDict:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    # has __dict__, can add arbitrary attributes

class WithSlots:
    __slots__ = ('x', 'y')      # declare allowed attributes
    def __init__(self, x, y):
        self.x = x
        self.y = y
    # NO __dict__, cannot add arbitrary attributes

p = WithSlots(1, 2)
p.x                             # 1
p.z = 3                         # AttributeError: 'WithSlots' has no attribute 'z'

# Memory comparison (slots uses ~40-60% less memory per instance)
import sys
sys.getsizeof(WithDict(1,2).__dict__)    # ~200+ bytes for dict overhead
# WithSlots has no __dict__ at all

# Use __slots__ when: creating millions of small objects (Point, Particle, etc.)
# Don't use __slots__ when: class needs dynamic attribute addition
```

---

## 35. Descriptors

```python
# A descriptor is any object that implements __get__, __set__, or __delete__
# They power Python's property, classmethod, staticmethod, and method binding

class Validator:
    """Descriptor that validates numeric values."""
    def __init__(self, min_val, max_val):
        self.min_val = min_val
        self.max_val = max_val
        self.name = None

    def __set_name__(self, owner, name):
        self.name = name           # called when class is defined

    def __get__(self, obj, objtype=None):
        if obj is None: return self    # accessed from class, not instance
        return getattr(obj, f'_{self.name}', None)

    def __set__(self, obj, value):
        if not (self.min_val <= value <= self.max_val):
            raise ValueError(f"{self.name} must be between {self.min_val} and {self.max_val}")
        setattr(obj, f'_{self.name}', value)

    def __delete__(self, obj):
        delattr(obj, f'_{self.name}')

class Person:
    age    = Validator(0, 150)      # Descriptor instance
    height = Validator(0, 300)

    def __init__(self, age, height):
        self.age = age             # calls Validator.__set__
        self.height = height

p = Person(25, 175)
p.age                              # 25 (calls Validator.__get__)
p.age = 200                        # ValueError: age must be between 0 and 150

# __getattr__ vs __getattribute__
class Demo:
    def __getattr__(self, name):
        # Called ONLY when normal attribute lookup fails
        return f"'{name}' not found"

    def __getattribute__(self, name):
        # Called for EVERY attribute access — use with extreme care
        return super().__getattribute__(name)

d = Demo()
d.x              # '__x__ not found'  (triggers __getattr__ since x doesn't exist)
```

---

## 36. Regular Expressions (`re` module)

```python
import re

# --- Core Functions ---
re.search(pattern, string)   # find first match anywhere → Match or None
re.match(pattern, string)    # match at BEGINNING only → Match or None
re.fullmatch(pattern, string)# match ENTIRE string → Match or None
re.findall(pattern, string)  # list of all matches (strings)
re.finditer(pattern, string) # iterator of Match objects
re.sub(pattern, repl, string)# replace all matches
re.split(pattern, string)    # split on pattern

# --- Pattern Syntax ---
# .       any character except newline
# \d      digit [0-9]        \D  non-digit
# \w      word char [a-zA-Z0-9_]   \W  non-word
# \s      whitespace         \S  non-whitespace
# \b      word boundary
# ^       start of string    $   end of string
# *       0 or more          +   1 or more    ?  0 or 1
# {n}     exactly n          {n,m}  between n and m
# [abc]   character class    [^abc]  negated class
# (abc)   capturing group    (?:abc)  non-capturing group
# |       alternation (OR)   (?=abc) lookahead  (?!abc) negative lookahead

# --- Examples ---
# Extract all numbers from string
re.findall(r'\d+', "abc123def456ghi789")    # ['123', '456', '789']

# Validate email
pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
bool(re.fullmatch(pattern, "user@example.com"))   # True

# Validate phone number
re.fullmatch(r'\d{10}', "9876543210")             # Match

# Find words
re.findall(r'\b[A-Z][a-z]+\b', "Hello World foo Bar")  # ['Hello', 'World', 'Bar']

# Named groups
m = re.search(r'(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})', '2024-07-06')
m.group('year')    # '2024'
m.group('month')   # '07'
m.groupdict()      # {'year': '2024', 'month': '07', 'day': '06'}

# Replace with function
def double(m): return str(int(m.group()) * 2)
re.sub(r'\d+', double, "I have 3 cats and 5 dogs")  # "I have 6 cats and 10 dogs"

# Split on multiple delimiters
re.split(r'[,;|\s]+', "a,b; c|d  e")   # ['a', 'b', 'c', 'd', 'e']

# Compile for repeated use (faster)
email_re = re.compile(r'^[\w.+-]+@[\w-]+\.[a-zA-Z]{2,}$', re.IGNORECASE)
email_re.match("Alice@Example.COM")   # Match

# Flags
re.IGNORECASE  # re.I — case-insensitive
re.MULTILINE   # re.M — ^ and $ match start/end of each line
re.DOTALL      # re.S — . matches newline too
re.VERBOSE     # re.X — allow whitespace and comments in pattern

pattern = re.compile(r"""
    (\d{3})   # area code
    [-.\s]?   # separator
    (\d{3})   # prefix
    [-.\s]?   # separator
    (\d{4})   # line number
""", re.VERBOSE)
```

---

## 37. `async` / `await` — Asyncio Basics

```python
import asyncio

# Async function (coroutine): returns a coroutine object when called
# Must be awaited inside another async function or run with asyncio.run()

async def fetch_data(url, delay=1):
    print(f"Fetching {url}...")
    await asyncio.sleep(delay)    # non-blocking wait (gives control back to event loop)
    print(f"Done: {url}")
    return f"Data from {url}"

# Run a single coroutine
result = asyncio.run(fetch_data("https://api.example.com"))

# Run multiple coroutines CONCURRENTLY (not in parallel — single thread)
async def main():
    # Sequential: total time = 2 + 3 = 5 seconds
    r1 = await fetch_data("url1", 2)
    r2 = await fetch_data("url2", 3)

    # Concurrent: total time = max(2, 3) = 3 seconds
    r1, r2 = await asyncio.gather(
        fetch_data("url1", 2),
        fetch_data("url2", 3)
    )

    # With timeout
    try:
        result = await asyncio.wait_for(fetch_data("url", 5), timeout=3.0)
    except asyncio.TimeoutError:
        print("Request timed out!")

asyncio.run(main())

# Async context manager
class AsyncDBConnection:
    async def __aenter__(self):
        print("Connecting...")
        await asyncio.sleep(0.1)    # simulate connection time
        return self

    async def __aexit__(self, *args):
        print("Disconnecting...")

async def use_db():
    async with AsyncDBConnection() as db:
        pass   # use db here

# Async generator
async def async_range(n):
    for i in range(n):
        await asyncio.sleep(0)    # yield control between iterations
        yield i

async def consume():
    async for i in async_range(5):
        print(i)

# asyncio.Queue: for producer-consumer patterns
async def producer(queue):
    for i in range(5):
        await queue.put(i)
        await asyncio.sleep(0.1)
    await queue.put(None)   # sentinel to stop consumer

async def consumer(queue):
    while True:
        item = await queue.get()
        if item is None: break
        print(f"Consumed: {item}")
        queue.task_done()

async def main():
    q = asyncio.Queue()
    await asyncio.gather(producer(q), consumer(q))
```

---

# PART 4 — Advanced DSA

---

## 38. Bit Manipulation

```python
# Binary representations
bin(42)              # '0b101010'
int('101010', 2)     # 42

# Basic operations
n = 42               # 0b101010

# AND: 1 only when both bits are 1
n & 0xFF             # mask lower 8 bits

# OR: 1 when at least one bit is 1
n | (1 << 3)         # set bit 3

# XOR: 1 when bits differ
n ^ n                # 0  (XOR with self = 0)
n ^ 0                # n  (XOR with 0 = identity)
a ^ b ^ a            # b  (used for "find single number" trick)

# NOT: flip all bits
~n                   # -(n+1) in Python (two's complement)

# LEFT SHIFT: multiply by 2^k
n << 1               # 84  (42 * 2)
n << 2               # 168 (42 * 4)
1 << k               # 2^k

# RIGHT SHIFT: divide by 2^k (floor)
n >> 1               # 21  (42 // 2)
n >> 2               # 10  (42 // 4)

# ============================================
# ESSENTIAL BIT TRICKS
# ============================================

# Check if bit i is set
def is_set(n, i):
    return (n >> i) & 1 == 1

# Set bit i
def set_bit(n, i):
    return n | (1 << i)

# Clear bit i
def clear_bit(n, i):
    return n & ~(1 << i)

# Toggle bit i
def toggle_bit(n, i):
    return n ^ (1 << i)

# Get lowest set bit (rightmost 1)
def lowest_set_bit(n):
    return n & (-n)          # (-n) is two's complement

# Clear lowest set bit
def clear_lowest(n):
    return n & (n - 1)       # KEY TRICK: removes rightmost 1 bit

# Count set bits (Brian Kernighan's algorithm) — O(number of set bits)
def count_bits(n):
    count = 0
    while n:
        n &= (n - 1)         # clear lowest set bit each iteration
        count += 1
    return count

bin(12).count('1')            # built-in alternative: 2
n.bit_count()                 # Python 3.10+: 2

# Power of 2 check
def is_power_of_two(n):
    return n > 0 and (n & (n - 1)) == 0

# Power of 4 check
def is_power_of_four(n):
    return n > 0 and (n & (n-1)) == 0 and (n & 0xAAAAAAAA) == 0

# ============================================
# CLASSIC BIT PROBLEMS
# ============================================

# Single Number (all others appear twice) — XOR cancels pairs
def single_number(nums):
    result = 0
    for n in nums: result ^= n
    return result

# Single Number II (all others appear 3 times)
def single_number_ii(nums):
    ones = twos = 0
    for n in nums:
        ones = (ones ^ n) & ~twos
        twos = (twos ^ n) & ~ones
    return ones

# Single Number III (two numbers appear once, rest twice)
def single_number_iii(nums):
    xor = 0
    for n in nums: xor ^= n      # xor = a ^ b
    diff = xor & (-xor)          # isolate rightmost different bit
    a = 0
    for n in nums:
        if n & diff: a ^= n      # partition into two groups
    return [a, xor ^ a]

# Number of 1 bits (Hamming weight)
def hamming_weight(n):
    return bin(n).count('1')

# Hamming distance between two integers
def hamming_distance(x, y):
    return bin(x ^ y).count('1')

# Reverse bits (32-bit integer)
def reverse_bits(n):
    result = 0
    for _ in range(32):
        result = (result << 1) | (n & 1)
        n >>= 1
    return result

# Sum of two integers without + operator
def get_sum(a, b):
    mask = 0xFFFFFFFF
    while b & mask:
        carry = (a & b) << 1
        a = a ^ b
        b = carry
    return a if b == 0 else a & mask

# Missing number (0 to n, one missing)
def missing_number(nums):
    n = len(nums)
    return n * (n+1)//2 - sum(nums)    # math trick
    # XOR trick: return reduce(xor, range(len(nums)+1)) ^ reduce(xor, nums)

# Find two non-repeating numbers in array
def find_two_singles(nums):
    xor = 0
    for n in nums: xor ^= n          # xor = a ^ b
    bit = xor & (-xor)               # rightmost differing bit
    x = y = 0
    for n in nums:
        if n & bit: x ^= n
        else:       y ^= n
    return x, y

# Subsets using bit masking
def subsets_bitmask(nums):
    n = len(nums)
    result = []
    for mask in range(1 << n):        # 2^n masks
        subset = [nums[i] for i in range(n) if mask & (1 << i)]
        result.append(subset)
    return result
```

---

## 39. Math & Number Theory

```python
import math
from functools import reduce

# ============================================
# GCD & LCM
# ============================================
math.gcd(12, 8)                   # 4
math.gcd(48, 18)                  # 6
math.lcm(4, 6)                    # 12  (Python 3.9+)

# Manual GCD (Euclidean algorithm) — O(log min(a,b))
def gcd(a, b):
    while b: a, b = b, a % b
    return a

# LCM using GCD
def lcm(a, b):
    return a * b // gcd(a, b)

# GCD of a list
reduce(math.gcd, [12, 18, 24])    # 6

# ============================================
# PRIME NUMBERS
# ============================================

# Check if prime — O(√n)
def is_prime(n):
    if n < 2: return False
    if n < 4: return True
    if n % 2 == 0 or n % 3 == 0: return False
    i = 5
    while i * i <= n:
        if n % i == 0 or n % (i+2) == 0: return False
        i += 6
    return True

# Sieve of Eratosthenes — O(n log log n), find all primes up to n
def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    p = 2
    while p * p <= n:
        if is_prime[p]:
            for i in range(p*p, n+1, p):
                is_prime[i] = False
        p += 1
    return [i for i in range(2, n+1) if is_prime[i]]

sieve(30)   # [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]

# Prime factorization — O(√n)
def prime_factors(n):
    factors = []
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors.append(d)
            n //= d
        d += 1
    if n > 1: factors.append(n)
    return factors

prime_factors(360)   # [2, 2, 2, 3, 3, 5]

# Count divisors
def count_divisors(n):
    count = 0
    for i in range(1, int(math.sqrt(n)) + 1):
        if n % i == 0:
            count += 2 if i != n // i else 1
    return count

# ============================================
# MODULAR ARITHMETIC
# ============================================
MOD = 10**9 + 7

# All operations preserve mod property
(a + b) % MOD
(a - b + MOD) % MOD              # ensure non-negative
(a * b) % MOD
pow(a, b, MOD)                   # fast modular exponentiation O(log b)

# Modular inverse (when MOD is prime, Fermat's little theorem)
def mod_inverse(a, mod):
    return pow(a, mod - 2, mod)  # a^(mod-2) mod p

# ============================================
# COMBINATORICS
# ============================================
import math

math.factorial(10)               # 3628800
math.comb(10, 3)                 # 120  (C(10,3) = 10!/(3!*7!))
math.perm(10, 3)                 # 720  (P(10,3) = 10!/(7!))

# Modular combinatorics (for large n)
def precompute_factorials(n, mod):
    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i-1] * i % mod
    inv_fact = [1] * (n + 1)
    inv_fact[n] = pow(fact[n], mod - 2, mod)
    for i in range(n - 1, -1, -1):
        inv_fact[i] = inv_fact[i+1] * (i+1) % mod
    return fact, inv_fact

def comb_mod(n, r, fact, inv_fact, mod):
    if r < 0 or r > n: return 0
    return fact[n] * inv_fact[r] % mod * inv_fact[n-r] % mod

# ============================================
# FAST POWER (matrix exponentiation for Fibonacci)
# ============================================
def matrix_mult(A, B, mod):
    n = len(A)
    C = [[0]*n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if A[i][k]:
                for j in range(n):
                    C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % mod
    return C

def matrix_pow(M, p, mod):
    n = len(M)
    result = [[1 if i==j else 0 for j in range(n)] for i in range(n)]  # identity
    while p:
        if p & 1: result = matrix_mult(result, M, mod)
        M = matrix_mult(M, M, mod)
        p >>= 1
    return result

def fib_fast(n, mod=10**9+7):
    if n == 0: return 0
    M = [[1,1],[1,0]]
    return matrix_pow(M, n, mod)[0][1]

# ============================================
# DIGIT OPERATIONS
# ============================================
# Sum of digits
def digit_sum(n):
    return sum(int(d) for d in str(abs(n)))

# Number of digits
def num_digits(n):
    return len(str(abs(n))) if n != 0 else 1

# Reverse a number
def reverse_num(n):
    sign = -1 if n < 0 else 1
    return sign * int(str(abs(n))[::-1])
```

---

## 40. Interval Problems

```python
# ============================================
# MERGE INTERVALS
# ============================================
def merge_intervals(intervals):
    intervals.sort(key=lambda x: x[0])   # sort by start time
    merged = [intervals[0]]
    for start, end in intervals[1:]:
        if start <= merged[-1][1]:        # overlapping
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])
    return merged

# Example: [[1,3],[2,6],[8,10],[15,18]] → [[1,6],[8,10],[15,18]]

# ============================================
# INSERT INTERVAL
# ============================================
def insert_interval(intervals, new_interval):
    result = []
    i = 0
    n = len(intervals)
    # Add all intervals that end before new_interval starts
    while i < n and intervals[i][1] < new_interval[0]:
        result.append(intervals[i]); i += 1
    # Merge all overlapping intervals
    while i < n and intervals[i][0] <= new_interval[1]:
        new_interval[0] = min(new_interval[0], intervals[i][0])
        new_interval[1] = max(new_interval[1], intervals[i][1])
        i += 1
    result.append(new_interval)
    # Add remaining intervals
    result.extend(intervals[i:])
    return result

# ============================================
# MINIMUM PLATFORMS (meeting rooms II)
# ============================================
def min_platforms(arrivals, departures):
    arrivals.sort()
    departures.sort()
    platforms = max_platforms = 0
    i = j = 0
    while i < len(arrivals):
        if arrivals[i] <= departures[j]:
            platforms += 1; i += 1
        else:
            platforms -= 1; j += 1
        max_platforms = max(max_platforms, platforms)
    return max_platforms

# ============================================
# NON-OVERLAPPING INTERVALS (minimum removals)
# ============================================
def erase_overlap_intervals(intervals):
    intervals.sort(key=lambda x: x[1])   # sort by end time (greedy!)
    count = 0
    prev_end = float('-inf')
    for start, end in intervals:
        if start >= prev_end:
            prev_end = end   # keep this interval
        else:
            count += 1       # remove overlapping interval
    return count

# ============================================
# SWEEP LINE — Count max overlapping intervals
# ============================================
def max_overlapping(intervals):
    events = []
    for start, end in intervals:
        events.append((start, 1))    # +1 at start
        events.append((end, -1))     # -1 at end
    events.sort(key=lambda x: (x[0], x[1]))   # sort; -1 before +1 at same time
    max_count = current = 0
    for _, delta in events:
        current += delta
        max_count = max(max_count, current)
    return max_count
```

---

## 41. More Graph Algorithms

### Bellman-Ford (Negative Weights)

```python
# Dijkstra fails with negative weights — use Bellman-Ford
# O(VE) time — can detect negative cycles
def bellman_ford(n, edges, source):
    dist = [float('inf')] * n
    dist[source] = 0

    # Relax all edges V-1 times
    for _ in range(n - 1):
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w

    # Check for negative cycles (if we can still relax, there's a cycle)
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None   # Negative cycle detected!

    return dist

# Example
edges = [(0,1,4),(0,2,1),(2,1,2),(1,3,1),(2,3,5)]
bellman_ford(4, edges, 0)   # [0, 3, 1, 4]
```

---

### Floyd-Warshall (All-Pairs Shortest Path)

```python
# O(V³) time, O(V²) space
# Finds shortest path between ALL pairs of vertices
# Works with negative weights (not negative cycles)
def floyd_warshall(n, edges):
    INF = float('inf')
    dist = [[INF]*n for _ in range(n)]
    for i in range(n): dist[i][i] = 0
    for u, v, w in edges:
        dist[u][v] = w
        dist[v][u] = w   # for undirected

    for k in range(n):           # intermediate vertex
        for i in range(n):
            for j in range(n):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    # Check negative cycles: dist[i][i] < 0 for any i
    return dist   # dist[i][j] = shortest path from i to j
```

---

### Prim's Algorithm (Minimum Spanning Tree)

```python
import heapq
from collections import defaultdict

# O((V+E) log V) — greedy MST starting from any node
def prims_mst(n, edges):
    graph = defaultdict(list)
    for u, v, w in edges:
        graph[u].append((w, v))
        graph[v].append((w, u))

    visited = set()
    min_heap = [(0, 0, -1)]    # (weight, node, parent)
    mst_edges = []
    total_weight = 0

    while min_heap and len(visited) < n:
        w, u, parent = heapq.heappop(min_heap)
        if u in visited: continue
        visited.add(u)
        total_weight += w
        if parent != -1: mst_edges.append((parent, u, w))

        for edge_w, v in graph[u]:
            if v not in visited:
                heapq.heappush(min_heap, (edge_w, v, u))

    return mst_edges, total_weight
```

---

### Tarjan's Strongly Connected Components (SCC)

```python
# SCC: a maximal set of vertices where every vertex is reachable from every other
# O(V + E) using DFS + discovery times + low-link values

def tarjans_scc(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)

    disc = [-1] * n          # discovery time
    low  = [0]  * n          # lowest disc time reachable
    on_stack = [False] * n
    stack = []
    timer = [0]
    sccs = []

    def dfs(u):
        disc[u] = low[u] = timer[0]
        timer[0] += 1
        stack.append(u)
        on_stack[u] = True

        for v in graph[u]:
            if disc[v] == -1:           # not visited
                dfs(v)
                low[u] = min(low[u], low[v])
            elif on_stack[v]:           # back edge to ancestor
                low[u] = min(low[u], disc[v])

        if low[u] == disc[u]:          # u is root of an SCC
            scc = []
            while True:
                v = stack.pop()
                on_stack[v] = False
                scc.append(v)
                if v == u: break
            sccs.append(scc)

    for i in range(n):
        if disc[i] == -1:
            dfs(i)
    return sccs
```

---

### Kosaraju's SCC Algorithm

```python
def kosarajus_scc(n, edges):
    graph   = defaultdict(list)
    reverse = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        reverse[v].append(u)

    # Step 1: DFS on original graph, record finish order
    visited = set()
    finish_order = []
    def dfs1(u):
        visited.add(u)
        for v in graph[u]:
            if v not in visited: dfs1(v)
        finish_order.append(u)   # add AFTER all descendants

    for i in range(n):
        if i not in visited: dfs1(i)

    # Step 2: DFS on reverse graph in reverse finish order
    visited.clear()
    sccs = []
    def dfs2(u, scc):
        visited.add(u)
        scc.append(u)
        for v in reverse[u]:
            if v not in visited: dfs2(v, scc)

    for u in reversed(finish_order):
        if u not in visited:
            scc = []
            dfs2(u, scc)
            sccs.append(scc)
    return sccs
```

---

### Articulation Points & Bridges

```python
# Articulation point: removing it disconnects the graph
# Bridge: removing it disconnects the graph (edge version)
def find_bridges(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    disc = [-1] * n
    low  = [0]  * n
    timer = [0]
    bridges = []

    def dfs(u, parent):
        disc[u] = low[u] = timer[0]
        timer[0] += 1
        for v in graph[u]:
            if disc[v] == -1:
                dfs(v, u)
                low[u] = min(low[u], low[v])
                if low[v] > disc[u]:     # no back edge from subtree to u or above
                    bridges.append((u, v))
            elif v != parent:
                low[u] = min(low[u], disc[v])

    for i in range(n):
        if disc[i] == -1: dfs(i, -1)
    return bridges
```

---

## 42. Segment Tree

```python
# Segment Tree: range queries (sum, min, max) and point updates in O(log n)
# Space: O(4n) — build a full tree in an array of size 4n

class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (4 * self.n)
        if nums: self.build(nums, 0, 0, self.n - 1)

    def build(self, nums, node, start, end):
        if start == end:
            self.tree[node] = nums[start]
        else:
            mid = (start + end) // 2
            self.build(nums, 2*node+1, start, mid)
            self.build(nums, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    # Point update: set nums[idx] = val — O(log n)
    def update(self, idx, val, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if start == end:
            self.tree[node] = val
        else:
            mid = (start + end) // 2
            if idx <= mid:
                self.update(idx, val, 2*node+1, start, mid)
            else:
                self.update(idx, val, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    # Range sum query: sum of nums[l..r] — O(log n)
    def query(self, l, r, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if r < start or end < l:     # completely outside range
            return 0
        if l <= start and end <= r:  # completely inside range
            return self.tree[node]
        mid = (start + end) // 2
        left  = self.query(l, r, 2*node+1, start, mid)
        right = self.query(l, r, 2*node+2, mid+1, end)
        return left + right

# Usage
st = SegmentTree([1, 3, 5, 7, 9, 11])
st.query(1, 3)       # 3+5+7 = 15
st.update(1, 10)     # nums[1] = 10
st.query(1, 3)       # 10+5+7 = 22

# Lazy Propagation Segment Tree (range update + range query)
class LazySegTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (4 * n)
        self.lazy = [0] * (4 * n)

    def push_down(self, node, start, end):
        if self.lazy[node] != 0:
            mid = (start + end) // 2
            self.tree[2*node+1] += self.lazy[node] * (mid - start + 1)
            self.tree[2*node+2] += self.lazy[node] * (end - mid)
            self.lazy[2*node+1] += self.lazy[node]
            self.lazy[2*node+2] += self.lazy[node]
            self.lazy[node] = 0

    def range_update(self, l, r, val, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if r < start or end < l: return
        if l <= start and end <= r:
            self.tree[node] += val * (end - start + 1)
            self.lazy[node] += val
            return
        self.push_down(node, start, end)
        mid = (start + end) // 2
        self.range_update(l, r, val, 2*node+1, start, mid)
        self.range_update(l, r, val, 2*node+2, mid+1, end)
        self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def range_query(self, l, r, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if r < start or end < l: return 0
        if l <= start and end <= r: return self.tree[node]
        self.push_down(node, start, end)
        mid = (start + end) // 2
        return (self.range_query(l, r, 2*node+1, start, mid) +
                self.range_query(l, r, 2*node+2, mid+1, end))
```

---

## 43. Fenwick Tree (Binary Indexed Tree — BIT)

```python
# Simpler than Segment Tree; supports prefix sums and point updates in O(log n)
# Key insight: each index stores partial sum of a specific range based on lowest set bit

class FenwickTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)   # 1-indexed

    # Add val to position i — O(log n)
    def update(self, i, val):
        while i <= self.n:
            self.tree[i] += val
            i += i & (-i)    # move to next responsible node

    # Prefix sum [1..i] — O(log n)
    def prefix_sum(self, i):
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)    # move to parent
        return total

    # Range sum [l..r] — O(log n)
    def range_sum(self, l, r):
        return self.prefix_sum(r) - self.prefix_sum(l - 1)

    # Build from array — O(n log n)
    def build(self, nums):
        for i, val in enumerate(nums, 1):
            self.update(i, val)

# Usage
bit = FenwickTree(6)
for i, v in enumerate([1,3,5,7,9,11], 1):
    bit.update(i, v)

bit.prefix_sum(4)    # 1+3+5+7 = 16
bit.range_sum(2, 5)  # 3+5+7+9 = 24

bit.update(3, -5)    # nums[3] was 5, now 0 (add -5)
bit.range_sum(2, 5)  # 3+0+7+9 = 19

# Count inversions using BIT
def count_inversions(arr):
    max_val = max(arr)
    bit = FenwickTree(max_val)
    inversions = 0
    for i in range(len(arr) - 1, -1, -1):   # traverse right to left
        inversions += bit.prefix_sum(arr[i] - 1)   # count smaller elements to right
        bit.update(arr[i], 1)
    return inversions
```

---

## 44. Sparse Table (Range Minimum Query)

```python
import math

# Sparse Table: O(n log n) build, O(1) RMQ query (immutable array)
class SparseTable:
    def __init__(self, nums):
        n = len(nums)
        k = int(math.log2(n)) + 1 if n > 0 else 1
        self.LOG = [0] * (n + 1)
        for i in range(2, n + 1):
            self.LOG[i] = self.LOG[i // 2] + 1

        # table[i][j] = min of nums[i..i+2^j-1]
        self.table = [[float('inf')] * k for _ in range(n)]
        for i in range(n):
            self.table[i][0] = nums[i]

        j = 1
        while (1 << j) <= n:
            for i in range(n - (1 << j) + 1):
                self.table[i][j] = min(
                    self.table[i][j-1],
                    self.table[i + (1 << (j-1))][j-1]
                )
            j += 1

    # Range minimum query [l, r] — O(1)
    def query(self, l, r):
        k = self.LOG[r - l + 1]
        return min(self.table[l][k], self.table[r - (1 << k) + 1][k])

# Usage
st = SparseTable([2, 4, 3, 1, 6, 7, 8, 9, 1, 7])
st.query(0, 4)    # min(2,4,3,1,6) = 1
st.query(4, 7)    # min(6,7,8,9) = 6
```

---

## 45. QuickSelect (Kth Smallest/Largest in O(n) Average)

```python
import random

# QuickSelect: O(n) average, O(n²) worst — use random pivot to avoid worst case
def quickselect(nums, k):
    """Find kth smallest element (1-indexed k)."""
    k -= 1   # 0-indexed

    def partition(lo, hi):
        pivot_idx = random.randint(lo, hi)
        nums[pivot_idx], nums[hi] = nums[hi], nums[pivot_idx]
        pivot = nums[hi]
        i = lo
        for j in range(lo, hi):
            if nums[j] <= pivot:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
        nums[i], nums[hi] = nums[hi], nums[i]
        return i

    lo, hi = 0, len(nums) - 1
    while lo < hi:
        pivot_idx = partition(lo, hi)
        if pivot_idx == k:
            break
        elif pivot_idx < k:
            lo = pivot_idx + 1
        else:
            hi = pivot_idx - 1
    return nums[k]

# Kth LARGEST: use k = n - k + 1 for kth smallest
def kth_largest(nums, k):
    return quickselect(nums[:], len(nums) - k + 1)

# Python's built-in heapq.nsmallest / nlargest use a similar approach
# For kth largest in O(n log k): maintain a min-heap of size k
import heapq
def kth_largest_heap(nums, k):
    heap = nums[:k]
    heapq.heapify(heap)
    for n in nums[k:]:
        if n > heap[0]:
            heapq.heapreplace(heap, n)
    return heap[0]
```

---

## 46. Boyer-Moore Voting Algorithm

```python
# Find majority element (appears > n/2 times) — O(n) time, O(1) space
def majority_element(nums):
    candidate, count = None, 0
    for n in nums:
        if count == 0:
            candidate = n
        count += (1 if n == candidate else -1)
    # candidate is the majority element (guaranteed if majority exists)
    # If not guaranteed, verify: nums.count(candidate) > len(nums)//2
    return candidate

# Extended: Find all elements appearing > n/3 times (at most 2 such elements)
def majority_element_ii(nums):
    c1, c2, cnt1, cnt2 = None, None, 0, 0
    for n in nums:
        if n == c1:       cnt1 += 1
        elif n == c2:     cnt2 += 1
        elif cnt1 == 0:   c1, cnt1 = n, 1
        elif cnt2 == 0:   c2, cnt2 = n, 1
        else:             cnt1 -= 1; cnt2 -= 1
    # Verify both candidates
    threshold = len(nums) // 3
    return [c for c in [c1, c2] if c is not None and nums.count(c) > threshold]
```

---

## 47. Dutch National Flag (3-Way Partition)

```python
# Dijkstra's 3-way partition: sort array with 3 distinct values in O(n), O(1)
# Classic: sort array of 0s, 1s, 2s (Sort Colors)

def dutch_national_flag(nums):
    lo = mid = 0
    hi = len(nums) - 1
    while mid <= hi:
        if nums[mid] == 0:
            nums[lo], nums[mid] = nums[mid], nums[lo]
            lo += 1; mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:  # nums[mid] == 2
            nums[mid], nums[hi] = nums[hi], nums[mid]
            hi -= 1    # don't increment mid (swapped element is unexamined)

# Generalized: partition array around a pivot into <pivot, ==pivot, >pivot
def three_way_partition(arr, pivot):
    lo = mid = 0
    hi = len(arr) - 1
    while mid <= hi:
        if arr[mid] < pivot:
            arr[lo], arr[mid] = arr[mid], arr[lo]
            lo += 1; mid += 1
        elif arr[mid] == pivot:
            mid += 1
        else:
            arr[mid], arr[hi] = arr[hi], arr[mid]
            hi -= 1
    # After: arr[0..lo-1] < pivot, arr[lo..mid-1] == pivot, arr[mid..] > pivot
```

---

## 48. Rabin-Karp (Rolling Hash String Search)

```python
# O(n+m) average for single pattern search using rolling hash
def rabin_karp(text, pattern):
    BASE = 31
    MOD  = 10**9 + 7
    n, m = len(text), len(pattern)
    if m > n: return []

    def char_val(c): return ord(c) - ord('a') + 1

    # Compute hash of pattern and first window
    pattern_hash = 0
    window_hash  = 0
    power = 1   # BASE^(m-1)

    for i in range(m):
        pattern_hash = (pattern_hash * BASE + char_val(pattern[i])) % MOD
        window_hash  = (window_hash  * BASE + char_val(text[i]))    % MOD
        if i < m - 1: power = power * BASE % MOD

    matches = []
    for i in range(n - m + 1):
        if window_hash == pattern_hash:
            if text[i:i+m] == pattern:    # verify (avoid hash collision false positives)
                matches.append(i)
        if i < n - m:
            # Roll the hash: remove leftmost char, add new rightmost char
            window_hash = (window_hash - char_val(text[i]) * power) % MOD
            window_hash = (window_hash * BASE + char_val(text[i + m])) % MOD
            window_hash = (window_hash + MOD) % MOD   # ensure non-negative
    return matches

rabin_karp("abcabcabc", "abc")   # [0, 3, 6]
```

---

## 49. Advanced Dynamic Programming

### Bitmask DP

```python
# Use bitmask to represent subsets in DP
# Classic: Travelling Salesman Problem (TSP)

def tsp(dist):
    n = len(dist)
    INF = float('inf')
    # dp[mask][i] = min cost to visit all cities in mask, ending at city i
    dp = [[INF] * n for _ in range(1 << n)]
    dp[1][0] = 0   # start at city 0, only city 0 visited (mask = 0b0001)

    for mask in range(1 << n):
        for u in range(n):
            if not (mask >> u & 1) or dp[mask][u] == INF: continue
            for v in range(n):
                if mask >> v & 1: continue   # already visited
                new_mask = mask | (1 << v)
                dp[new_mask][v] = min(dp[new_mask][v], dp[mask][u] + dist[u][v])

    full_mask = (1 << n) - 1
    return min(dp[full_mask][i] + dist[i][0] for i in range(1, n))

# Minimum cost to cover all nodes (Set Cover DP)
def min_cost_cover(n, costs):
    # costs[mask] = cost of covering cities in mask
    dp = [float('inf')] * (1 << n)
    dp[0] = 0
    for mask in range(1 << n):
        if dp[mask] == float('inf'): continue
        for subset, cost in costs:
            new_mask = mask | subset
            dp[new_mask] = min(dp[new_mask], dp[mask] + cost)
    return dp[(1 << n) - 1]

# Count paths visiting all nodes (Hamiltonian Path count)
def count_hamiltonian_paths(graph, n):
    dp = [[0]*n for _ in range(1 << n)]
    for i in range(n): dp[1 << i][i] = 1   # single-node paths

    for mask in range(1 << n):
        for u in range(n):
            if not dp[mask][u]: continue
            for v in range(n):
                if mask >> v & 1: continue   # already visited
                if graph[u][v]:
                    dp[mask|(1<<v)][v] += dp[mask][u]

    full = (1 << n) - 1
    return sum(dp[full])
```

---

### Digit DP

```python
# Count numbers in [0, N] satisfying some digit-based property

# Count numbers from 1 to n with digit sum == target
def count_digit_sum(n, target):
    digits = [int(d) for d in str(n)]
    from functools import lru_cache

    @lru_cache(maxsize=None)
    def dp(pos, current_sum, tight, started):
        """
        pos:         current digit position
        current_sum: sum of digits so far
        tight:       if True, current number is bounded by n's prefix
        started:     if False, haven't placed a non-zero digit yet (handle leading zeros)
        """
        if pos == len(digits):
            return 1 if started and current_sum == target else 0

        limit = digits[pos] if tight else 9
        result = 0

        for d in range(0, limit + 1):
            new_tight   = tight and (d == limit)
            new_started = started or (d > 0)
            new_sum     = current_sum + d if new_started else 0
            if new_sum <= target:
                result += dp(pos + 1, new_sum, new_tight, new_started)

        return result

    return dp(0, 0, True, False)

# Count numbers with no two consecutive same digits from 1 to n
def count_no_consecutive(n):
    digits = [int(d) for d in str(n)]
    from functools import lru_cache

    @lru_cache(maxsize=None)
    def dp(pos, prev_digit, tight, started):
        if pos == len(digits):
            return 1 if started else 0
        limit = digits[pos] if tight else 9
        result = 0
        for d in range(0, limit + 1):
            if started and d == prev_digit: continue  # skip consecutive same digit
            result += dp(pos + 1, d, tight and d == limit, started or d > 0)
        return result

    return dp(0, -1, True, False)
```

---

### Interval DP

```python
# Used when the problem involves choosing a split point in a range
# State: dp[i][j] = answer for subarray/substring [i..j]

# Burst Balloons (pick order of elements to maximize score)
def max_coins(nums):
    nums = [1] + nums + [1]   # add boundary balloons
    n = len(nums)
    dp = [[0]*n for _ in range(n)]

    for length in range(2, n):           # window length
        for left in range(0, n - length):
            right = left + length
            for k in range(left + 1, right):   # k is the LAST balloon to burst
                dp[left][right] = max(
                    dp[left][right],
                    dp[left][k] + nums[left]*nums[k]*nums[right] + dp[k][right]
                )
    return dp[0][n-1]

# Strange Printer (minimum turns to print string)
def strange_printer(s):
    # Remove consecutive duplicates first
    s = ''.join(c for i, c in enumerate(s) if i == 0 or c != s[i-1])
    n = len(s)
    dp = [[0]*n for _ in range(n)]
    for i in range(n): dp[i][i] = 1   # 1 turn to print single char

    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = dp[i][j-1] + 1   # worst case: print s[j] separately
            for k in range(i, j):
                if s[k] == s[j]:         # can combine s[k] and s[j] in one turn
                    dp[i][j] = min(dp[i][j], dp[i][k] + (dp[k+1][j-1] if k+1<=j-1 else 0))
    return dp[0][n-1]
```

---

### More DP Patterns

```python
# Stock problems — state machine DP

# Best Time to Buy and Sell Stock with Cooldown
def max_profit_cooldown(prices):
    held = float('-inf')     # max profit while holding stock
    sold = 0                 # max profit on day we just sold
    rest = 0                 # max profit while resting (cooldown or never bought)
    for price in prices:
        prev_sold = sold
        sold = held + price                    # sell today
        held = max(held, rest - price)         # buy today (can only if we were resting)
        rest = max(rest, prev_sold)            # rest (or continue resting)
    return max(sold, rest)

# Best Time to Buy and Sell Stock with Transaction Fee
def max_profit_fee(prices, fee):
    cash = 0                 # max profit while NOT holding stock
    hold = -prices[0]        # max profit while holding stock
    for price in prices[1:]:
        cash = max(cash, hold + price - fee)   # sell
        hold = max(hold, cash - price)         # buy
    return cash

# Stock with at most k transactions
def max_profit_k_transactions(k, prices):
    n = len(prices)
    if not prices or k == 0: return 0
    if k >= n // 2:   # unlimited transactions
        return sum(max(0, prices[i+1]-prices[i]) for i in range(n-1))

    dp = [[0]*n for _ in range(k+1)]
    for t in range(1, k+1):
        max_so_far = -prices[0]
        for d in range(1, n):
            dp[t][d] = max(dp[t][d-1], prices[d] + max_so_far)
            max_so_far = max(max_so_far, dp[t-1][d] - prices[d])
    return dp[k][n-1]
```

---

## 50. Monotonic Deque (Advanced Window Patterns)

```python
from collections import deque

# ============================================
# SLIDING WINDOW MAXIMUM — O(n)
# ============================================
def sliding_window_max(nums, k):
    dq = deque()    # stores INDICES, front = index of current window max
    result = []
    for i in range(len(nums)):
        # Remove indices outside current window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        # Maintain decreasing order: remove smaller elements from back
        while dq and nums[dq[-1]] < nums[i]:
            dq.pop()
        dq.append(i)
        if i >= k - 1:
            result.append(nums[dq[0]])   # front is always current max
    return result

# ============================================
# SHORTEST SUBARRAY WITH SUM >= K — O(n)
# Uses monotonic deque on prefix sums
# ============================================
def shortest_subarray(nums, k):
    n = len(nums)
    prefix = [0] * (n + 1)
    for i in range(n): prefix[i+1] = prefix[i] + nums[i]

    result = float('inf')
    dq = deque()   # monotonically increasing prefix sums (indices)

    for i in range(n + 1):
        # Check if current prefix - front gives sum >= k
        while dq and prefix[i] - prefix[dq[0]] >= k:
            result = min(result, i - dq.popleft())
        # Maintain increasing prefix sums
        while dq and prefix[dq[-1]] >= prefix[i]:
            dq.pop()
        dq.append(i)

    return result if result != float('inf') else -1

# ============================================
# JUMP GAME VI — max score jumping k steps
# ============================================
def max_result(nums, k):
    n = len(nums)
    dp = [float('-inf')] * n
    dp[0] = nums[0]
    dq = deque([0])   # monotonically decreasing dp values

    for i in range(1, n):
        while dq and dq[0] < i - k:   # remove out of window
            dq.popleft()
        dp[i] = dp[dq[0]] + nums[i]   # best jump in window
        while dq and dp[dq[-1]] <= dp[i]:   # maintain decreasing
            dq.pop()
        dq.append(i)
    return dp[-1]
```

---

## 51. Advanced String Algorithms

### Z-Algorithm (Pattern Matching)

```python
# Z-array: Z[i] = length of longest substring starting at s[i] that matches s[0..]
# O(n) time — alternative to KMP

def z_function(s):
    n = len(s)
    z = [0] * n
    z[0] = n
    l = r = 0
    for i in range(1, n):
        if i < r:
            z[i] = min(r - i, z[i - l])
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
        if i + z[i] > r:
            l, r = i, i + z[i]
    return z

def z_search(text, pattern):
    combined = pattern + '#' + text   # '#' is a separator not in either string
    z = z_function(combined)
    m = len(pattern)
    return [i - m - 1 for i in range(m+1, len(combined)) if z[i] >= m]
```

---

### Manacher's Algorithm (Longest Palindromic Substring in O(n))

```python
def manacher(s):
    # Transform: "abc" → "#a#b#c#" (handles both odd/even palindromes uniformly)
    t = '#' + '#'.join(s) + '#'
    n = len(t)
    p = [0] * n     # p[i] = radius of longest palindrome centered at t[i]
    c = r = 0       # center and right boundary of rightmost palindrome

    for i in range(n):
        mirror = 2 * c - i
        if i < r:
            p[i] = min(r - i, p[mirror])
        # Expand around center i
        while i + p[i] + 1 < n and i - p[i] - 1 >= 0 and t[i+p[i]+1] == t[i-p[i]-1]:
            p[i] += 1
        if i + p[i] > r:
            c, r = i, i + p[i]

    # Find maximum palindrome
    max_len, center = max((v, i) for i, v in enumerate(p))
    start = (center - max_len) // 2   # convert back to original string index
    return s[start:start + max_len]

manacher("babad")    # "bab"
manacher("cbbd")     # "bb"
```

---

## 52. Python Memory Model & Performance Tips

```python
import sys
import timeit

# ============================================
# MEMORY
# ============================================
# Object sizes (approximate)
sys.getsizeof(1)          # 28 bytes (int)
sys.getsizeof(1.0)        # 24 bytes (float)
sys.getsizeof("")         # 49 bytes (empty str)
sys.getsizeof([])         # 56 bytes (empty list)
sys.getsizeof({})         # 64 bytes (empty dict)
sys.getsizeof(set())      # 216 bytes (empty set — larger!)
sys.getsizeof(())         # 40 bytes (empty tuple — smaller than list!)

# Small integer caching: Python caches -5 to 256
a = 256; b = 256; a is b    # True  (same object)
a = 257; b = 257; a is b    # False (different objects)

# String interning: short strings are interned (cached)
a = "hello"; b = "hello"; a is b   # True
a = "hello world"; b = "hello world"; a is b  # May be False!
# Use == for value comparison, is only for identity

# ============================================
# PERFORMANCE TRICKS
# ============================================

# 1. List comprehension > loop append (faster bytecode)
# Slow:
result = []
for i in range(1000): result.append(i**2)
# Fast:
result = [i**2 for i in range(1000)]

# 2. join > repeated string concatenation
# Slow: (creates new string object each iteration — O(n²))
s = ""
for w in words: s += w
# Fast: O(n) — build list first, join at end
s = "".join(words)

# 3. set/dict lookup > list lookup (O(1) vs O(n))
items_list = list(range(1000))
items_set  = set(range(1000))
999 in items_list   # O(n)
999 in items_set    # O(1)

# 4. local variable faster than global (fewer LOAD_GLOBAL bytecodes)
def fast_sum(n):
    local_range = range    # cache global as local
    total = 0
    for i in local_range(n):
        total += i
    return total

# 5. Avoid repeated attribute lookup
import math
sqrt = math.sqrt   # cache the method lookup
result = [sqrt(x) for x in range(1000)]

# 6. Use enumerate instead of range(len())
# Slow:
for i in range(len(lst)): print(i, lst[i])
# Fast:
for i, val in enumerate(lst): print(i, val)

# 7. zip instead of index-based iteration
for a, b in zip(list1, list2): ...

# 8. Deque for queue (O(1) popleft vs O(n) pop(0) for list)
from collections import deque
q = deque()

# 9. __slots__ for memory-efficient objects (covered above)

# 10. Generator expressions for large datasets (O(1) memory)
total = sum(x**2 for x in range(10**8))   # doesn't build list in memory

# ============================================
# PROFILING
# ============================================
import cProfile
cProfile.run('your_function()')

import timeit
timeit.timeit('sorted([3,1,2])', number=100000)

# line_profiler (install separately):
# @profile decorator, then: kernprof -l -v script.py
```

---

## 🏆 FAANG Interview Patterns Cheat Sheet

### Recognize the Pattern Instantly

| Problem Keywords | Pattern | Data Structure |
| :--- | :--- | :--- |
| "sorted array, find pair" | Two Pointers | Array |
| "subarray/substring of size k" | Sliding Window | Array + HashMap |
| "longest/shortest subarray with condition" | Sliding Window (variable) | Array + HashMap |
| "range sum query, immutable" | Prefix Sum | Array |
| "shortest path, unweighted" | BFS | Queue + Graph |
| "path exists, number of islands" | BFS/DFS | Graph/Grid |
| "all permutations/combinations/subsets" | Backtracking | Recursion |
| "sorted in parts, find target" | Binary Search | Sorted Array |
| "min/max of sliding window" | Monotonic Deque | Deque |
| "next greater/smaller element" | Monotonic Stack | Stack |
| "largest rectangle, histogram" | Monotonic Stack | Stack |
| "kth largest/smallest" | Heap (size k) or QuickSelect | Heap |
| "merge k sorted lists/streams" | Heap | Min-Heap |
| "find median from stream" | Two Heaps | Heap |
| "connected components, cycle detect" | Union-Find | DSU |
| "minimum spanning tree" | Kruskal's (sort+DSU) / Prim's (heap) | Heap+DSU |
| "task scheduling with dependencies" | Topological Sort | Queue + Graph |
| "shortest path with weights" | Dijkstra | Heap + Graph |
| "shortest path with negative weights" | Bellman-Ford | Graph |
| "all-pairs shortest path" | Floyd-Warshall | Matrix |
| "prefix/word matching, autocomplete" | Trie | Trie |
| "LCS, LIS, knapsack, grid paths" | Dynamic Programming | Array/Dict |
| "overlapping subproblems in string" | DP (interval/memo) | 2D Array |
| "majority element" | Boyer-Moore Voting | O(1) space |
| "sort 0s,1s,2s" | Dutch National Flag | Two Pointers |
| "count inversions, rank queries" | Fenwick Tree | BIT |
| "range query + point update" | Segment Tree | Tree Array |
| "range query on immutable array" | Sparse Table | 2D Array |
| "string pattern search" | KMP / Rabin-Karp / Z-algorithm | String |
| "count numbers satisfying digit property" | Digit DP | Memoization |
| "visit all subsets optimally" | Bitmask DP | Bitmask |

---

### Python-Specific Interview Tips

```python
# 1. Use float('inf') and float('-inf') as sentinels
# 2. Use collections.defaultdict(list) for graphs
# 3. Use collections.Counter for frequency maps
# 4. Use heapq for priority queues (only min-heap; negate for max-heap)
# 5. Use collections.deque for BFS queues and sliding window
# 6. sorted() with key= for custom sorting
# 7. list[::-1] for reversal
# 8. zip(*matrix) for transposing a 2D list
# 9. any() / all() for short-circuit evaluation on iterables
# 10. enumerate() instead of range(len())
# 11. @functools.lru_cache(maxsize=None) for memoization
# 12. Use tuple as dict keys (tuples are hashable, lists are not)
# 13. set for O(1) membership testing
# 14. bisect.bisect_left / bisect_right for binary search on sorted list
# 15. Python integers don't overflow — no need for long in Java-style tricks

# Transpose a matrix
matrix = [[1,2,3],[4,5,6],[7,8,9]]
transposed = list(map(list, zip(*matrix)))

# Flatten nested list
nested = [[1,2],[3,4],[5,6]]
flat = [x for row in nested for x in row]
# or: list(itertools.chain.from_iterable(nested))

# Sort with multiple keys
data = [(1,'b'),(2,'a'),(1,'a')]
sorted(data, key=lambda x: (x[0], x[1]))  # [(1,'a'),(1,'b'),(2,'a')]

# Group into chunks of size k
def chunks(lst, k):
    return [lst[i:i+k] for i in range(0, len(lst), k)]

# Rotate array right by k positions
def rotate(nums, k):
    k %= len(nums)
    nums[:] = nums[-k:] + nums[:-k]

# Check if two strings are anagrams
def is_anagram(a, b): return sorted(a) == sorted(b)
# Or: Counter(a) == Counter(b)  (O(n) but with more constant factors)

# Generate all substrings
s = "abc"
substrings = [s[i:j] for i in range(len(s)) for j in range(i+1, len(s)+1)]
# ['a', 'ab', 'abc', 'b', 'bc', 'c']

# Binary search template (always works)
def binary_search(lo, hi, condition):
    while lo < hi:
        mid = (lo + hi) // 2
        if condition(mid):
            hi = mid          # or lo = mid+1 for upper bound
        else:
            lo = mid + 1
    return lo
```
