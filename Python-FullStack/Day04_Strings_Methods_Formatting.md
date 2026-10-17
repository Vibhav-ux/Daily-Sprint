# 🐍 Python Full Stack — Day 04: Strings & Text Processing
### Phase 0: Foundations | Slicing, Methods, Formatting & Regular Expressions

---

## 📌 1. String Immutability & Memory

In Python, strings are **immutable sequence of Unicode characters**.
Any operation that modifies a string creates a **new** string in memory.

```python
s = "hello"
# s[0] = "H"  # ❌ TypeError: 'str' object does not support item assignment
s = "H" + s[1:] # ✅ Creates a brand new string "Hello"
```

---

## 📌 2. Slicing Syntax: `[start:stop:step]`

```python
text = "DailySprint2026"

print(text[0:5])    # 'Daily' (index 0 to 4)
print(text[:5])     # 'Daily' (default start 0)
print(text[5:])     # 'Sprint2026' (from 5 to end)
print(text[-4:])    # '2026' (last 4 characters)
print(text[::2])    # 'DiySrn06' (every 2nd character)
print(text[::-1])   # '6202tnirpSyliaD' (reverse string!)
```

---

## 📌 3. Essential String Methods

### Searching & Validation
* `s.startswith(prefix)` / `s.endswith(suffix)` $\rightarrow$ `bool`
* `s.find(sub)` $\rightarrow$ Index or `-1` if not found (doesn't throw)
* `s.index(sub)` $\rightarrow$ Index or raises `ValueError`
* `s.isalpha()`, `s.isdigit()`, `s.isalnum()`

### Cleaning & Transforming
* `s.strip()` / `s.lstrip()` / `s.rstrip()`: Removes leading/trailing whitespace.
* `s.lower()`, `s.upper()`, `s.title()`, `s.capitalize()`
* `s.replace(old, new, [count])`

### Splitting & Joining (Crucial for Interviews)
```python
# Split into list
csv_line = "apple,banana,orange,grape"
fruits = csv_line.split(",")
# ['apple', 'banana', 'orange', 'grape']

# Join list back to string
reconstructed = " | ".join(fruits)
# 'apple | banana | orange | grape'
```

---

## 📌 4. Modern String Formatting (f-strings)

Introduced in Python 3.6, f-strings evaluate expressions at runtime and are faster than `%` or `.format()`.

```python
name = "Vibhav"
score = 98.4567
rank = 1

# Formatting numbers
print(f"Candidate: {name:<10} | Rank: {rank:03d} | Score: {score:.2f}%")
# Output: Candidate: Vibhav     | Rank: 001 | Score: 98.46%

# Expressions inside f-strings
print(f"Calculated: {10 * 5 = }")
# Output: Calculated: 10 * 5 = 50
```

---

## 💡 Practical Challenge: Palindrome Checker (Ignoring Punctuation & Case)
```python
def is_palindrome(s: str) -> bool:
    # Filter only alphanumeric characters and lowercase
    cleaned = "".join(ch.lower() for ch in s if ch.isalnum())
    return cleaned == cleaned[::-1]

print(is_palindrome("A man, a plan, a canal: Panama")) # True
```
