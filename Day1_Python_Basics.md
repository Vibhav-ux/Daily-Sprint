# 🐍 Day 1 — Python Study Material
### Phase 0 | October 1, 2026 | Vibhav Shukla

---

> 📌 **How to use this file:**
> Read one section → open VS Code → type the code yourself (don't copy-paste) → run it → then move to next section.
> Typing it yourself is 10x better than copy-pasting. Trust the process.

---

# PART 1 — Variables & Data Types

## 🤔 What is a Variable?

Think of your phone contacts list.
- You save a number with a name: "Mom" → 9876543210
- Whenever you want to call mom, you don't type the number — you just say "Mom"

A variable is exactly this. You give a name to a value so you can use it later.

```python
# Syntax:  variable_name = value
name = "Vibhav"
age = 20
```

The = here is NOT "equals" like in maths.
It means "store this value in this box".

---

## 📦 The 4 Basic Data Types

| Type    | Full Name | Example               | When to use       |
|---------|-----------|-----------------------|-------------------|
| int     | Integer   | 20, 100, -5           | Whole numbers     |
| float   | Float     | 8.5, 3.14, -2.7       | Decimal numbers   |
| str     | String    | "Vibhav", "AKTU"      | Text / words      |
| bool    | Boolean   | True, False           | Yes/No situations |

```python
# int — whole numbers (no decimal)
age = 20
year = 2026
rank = 6

# float — numbers with decimal point
cgpa = 8.5
pi = 3.14159
score = 85.56

# str — text, always inside quotes (" " or ' ')
name = "Vibhav Shukla"
college = "AKTU"
team = 'BORNTOTECH'

# bool — only two values: True or False (capital T and F)
is_ieee_chair = True
has_internship = False
```

---

## 🔍 type() — Check what type a variable is

```python
print(type(20))           # <class 'int'>
print(type(8.5))          # <class 'float'>
print(type("Vibhav"))     # <class 'str'>
print(type(True))         # <class 'bool'>
```

---

## 🖨️ print() — Show output on screen

```python
print("Hello World")          # Hello World
print(42)                     # 42
print(3.14)                   # 3.14

# Print multiple things — separate with comma
print("Name:", "Vibhav")      # Name: Vibhav
print("Age:", 20)             # Age: 20
```

---

## 🎨 f-strings — The Clean Way to Print

Instead of messy string joining, use f-strings
(put f before the quote, variables go in {}):

```python
name = "Vibhav"
age = 20
college = "AKTU"

print(f"My name is {name}")
print(f"I am {age} years old")
print(f"My name is {name}, I am {age} years old at {college}")

# You can even do math inside {}
price = 100
discount = 20
print(f"Final price: {price - discount}")   # Final price: 80
```

---

## ⌨️ input() — Take Input from User

input() pauses the program and waits for the user to type something.
It ALWAYS returns a string (text).

```python
name = input("Enter your name: ")
print(f"Hello {name}!")                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
```

### ⚠️ IMPORTANT: input() gives STRING, not number

```python
# WRONG — age will be "20" (string), not 20 (int)
age = input("Enter your age: ")

# CORRECT — wrap with int() to convert
age = int(input("Enter your age: "))

# For decimals, use float()
cgpa = float(input("Enter your CGPA: "))
```

---

## ✏️ EXERCISES — Part 1

Create a file called day1_part1.py and solve these:

Exercise 1:
Create variables — your name, age, college, year (3), cgpa.
Print all in ONE f-string:
"Hi! I am Vibhav, 20 years old. I study CSE Year 3 at AKTU with 8.5 CGPA."

Exercise 2:
Ask the user to enter their name and city.
Print: "Welcome [name] from [city]! Lets start learning Python 🚀"

Exercise 3:
Run these and write what you THINK the output is before running:
```python
x = 5
y = 2.0
z = "10"
print(type(x))
print(type(y))
print(type(z))
print(int(z) + x)
```

---
---

# PART 2 — Operators

## ➕ Arithmetic Operators

```python
a = 10
b = 3

print(a + b)    # 13   → addition
print(a - b)    # 7    → subtraction
print(a * b)    # 30   → multiplication
print(a / b)    # 3.333... → division (always gives float)
print(a // b)   # 3    → floor division (removes decimal)
print(a % b)    # 1    → modulo (remainder)
print(a ** b)   # 1000 → power (10 to the power 3)
```

### 💡 Modulo % — super useful in DSA!

```python
print(10 % 2)   # 0  → 10 is even
print(11 % 2)   # 1  → 11 is odd
print(15 % 5)   # 0  → 15 is divisible by 5
```

---

## ⚖️ Comparison Operators (Returns True or False)

```python
a = 10
b = 5

print(a > b)    # True
print(a < b)    # False
print(a >= 10)  # True
print(a == 10)  # True  ← two == signs to compare!
print(a != b)   # True  ← not equal
```

⚠️ = vs ==
- = means assign (store):  x = 5
- == means compare (check equal):  x == 5

---

## 🔗 Logical Operators

```python
age = 20
has_id = True

# and → BOTH must be True
print(age >= 18 and has_id == True)   # True

# or → at least ONE must be True
print(age < 18 or has_id == True)     # True

# not → flips True/False
print(not has_id)                     # False
```

---
---

# PART 3 — If / Elif / Else

## 🤔 What is it?

Real life: "If it rains → take umbrella. Else → don't bother."
In code: "If a condition is True → do this. Else → do that."

```python
# Basic syntax
if condition:
    # runs if True
elif another_condition:
    # runs if first was False but this is True
else:
    # runs if ALL above were False
```

⚠️ Indentation is CRUCIAL in Python!
Code inside if MUST be indented (4 spaces or 1 Tab).

---

## 📘 Examples

```python


# Example 1 — grades
marks = 85

if marks >= 90:
    print("Grade: A+")
elif marks >= 80:
    print("Grade: A")
elif marks >= 70:
    print("Grade: B")
elif marks >= 60:
    print("Grade: C")
else:
    print("Grade: Fail")


# Example 2 — even or odd
num = int(input("Enter a number: "))

if num % 2 == 0:
    print(f"{num} is EVEN")
else:
    print(f"{num} is ODD")


# Example 3 — password check
password = input("Enter password: ")

if password == "vibhav123":
    print("Access granted ✅")
else:
    print("Wrong password ❌")
```

---

## ✏️ EXERCISES — Part 3

Create day1_part3.py and solve:

Exercise 1 — CGPA Grader:
Ask user to enter CGPA. Print:
- "Distinction 🏆" if CGPA >= 9
- "First Class 🎉" if CGPA >= 7
- "Second Class" if CGPA >= 5
- "Needs improvement" if below 5

Exercise 2 — Biggest of 3 numbers:
Ask user to enter 3 numbers. Print the biggest one.

Exercise 3 — Voting eligibility:
Ask user their age and voter ID (yes/no).
- age >= 18 AND yes → "You can vote ✅"
- age >= 18 AND no  → "Get your voter ID first"
- age < 18          → "Too young to vote"

Exercise 4 — Simple Calculator:
Ask user for 2 numbers and an operator (+, -, *, /).
Print the result. Handle division by zero!

---
---

# PART 4 — DSA: Arrays (Python + Java)

## 🧮 What is an Array?

Imagine a row of lockers in college — each locker has a number (0, 1, 2...)
and stores something inside.

Array = collection of items stored in order, accessible by position (index).

```
Index:  0     1     2     3     4
      [85]  [90]  [78]  [92]  [88]
```

---

## Arrays in Python (called Lists)

```python
marks = [85, 90, 78, 92, 88]
names = ["Vibhav", "Yuvraj", "Swapnil", "Vaibhav"]

# Access by index (starts at 0!)
print(marks[0])    # 85  ← first
print(marks[-1])   # 88  ← last
print(marks[-2])   # 92  ← second from last

# Length, sum, max, min
print(len(marks))  # 5
print(sum(marks))  # 433
print(max(marks))  # 92
print(min(marks))  # 78

# Loop through
for mark in marks:
    print(mark)

# Loop with index
for i in range(len(marks)):
    print(f"Index {i}: {marks[i]}")
```

---

## Arrays in Java

```java
public class Day1DSA {
    public static void main(String[] args) {

        int[] marks = {85, 90, 78, 92, 88};

        System.out.println(marks[0]);              // 85
        System.out.println(marks[marks.length-1]); // 88
        System.out.println(marks.length);          // 5

        // Loop
        for (int i = 0; i < marks.length; i++) {
            System.out.println("Index " + i + ": " + marks[i]);
        }

        // For-each loop
        for (int mark : marks) {
            System.out.println(mark);
        }

        // Find max
        int max = marks[0];
        for (int i = 1; i < marks.length; i++) {
            if (marks[i] > max) {
                max = marks[i];
            }
        }
        System.out.println("Max: " + max);  // 92
    }
}
```

---

## 🔴 LeetCode Problem — Two Sum (Easy)

Link: https://leetcode.com/problems/two-sum/

Problem:
Given a list of numbers and a target, find TWO numbers that add up to target.
Return their indexes.

Example:
Input:  nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
Reason: nums[0] + nums[1] = 2 + 7 = 9 ✅

### Solution — Python

```python
def twoSum(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):       # j always AFTER i
            if nums[i] + nums[j] == target:
                return [i, j]
    return []

# Test
print(twoSum([2, 7, 11, 15], 9))   # [0, 1]
print(twoSum([3, 2, 4], 6))        # [1, 2]
```

### Solution — Java

```java
public int[] twoSum(int[] nums, int target) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{};
}
```

Steps to submit:
1. Go to https://leetcode.com/problems/two-sum/
2. Select Python3 → paste Python solution → Submit
3. Select Java → paste Java solution → Submit
4. Screenshot both Accepted results!

---

## ✅ DAY 1 COMPLETE CHECKLIST

Python:
[ ] Variables & data types — understood
[ ] f-strings — can write without looking
[ ] input() with int() conversion — working
[ ] Arithmetic operators (%, //) — understood
[ ] Comparison + logical operators — understood
[ ] if / elif / else — can write independently
[ ] CGPA Grader — completed
[ ] Simple Calculator — completed

DSA:
[ ] Array basics in Python — understood
[ ] Array basics in Java — understood
[ ] Two Sum — solved in Python on LeetCode ✅
[ ] Two Sum — solved in Java on LeetCode ✅

---

## 🔜 Tomorrow (Day 2):
- for loops and while loops
- range() function
- Lists deep dive (append, remove, sort, slice)
- DSA: Best Time to Buy Stock, Contains Duplicate

---

Stuck somewhere? Tell me: "I am stuck on [topic] — [what's confusing you]"
and I will explain it differently until it clicks! 🎯
