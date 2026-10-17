# 🐍 Python Full Stack — Day 03: Data Structures Mastery
### Phase 0: Foundations | Lists, Tuples, Sets & Dictionaries

---

## 📌 Executive Summary

Python provides four built-in collection data structures. Understanding when to use which is the hallmark of an effective developer:

| Structure | Syntax | Mutable? | Ordered? | Duplicates? | Typical Use Case |
|---|---|---|---|---|---|
| **List** | `[1, 2, 3]` | ✅ Yes | ✅ Yes | ✅ Allowed | Dynamic collections, stacks, queues |
| **Tuple** | `(1, 2, 3)` | ❌ No | ✅ Yes | ✅ Allowed | Coordinates, DB records, constants |
| **Set** | `{1, 2, 3}` | ✅ Yes | ❌ No | ❌ Unique only | Membership test $O(1)$, removing duplicates |
| **Dictionary**| `{"a": 1}` | ✅ Yes | ✅ Yes (3.7+) | Keys unique | Key-value lookups $O(1)$, caching, JSON |

---

## 1. Lists (`list`) Deep Dive

### Creation & Comprehensions
```python
# List creation
nums = [10, 20, 30, 40, 50]
matrix = [[1, 2], [3, 4], [5, 6]]

# List Comprehension (Fast, idiomatic)
squares = [x**2 for x in range(1, 11) if x % 2 == 0]
# [4, 16, 36, 64, 100]
```

### Essential Operations & Complexities
* `append(val)`: $O(1)$ amortized — Adds to end.
* `pop()`: $O(1)$ — Removes from end.
* `pop(0)` or `insert(0, val)`: $O(N)$ — Shifts all elements!
* `extend(iterable)`: $O(K)$ — In-place concatenation.
* `sort()`: $O(N \log N)$ (Timsort) — In-place sort.

---

## 2. Tuples (`tuple`)

### Why use Tuples over Lists?
1. **Immutability guarantee**: Prevents accidental data modification.
2. **Hashable**: Tuples can be dictionary keys or set members (if their items are immutable). Lists cannot!
3. **Memory efficiency**: Tuples require less memory overhead than lists.

```python
# Tuple packing & unpacking
point = (10, 20, 30)
x, y, z = point  # unpacking: x=10, y=20, z=30

# Single element tuple MUST have a trailing comma
single = (42,)   # type: tuple
not_a_tuple = (42) # type: int!
```

---

## 3. Sets (`set`)

Sets are implemented under the hood as Hash Tables.

### Key Operations
* `item in my_set`: **$O(1)$ average time** (vs $O(N)$ in a list!)
* Math set operations:
```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)  # Union: {1, 2, 3, 4, 5, 6}
print(a & b)  # Intersection: {3, 4}
print(a - b)  # Difference: {1, 2}
print(a ^ b)  # Symmetric Difference: {1, 2, 5, 6}
```

---

## 4. Dictionaries (`dict`)

The backbone of Python. Hash table mapping hashable keys to values.

```python
# Dictionary methods
student = {
    "name": "Vibhav",
    "track": "Full Stack & DSA",
    "streak": 3
}

# Safe access with .get() (never crashes with KeyError)
score = student.get("score", 0)  # returns default 0

# Iteration
for key, value in student.items():
    print(f"{key} -> {value}")

# Dictionary Comprehension
squared_dict = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

---

## 💡 Practical Challenge
Write a function `char_frequency(s: str) -> dict` that counts occurrences of each character in a string using a dictionary.
```python
def char_frequency(s: str) -> dict:
    freq = {}
    for ch in s:
        freq[ch] = freq.get(ch, 0) + 1
    return freq
```
