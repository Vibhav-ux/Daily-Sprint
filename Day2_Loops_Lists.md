# 🐍 Day 2 — Loops, Lists & DSA
### Phase 0 | October 2, 2026 | Vibhav Shukla

---

> 📌 How to use: Read section → open VS Code → type code yourself → run → next section.
> File: day2_loops.py  |  day2_lists.py  |  day2_dsa.py

---

# PART 1 — for Loop

## 🤔 What is a Loop?

Imagine you have to write "I will study daily" 10 times on a notebook.
Without loop → you write it 10 times manually.
With loop    → you tell Python "do this 10 times" and it does it.

---

## range() — The Loop Counter

range() generates a sequence of numbers for the loop to run through.

```python
range(5)          # 0, 1, 2, 3, 4         (starts at 0, stops BEFORE 5)
range(1, 6)       # 1, 2, 3, 4, 5         (starts at 1, stops BEFORE 6)
range(0, 10, 2)   # 0, 2, 4, 6, 8         (start, stop, step)
range(10, 0, -1)  # 10, 9, 8, 7...1       (countdown)
```

---

## Basic for Loop

```python
# Print 0 to 4
for i in range(5):
    print(i)
# Output: 0  1  2  3  4

# Print 1 to 5
for i in range(1, 6):
    print(i)
# Output: 1  2  3  4  5

# Print even numbers 0 to 8
for i in range(0, 10, 2):
    print(i)
# Output: 0  2  4  6  8

# Countdown from 5 to 1
for i in range(5, 0, -1):
    print(i)
# Output: 5  4  3  2  1
```

---

## Loop Over a List

```python
names = ["Vibhav", "Yuvraj", "Swapnil", "Vaibhav"]

# Loop through values directly
for name in names:
    print(name)

# Loop with index using range
for i in range(len(names)):
    print(f"Position {i}: {names[i]}")
```

---

## enumerate() — Index + Value Together

Instead of using range(len(list)), enumerate gives both index and value at once.

```python
names = ["Vibhav", "Yuvraj", "Swapnil", "Vaibhav"]

# Without enumerate (old way)
for i in range(len(names)):
    print(i, names[i])

# With enumerate (clean way — same result)
for i, name in enumerate(names):
    print(i, name)

# Start index from 1 instead of 0
for i, name in enumerate(names, start=1):
    print(f"{i}. {name}")
# Output:
# 1. Vibhav
# 2. Yuvraj
# 3. Swapnil
# 4. Vaibhav
```

Rule: use enumerate() whenever you need BOTH the index AND the value.

---

## ✏️ EXERCISES — for Loop

```python
# Exercise 1: Multiplication Table
# Ask user for a number. Print its table from 1 to 10.
# Output example for 5:
# 5 x 1 = 5
# 5 x 2 = 10
# ...
# 5 x 10 = 50

# Exercise 2: Sum of 1 to N
# Ask user to enter N. Print sum of all numbers from 1 to N.
# Example: N=5 → 1+2+3+4+5 = 15

# Exercise 3: Print with enumerate
fruits = ["mango", "banana", "apple", "grape"]
# Print like this (index starts at 1):
# 1. mango
# 2. banana
# 3. apple
# 4. grape

# Exercise 4: Squares
# Print squares of numbers 1 to 10
# 1  → 1
# 2  → 4
# 3  → 9 ... etc
```

---
---

# PART 2 — while Loop

## 🤔 What is while?

for loop → runs a FIXED number of times (you know how many)
while loop → runs UNTIL a condition becomes False (you don't know how many times)

```python
# Syntax
while condition:
    # runs as long as condition is True
    # MUST change something so condition eventually becomes False!
```

---

## Examples

```python
# Example 1 — basic while
count = 0
while count < 5:
    print(count)
    count += 1       # count = count + 1
# Output: 0  1  2  3  4

# Example 2 — keep asking until correct input
password = ""
while password != "python123":
    password = input("Enter password: ")
print("Access granted! ✅")

# Example 3 — sum until user enters 0
total = 0
while True:
    num = int(input("Enter number (0 to stop): "))
    if num == 0:
        break          # exits the loop
    total += num
print(f"Total: {total}")
```

---

## ⚠️ Infinite Loop — Danger!

If condition NEVER becomes False, the loop runs forever and freezes your program!

```python
# WRONG — infinite loop! (count never changes)
count = 0
while count < 5:
    print(count)
    # forgot count += 1 !

# Fix: always make sure something in the loop changes the condition
count = 0
while count < 5:
    print(count)
    count += 1    # ✅ this eventually makes count >= 5
```

Press Ctrl + C to stop an infinite loop in terminal.

---
---

# PART 3 — break, continue, pass

## break — Stop the loop immediately

```python
# Find first number divisible by 7
for i in range(1, 100):
    if i % 7 == 0:
        print(f"First number divisible by 7: {i}")
        break      # stop as soon as we find it
# Output: First number divisible by 7: 7
```

## continue — Skip this round, go to next

```python
# Print all numbers 1-10 EXCEPT multiples of 3
for i in range(1, 11):
    if i % 3 == 0:
        continue      # skip 3, 6, 9
    print(i)
# Output: 1  2  4  5  7  8  10
```

## pass — Do nothing (placeholder)

```python
# pass is used when Python needs a code block but you have nothing yet
for i in range(5):
    pass    # will add code here later — no error
```

---

## ✏️ EXERCISES — while + break + continue

```python
# Exercise 1: Number guessing game
# Generate a secret number (use: secret = 42)
# Keep asking user to guess until they get it right
# Print "Too high!", "Too low!", or "Correct! 🎉"

# Exercise 2: Print 1 to 20, skip multiples of 4
# Use continue
# Output: 1 2 3 5 6 7 9 10 11 13 14 15 17 18 19

# Exercise 3: Keep taking numbers from user until they enter a negative number
# At the end, print: count of numbers entered, sum, average
```

---
---

# PART 4 — Lists (Deep Dive)

## 🤔 What is a List? (Quick Recap)

A list is an ordered collection of items.

```python
marks   = [85, 90, 78, 92, 88]
names   = ["Vibhav", "Yuvraj", "Swapnil"]
mixed   = [1, "hello", 3.14, True]   # can mix types
empty   = []                          # empty list
```

---

## Accessing Elements (Indexing)

```python
fruits = ["mango", "banana", "apple", "grape", "orange"]
#index:     0         1         2        3        4
#          -5        -4        -3       -2       -1   (negative index from end)

print(fruits[0])     # mango   ← first
print(fruits[2])     # apple
print(fruits[-1])    # orange  ← last
print(fruits[-2])    # grape   ← second from last
```

---

## Slicing — Get a Portion of List

Syntax: list[start : stop : step]
stop is NOT included.

```python
nums = [10, 20, 30, 40, 50, 60, 70]

print(nums[1:4])      # [20, 30, 40]     index 1 to 3
print(nums[:3])       # [10, 20, 30]     from start to index 2
print(nums[3:])       # [40, 50, 60, 70] from index 3 to end
print(nums[::2])      # [10, 30, 50, 70] every 2nd element
print(nums[::-1])     # [70, 60, 50, 40, 30, 20, 10] REVERSED!
```

---

## Modifying Lists — Key Methods

```python
fruits = ["mango", "banana", "apple"]

# append() — add to END
fruits.append("grape")
print(fruits)    # ['mango', 'banana', 'apple', 'grape']

# insert() — add at specific position
fruits.insert(1, "kiwi")     # insert at index 1
print(fruits)    # ['mango', 'kiwi', 'banana', 'apple', 'grape']

# remove() — remove by VALUE (first occurrence)
fruits.remove("banana")
print(fruits)    # ['mango', 'kiwi', 'apple', 'grape']

# pop() — remove by INDEX (default: last element)
fruits.pop()          # removes last → 'grape'
fruits.pop(0)         # removes index 0 → 'mango'
print(fruits)    # ['kiwi', 'apple']

# clear() — remove everything
fruits.clear()
print(fruits)    # []
```

---

## Searching & Info

```python
nums = [5, 2, 8, 2, 9, 2, 1]

print(len(nums))         # 7        ← how many items
print(sum(nums))         # 29       ← total
print(max(nums))         # 9        ← biggest
print(min(nums))         # 1        ← smallest
print(nums.count(2))     # 3        ← how many times 2 appears
print(nums.index(9))     # 4        ← index of first occurrence of 9
print(2 in nums)         # True     ← is 2 in the list?
print(10 in nums)        # False
```

---

## Sorting

```python
nums = [5, 2, 8, 1, 9, 3]

# sort() — sorts the ORIGINAL list (modifies it)
nums.sort()
print(nums)              # [1, 2, 3, 5, 8, 9]

# sort in reverse (descending)
nums.sort(reverse=True)
print(nums)              # [9, 8, 5, 3, 2, 1]

# sorted() — returns NEW sorted list (original unchanged)
original = [5, 2, 8, 1]
new_sorted = sorted(original)
print(original)          # [5, 2, 8, 1]  ← unchanged
print(new_sorted)        # [1, 2, 5, 8]  ← new sorted copy
```

---

## List Comprehension — One-Line List Creation

This is Python's superpower. Makes creating lists very fast.

```python
# Without list comprehension
squares = []
for i in range(1, 6):
    squares.append(i ** 2)
print(squares)    # [1, 4, 9, 16, 25]

# WITH list comprehension (same thing, one line!)
squares = [i ** 2 for i in range(1, 6)]
print(squares)    # [1, 4, 9, 16, 25]

# With condition — only even squares
even_squares = [i ** 2 for i in range(1, 11) if i % 2 == 0]
print(even_squares)    # [4, 16, 36, 64, 100]

# Real example — get all names with length > 5
names = ["Vibhav", "Yash", "Swapnil", "Vaibhav", "Yuvraj"]
long_names = [name for name in names if len(name) > 5]
print(long_names)    # ['Vibhav', 'Swapnil', 'Vaibhav', 'Yuvraj']
```

---

## ✏️ EXERCISES — Lists

```python
# Exercise 1:
# Create a list of 5 of your favourite movies.
# a) Print the first and last movie
# b) Add a new movie at the end
# c) Remove the second movie
# d) Sort the list alphabetically and print

# Exercise 2:
# Ask user to enter 5 numbers one by one (use a loop + append).
# After all are entered, print:
# - The list
# - Sum, Max, Min
# - List in reverse order
# - Only numbers greater than the average

# Exercise 3: List comprehension
# Create a list of all numbers from 1 to 50 that are divisible by 3 or 5
# Expected: [3, 5, 6, 9, 10, 12, 15, 18, 20, ...]

# Exercise 4:
# Given: marks = [45, 78, 92, 56, 88, 34, 76, 65, 90, 23]
# Using list comprehension, create two new lists:
# - passed: marks >= 40
# - failed: marks < 40
# Print both
```

---
---

# PART 5 — DSA: Dictionaries + Today's Problems

## Python Dictionary (Full)

```python
# Create
student = {
    "name": "Vibhav",
    "age": 20,
    "cgpa": 8.5,
    "is_chair": True
}

# Access
print(student["name"])       # Vibhav
print(student.get("age"))    # 20
print(student.get("phone"))  # None (no crash, unlike student["phone"])

# Add / Update
student["college"] = "AKTU"       # add new key
student["age"] = 21               # update existing key

# Delete
del student["is_chair"]           # remove a key
popped = student.pop("cgpa")      # remove + return value

# Check if key exists
print("name" in student)          # True
print("phone" in student)         # False

# Loop through dictionary
for key in student:
    print(key, "→", student[key])

# Loop through key-value pairs
for key, value in student.items():
    print(f"{key}: {value}")

# All keys and values
print(student.keys())             # dict_keys(['name', 'age', 'college'])
print(student.values())           # dict_values(['Vibhav', 21, 'AKTU'])
```

---

## 🔴 DSA Problem 1 — Contains Duplicate (Easy)

Link: https://leetcode.com/problems/contains-duplicate/

Problem:
Given a list of numbers, return True if any number appears MORE than once.
Return False if all numbers are unique.

Example:
Input:  [1, 2, 3, 1]   → True  (1 appears twice)
Input:  [1, 2, 3, 4]   → False (all unique)
Input:  [1, 1, 1, 3]   → True  (1 appears 3 times)


### Solution — Python (using a Set)

A Set in Python = a collection with NO duplicates.
If adding a number to set and it is already there → duplicate found!

```python
def containsDuplicate(nums):
    seen = set()                # empty set

    for num in nums:
        if num in seen:         # already saw this number?
            return True         # yes → duplicate!
        seen.add(num)           # no → add it to seen

    return False                # no duplicates found

# Test
print(containsDuplicate([1, 2, 3, 1]))    # True
print(containsDuplicate([1, 2, 3, 4]))    # False
print(containsDuplicate([1, 1, 1, 3]))    # True
```

One-liner (bonus):
```python
def containsDuplicate(nums):
    return len(nums) != len(set(nums))
# set() removes duplicates. If lengths differ → duplicates existed!
```

### Solution — Java (using HashSet)

```java
import java.util.HashSet;

public boolean containsDuplicate(int[] nums) {
    HashSet<Integer> seen = new HashSet<>();
    // HashSet = Java's version of Python's set (no duplicates)

    for (int num : nums) {
        if (seen.contains(num)) {    // already saw this?
            return true;             // duplicate found!
        }
        seen.add(num);               // add to seen
    }

    return false;
}
```

---

## 🔴 DSA Problem 2 — Valid Anagram (Easy)

Link: https://leetcode.com/problems/valid-anagram/

Problem:
Two words are anagrams if they use the same letters same number of times.
"anagram" and "nagaram" → True  (same letters)
"rat"     and "car"     → False (different letters)

### Solution — Python

```python
def isAnagram(s, t):
    # If different lengths, can't be anagram
    if len(s) != len(t):
        return False

    # Count letters in each word using dictionary
    count = {}

    for char in s:
        count[char] = count.get(char, 0) + 1   # add 1 for each letter in s

    for char in t:
        count[char] = count.get(char, 0) - 1   # subtract 1 for each letter in t

    # If all counts are 0, it's an anagram
    for val in count.values():
        if val != 0:
            return False

    return True

# Test
print(isAnagram("anagram", "nagaram"))   # True
print(isAnagram("rat", "car"))           # False
```

### Solution — Java

```java
import java.util.HashMap;

public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;

    HashMap<Character, Integer> count = new HashMap<>();

    for (char c : s.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) + 1);
    }

    for (char c : t.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) - 1);
    }

    for (int val : count.values()) {
        if (val != 0) return false;
    }

    return true;
}
```

---

## Quick Reference — Java HashMap vs Python Dict

| What you want          | Python                      | Java                              |
|------------------------|-----------------------------|-----------------------------------|
| Create empty           | d = {}                      | HashMap<K,V> d = new HashMap<>()  |
| Add/Update             | d[key] = value              | d.put(key, value)                 |
| Get value              | d[key] or d.get(key)        | d.get(key)                        |
| Get with default       | d.get(key, 0)               | d.getOrDefault(key, 0)            |
| Check key exists       | key in d                    | d.containsKey(key)                |
| Remove key             | del d[key] or d.pop(key)    | d.remove(key)                     |
| Loop key-value pairs   | for k,v in d.items()        | for (Map.Entry e : d.entrySet())  |
| All values             | d.values()                  | d.values()                        |

---

## ✅ DAY 2 COMPLETE CHECKLIST

Loops:
[ ] for loop with range() — understood
[ ] for loop over list — working
[ ] enumerate() — can use it
[ ] while loop — understood
[ ] break and continue — can use them
[ ] Multiplication table exercise — done
[ ] Number guessing game — done

Lists:
[ ] Indexing and slicing — understood
[ ] append, remove, pop, sort — can use all
[ ] List comprehension — can write one
[ ] All 4 list exercises — done

DSA:
[ ] Python Set / Java HashSet — understood
[ ] Contains Duplicate — solved in Python ✅
[ ] Contains Duplicate — solved in Java ✅
[ ] Valid Anagram — solved in Python ✅
[ ] Valid Anagram — solved in Java ✅

---

## 🔜 Day 3 Will Cover:
- Tuples and Sets (Python data structures)
- Strings deep dive (split, join, strip, replace, find)
- String formatting tricks
- Functions (def, return, parameters) — full lesson
- DSA: Best Time to Buy & Sell Stock, Maximum Subarray

---

Stuck anywhere? Say: "I am stuck on [topic]" and I will explain again differently! 🎯
