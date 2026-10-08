# 🐍 Day 3 — Tuples, Sets, Strings & Functions
### Phase 0 | October 8, 2026 | Vibhav Shukla

---

> 📌 How to use: Read section → VS Code → type code → run → next section.
> Files to create: day3_strings.py | day3_functions.py | day3_dsa.py

---

# PART 1 — Tuples

## 🤔 What is a Tuple?

A tuple is exactly like a list — BUT it cannot be changed after creation.
List   → [ ]  — can add, remove, modify  (mutable)
Tuple  → ( )  — frozen, read-only        (immutable)

When to use a tuple?
→ When data should NOT change: coordinates, RGB colors, days of week, DB records

---

```python
# Create a tuple
coordinates = (19.0760, 72.8777)    # Mumbai lat, lon
rgb_red     = (255, 0, 0)
days        = ("Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun")
single      = (42,)                 # single item tuple — needs trailing comma!
empty       = ()

# Access — same as list (indexing, slicing)
print(coordinates[0])    # 19.076
print(days[-1])          # Sun
print(days[1:4])         # ('Tue', 'Wed', 'Thu')

# Length, count, index
print(len(days))                  # 7
print(days.count("Mon"))          # 1
print(days.index("Fri"))          # 4

# Loop through tuple
for day in days:
    print(day)
```

---

## Tuple Unpacking — Super Useful!

```python
# Assign tuple values to variables in one line
point = (10, 20)
x, y = point           # x=10, y=20
print(x, y)

# Works with any size
name, age, city = ("Vibhav", 20, "Lucknow")
print(name)    # Vibhav
print(city)    # Lucknow

# Swap two variables using tuple (Python trick!)
a = 5
b = 10
a, b = b, a    # swap!
print(a, b)    # 10  5
```

---

## Why Tuple Cannot Be Changed

```python
t = (1, 2, 3)
t[0] = 99      # ERROR! TypeError: 'tuple' object does not support item assignment

# This is intentional — it protects your data from accidental changes
```

---

## ✏️ Tuple Exercises

```python
# Exercise 1:
# Create a tuple of 5 Indian cities.
# a) Print the first and last city
# b) Print cities at index 1, 2, 3 using slicing
# c) Check if "Mumbai" is in the tuple

# Exercise 2: Tuple unpacking
# Create a tuple: ("Vibhav Shukla", "AKTU", 8.5, "CSE")
# Unpack it into 4 variables: name, college, cgpa, branch
# Print: "Hi, I am [name] from [college], [branch] with CGPA [cgpa]"

# Exercise 3:
# Swap these values using tuple unpacking:
first_rank  = "Yuvraj"
second_rank = "Vibhav"
# After swap: first_rank = "Vibhav", second_rank = "Yuvraj"
```

---
---

# PART 2 — Sets

## 🤔 What is a Set?

A set is like a list BUT:
1. No duplicate values — automatically removes them
2. No order — items have no index
3. Very fast to check if something is IN it

Real life example: a bag of unique marble colors — you cannot have two red marbles.

---

```python
# Create a set
colors  = {"red", "blue", "green", "red", "blue"}
print(colors)    # {'red', 'blue', 'green'}  ← duplicates removed!

nums = {1, 2, 3, 4, 5}
empty_set = set()     # NOT {}  — that creates an empty DICT!

# Add / Remove
colors.add("yellow")
colors.add("red")       # already exists → nothing happens
colors.remove("blue")   # removes — ERROR if not found
colors.discard("pink")  # removes — NO error if not found

# Check membership — sets are VERY fast at this
print("red" in colors)      # True
print("purple" in colors)   # False

# Length
print(len(colors))
```

---

## Set Operations (Math-style)

```python
A = {1, 2, 3, 4, 5}
B = {4, 5, 6, 7, 8}

# Union — everything in A OR B (combined, no duplicates)
print(A | B)            # {1, 2, 3, 4, 5, 6, 7, 8}
print(A.union(B))       # same

# Intersection — only what is in BOTH A AND B
print(A & B)            # {4, 5}
print(A.intersection(B))

# Difference — in A but NOT in B
print(A - B)            # {1, 2, 3}
print(A.difference(B))

# Symmetric difference — in A or B but NOT BOTH
print(A ^ B)            # {1, 2, 3, 6, 7, 8}
```

---

## Most Common Use Case — Remove Duplicates from a List

```python
marks = [85, 90, 85, 78, 90, 92, 78, 85]
print(marks)                    # [85, 90, 85, 78, 90, 92, 78, 85]

unique_marks = list(set(marks)) # convert to set (removes dups) then back to list
print(unique_marks)             # [85, 90, 78, 92]  (order may vary)
```

---

## ✏️ Set Exercises

```python
# Exercise 1:
# You have two lists of students who attended two workshops.
workshop1 = ["Vibhav", "Yuvraj", "Swapnil", "Vaibhav", "Rohan"]
workshop2 = ["Swapnil", "Rohan", "Ankit", "Priya", "Vibhav"]
# Find:
# a) Students who attended BOTH workshops
# b) Students who attended ONLY workshop1
# c) ALL unique students across both workshops
# d) Students who attended exactly ONE workshop (not both)

# Exercise 2:
# Given this list with duplicates:
nums = [4, 7, 2, 4, 1, 7, 9, 2, 4, 3, 1]
# a) Print only unique numbers
# b) Print count of how many duplicates were removed
```

---
---

# PART 3 — Strings (Deep Dive)

## 🤔 Strings Recap

A string is a sequence of characters inside quotes.
String = "text" or 'text' or """multi
line text"""

---

## Indexing & Slicing (same as lists!)

```python
name = "Vibhav Shukla"
#       0123456789...

print(name[0])       # V
print(name[-1])      # a
print(name[0:6])     # Vibhav
print(name[7:])      # Shukla
print(name[:6])      # Vibhav
print(name[::-1])    # alkhS vahbiV  (reversed!)
print(len(name))     # 13
```

---

## Most Important String Methods

```python
s = "  Hello, World! I love Python.  "

# Case
print(s.upper())           # "  HELLO, WORLD! I LOVE PYTHON.  "
print(s.lower())           # "  hello, world! i love python.  "
print(s.title())           # "  Hello, World! I Love Python.  "

# Strip (remove spaces from edges)
print(s.strip())           # "Hello, World! I love Python."
print(s.lstrip())          # "Hello, World! I love Python.  "  (left only)
print(s.rstrip())          # "  Hello, World! I love Python."  (right only)

# Replace
print(s.replace("Python", "Coding"))   # replaces all occurrences

# Find / Contains
print("World" in s)          # True
print(s.find("World"))       # 9   (index where it starts, -1 if not found)
print(s.count("l"))          # 3   (how many times "l" appears)
print(s.startswith("  He"))  # True
print(s.endswith("n.  "))    # True

# Split — turns string into a list!
sentence = "Python is awesome and powerful"
words = sentence.split()       # splits by space (default)
print(words)    # ['Python', 'is', 'awesome', 'and', 'powerful']

csv_line = "Vibhav,20,AKTU,8.5"
parts = csv_line.split(",")    # split by comma
print(parts)    # ['Vibhav', '20', 'AKTU', '8.5']

# Join — turns a list back into a string!
words = ["Python", "is", "awesome"]
sentence = " ".join(words)        # joins with space
print(sentence)   # "Python is awesome"

path_parts = ["Users", "Vibhav", "Desktop", "LEARN"]
path = "\\".join(path_parts)       # joins with backslash
print(path)    # Users\Vibhav\Desktop\LEARN
```

---

## Check String Content

```python
s1 = "hello123"
s2 = "hello"
s3 = "12345"
s4 = "  "

print(s2.isalpha())    # True  — only letters
print(s3.isdigit())    # True  — only digits
print(s1.isalnum())    # True  — letters + digits
print(s4.isspace())    # True  — only spaces
print(s2.islower())    # True  — all lowercase
print(s2.isupper())    # False
```

---

## String Formatting

```python
name = "Vibhav"
cgpa = 8.567

# f-string with formatting
print(f"CGPA: {cgpa:.2f}")          # CGPA: 8.57  (2 decimal places)
print(f"{'Name':<10} {'Score':>5}")  # left-align, right-align
print(f"{name.upper():*^20}")        # centered, padded with *

# Useful for tables
students = [("Vibhav", 8.5), ("Yuvraj", 7.8), ("Swapnil", 9.1)]
print(f"{'Name':<12} {'CGPA':>6}")
print("-" * 20)
for name, cgpa in students:
    print(f"{name:<12} {cgpa:>6.1f}")
# Output:
# Name           CGPA
# --------------------
# Vibhav          8.5
# Yuvraj          7.8
# Swapnil         9.1
```

---

## ✏️ String Exercises

```python
# Exercise 1:
# Ask user to enter a sentence.
# Print:
# a) Number of words
# b) Number of characters (excluding spaces)
# c) The sentence reversed
# d) Each word capitalized (Title Case)
# e) How many times the letter 'a' appears (case insensitive)

# Exercise 2:
# Given: "vibhav.shukla@aktu.ac.in"
# Extract:
# a) Username (before @)
# b) Domain (after @)
# c) Is it a valid email? (check: contains @ and .)

# Exercise 3: Palindrome checker
# Ask user to enter a word. Check if it is a palindrome.
# "racecar" → YES it is a palindrome
# "hello"   → NO
# Tip: word == word[::-1]

# Exercise 4: Word frequency counter
sentence = "the cat sat on the mat the cat is fat"
# Count how many times each word appears.
# Output:
# the  → 3
# cat  → 2
# sat  → 1
# ...etc
# Use a dictionary!
```

---
---

# PART 4 — Functions

## 🤔 What is a Function?

A function is a reusable block of code that does ONE specific job.

Real life: A coffee machine is a function.
- You give it inputs (water, coffee powder)
- It does its job
- It gives you output (coffee)

Without functions → you repeat the same code 100 times
With functions    → you write it once, call it 100 times

---

## Basic Syntax

```python
def function_name(parameters):
    # code
    return result    # optional

# Call it:
result = function_name(arguments)
```

---

## Examples

```python
# Example 1 — No input, no output
def greet():
    print("Hello! Welcome to Python.")

greet()     # call the function
greet()     # call again — same code, runs again


# Example 2 — With input (parameters)
def greet_person(name):
    print(f"Hello {name}! Welcome.")

greet_person("Vibhav")    # Hello Vibhav! Welcome.
greet_person("Yuvraj")    # Hello Yuvraj! Welcome.


# Example 3 — With input AND output (return)
def add(a, b):
    result = a + b
    return result          # sends value back to caller

total = add(10, 5)
print(total)               # 15
print(add(100, 200))       # 300


# Example 4 — Multiple return values
def min_max(numbers):
    return min(numbers), max(numbers)   # returns a tuple

lo, hi = min_max([5, 2, 8, 1, 9])
print(lo, hi)    # 1  9


# Example 5 — Default parameter values
def power(base, exp=2):      # exp defaults to 2 if not given
    return base ** exp

print(power(3))       # 9    (3 to the power 2)
print(power(3, 3))    # 27   (3 to the power 3)
print(power(2, 10))   # 1024
```

---

## *args — Accept Any Number of Arguments

```python
# What if you don't know how many arguments you'll get?

def add_all(*args):      # *args collects all arguments into a TUPLE
    total = 0
    for num in args:
        total += num
    return total

print(add_all(1, 2))           # 3
print(add_all(1, 2, 3, 4, 5)) # 15
print(add_all(10))             # 10
```

---

## **kwargs — Accept Named Arguments

```python
def student_info(**kwargs):    # **kwargs collects into a DICT
    for key, value in kwargs.items():
        print(f"{key}: {value}")

student_info(name="Vibhav", age=20, college="AKTU")
# name: Vibhav
# age: 20
# college: AKTU
```

---

## Variable Scope — Local vs Global

```python
x = 10          # GLOBAL variable — exists everywhere

def my_func():
    y = 20      # LOCAL variable — only exists inside this function
    print(x)    # can read global variable
    print(y)    # can read local variable

my_func()
print(x)        # works — x is global
print(y)        # ERROR! y is local to my_func, doesn't exist here

# To MODIFY a global variable inside a function:
count = 0

def increment():
    global count       # tell Python we want the GLOBAL count
    count += 1

increment()
increment()
print(count)    # 2
```

---

## Lambda — One-Line Functions

```python
# Normal function
def square(x):
    return x ** 2

# Same function as lambda
square = lambda x: x ** 2

print(square(5))    # 25

# Useful with sort, filter, map
students = [("Vibhav", 8.5), ("Yuvraj", 7.8), ("Swapnil", 9.1)]

# Sort by CGPA (second element of each tuple)
students.sort(key=lambda s: s[1])
print(students)
# [('Yuvraj', 7.8), ('Vibhav', 8.5), ('Swapnil', 9.1)]

# Sort descending
students.sort(key=lambda s: s[1], reverse=True)
```

---

## ✏️ Function Exercises

```python
# Exercise 1: Basic functions
# Write a function is_even(n) → returns True if n is even, False if odd
# Write a function factorial(n) → returns n! (5! = 5x4x3x2x1 = 120)
# Write a function is_palindrome(word) → returns True/False

# Exercise 2: List functions
# Write a function get_stats(numbers) that takes a list and returns:
# (minimum, maximum, average, sum) as a tuple
# Test: get_stats([4, 7, 2, 9, 1]) → (1, 9, 4.6, 23)

# Exercise 3: String function
# Write a function count_vowels(text) → returns count of vowels (a,e,i,o,u)
# Test: count_vowels("Vibhav Shukla") → 4

# Exercise 4: Real world
# Write a function calculate_grade(marks_list) that:
# - Takes a list of marks
# - Calculates the average
# - Returns the grade: A+ (>=90), A (>=80), B (>=70), C (>=60), F (<60)

# Exercise 5: Using lambda
# Given a list of words, sort them by their LENGTH (shortest first)
words = ["python", "is", "an", "amazing", "programming", "language"]
# Expected: ['is', 'an', 'python', 'amazing', 'language', 'programming']
```

---
---

# PART 5 — DSA Today

## 🔴 Problem 1 — Best Time to Buy and Sell Stock (Easy)

Link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

Problem:
You have prices of a stock for N days. You can buy once and sell once.
Find the maximum profit you can make.

Example:
prices = [7, 1, 5, 3, 6, 4]
Buy on day 2 (price=1), sell on day 5 (price=6) → profit = 6-1 = 5

---

### Think first:
- You want to BUY at the lowest price
- and SELL at the highest price AFTER buying

### Solution — Python

```python
def maxProfit(prices):
    min_price = float('inf')    # start with infinity (any price will be smaller)
    max_profit = 0

    for price in prices:
        if price < min_price:
            min_price = price           # found a cheaper buy price
        elif price - min_price > max_profit:
            max_profit = price - min_price   # found a better profit

    return max_profit

# Test
print(maxProfit([7, 1, 5, 3, 6, 4]))   # 5
print(maxProfit([7, 6, 4, 3, 1]))       # 0  (prices only fall, no profit)
```

### Step-by-step walkthrough:
```
prices = [7, 1, 5, 3, 6, 4]

price=7 → min=7,  profit=0
price=1 → min=1,  profit=0   (1 < 7, update min)
price=5 → min=1,  profit=4   (5-1=4 > 0, update profit)
price=3 → min=1,  profit=4   (3-1=2, not better than 4)
price=6 → min=1,  profit=5   (6-1=5 > 4, update profit!)
price=4 → min=1,  profit=5   (4-1=3, not better)

Answer: 5 ✅
```

### Solution — Java

```java
public int maxProfit(int[] prices) {
    int minPrice = Integer.MAX_VALUE;   // like float('inf') in Python
    int maxProfit = 0;

    for (int price : prices) {
        if (price < minPrice) {
            minPrice = price;
        } else if (price - minPrice > maxProfit) {
            maxProfit = price - minPrice;
        }
    }

    return maxProfit;
}
```

---

## 🔴 Problem 2 — Maximum Subarray (Medium)

Link: https://leetcode.com/problems/maximum-subarray/

Problem:
Given a list of integers, find the CONTIGUOUS subarray with the largest sum.

Example:
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Answer: 6 → subarray [4, -1, 2, 1] has sum = 6

---

### The Algorithm — Kadane's Algorithm

Key idea:
→ At each position, decide: should I EXTEND the current subarray OR start fresh here?
→ If adding current element makes sum worse than just taking current element alone → start fresh!

```python
def maxSubArray(nums):
    current_sum = nums[0]    # start with first element
    max_sum     = nums[0]

    for i in range(1, len(nums)):
        # Should I extend current subarray OR start fresh at nums[i]?
        current_sum = max(nums[i], current_sum + nums[i])

        # Update max if current is better
        max_sum = max(max_sum, current_sum)

    return max_sum

# Test
print(maxSubArray([-2, 1, -3, 4, -1, 2, 1, -5, 4]))  # 6
print(maxSubArray([1]))                                # 1
print(maxSubArray([-1, -2, -3]))                       # -1
```

### Walkthrough:
```
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]

i=0: current=-2, max=-2
i=1: current=max(1, -2+1)=max(1,-1)=1,   max=1
i=2: current=max(-3, 1-3)=max(-3,-2)=-2, max=1
i=3: current=max(4, -2+4)=max(4,2)=4,    max=4
i=4: current=max(-1,4-1)=max(-1,3)=3,    max=4
i=5: current=max(2,3+2)=max(2,5)=5,      max=5
i=6: current=max(1,5+1)=max(1,6)=6,      max=6 ✅
i=7: current=max(-5,6-5)=max(-5,1)=1,    max=6
i=8: current=max(4,1+4)=max(4,5)=5,      max=6

Answer: 6 ✅
```

### Solution — Java

```java
public int maxSubArray(int[] nums) {
    int currentSum = nums[0];
    int maxSum     = nums[0];

    for (int i = 1; i < nums.length; i++) {
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSum     = Math.max(maxSum, currentSum);
    }

    return maxSum;
}
```

---

## Key Concept Learned Today: Sliding / Running State

Both problems use the same pattern:
→ Keep track of a RUNNING variable (min_price, current_sum)
→ Update it at each step
→ Track the BEST result seen so far

This pattern appears in 20+ DSA problems. Remember it!

---

## ✅ DAY 3 COMPLETE CHECKLIST

Tuples:
[ ] Know difference between list and tuple
[ ] Tuple indexing and slicing
[ ] Tuple unpacking (including swap trick)
[ ] All 3 tuple exercises done

Sets:
[ ] Know what a set is (no duplicates, no index)
[ ] add, remove, discard
[ ] Union, intersection, difference
[ ] Set exercises done

Strings:
[ ] strip, upper, lower, replace
[ ] split and join
[ ] find, count, startswith, endswith
[ ] isalpha, isdigit, isalnum
[ ] f-string formatting with :.2f
[ ] All 4 string exercises done (including palindrome + word frequency)

Functions:
[ ] def and return — can write independently
[ ] Default parameters
[ ] *args and **kwargs — understand the concept
[ ] Variable scope (local vs global) — understood
[ ] Lambda functions — can write one
[ ] All 5 function exercises done

DSA:
[ ] Best Time to Buy and Sell Stock — Python ✅
[ ] Best Time to Buy and Sell Stock — Java ✅
[ ] Maximum Subarray (Kadane's) — Python ✅
[ ] Maximum Subarray (Kadane's) — Java ✅

---

## 🔜 Day 4 Will Cover:
- File I/O (read and write files — used in data analysis!)
- Error handling (try / except / finally)
- Modules and imports (os, sys, json, datetime)
- Classes and OOP — the big one!
- DSA: Product of Array Except Self, Maximum Product Subarray

---

Stuck? Say "I am stuck on [topic]" and I will explain differently! 🎯
