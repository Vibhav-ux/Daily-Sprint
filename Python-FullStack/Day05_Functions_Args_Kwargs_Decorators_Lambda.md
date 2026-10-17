# 🐍 Python Full Stack — Day 05: Functions, Decorators & Closures
### Phase 0: Foundations | Parameters, *args, **kwargs, Lambdas & Decorators

---

## 📌 1. Function Arguments Architecture

```python
# Positional, Default, *args, **kwargs
def build_profile(name: str, role: str = "Engineer", *skills, **metadata):
    print(f"Name: {name}, Role: {role}")
    print(f"Skills: {skills}")       # tuple of variable positional args
    print(f"Metadata: {metadata}")   # dict of variable keyword args

build_profile("Vibhav", "Full Stack Dev", "Python", "FastAPI", "PostgreSQL", location="India", remote=True)
```

---

## 📌 2. First-Class Functions & Lambdas

In Python, functions are first-class citizens. They can be passed as arguments, returned from other functions, and assigned to variables.

```python
# Lambda: Anonymous short function
square = lambda x: x ** 2
add = lambda a, b: a + b

# Sorting with custom keys
users = [
    {"name": "Alice", "age": 25},
    {"name": "Bob", "age": 20},
    {"name": "Charlie", "age": 30}
]
users.sort(key=lambda u: u["age"])
# Sorted by age ascending
```

---

## 📌 3. Decorators (High-Frequency Web Framework Concept)

A decorator wraps a function to extend its behavior without altering its source code. Crucial in **FastAPI** (`@app.get()`) and **Flask** (`@app.route()`).

### Writing an Execution Timer Decorator
```python
import time
from functools import wraps

def time_it(func):
    @wraps(func)  # Preserves func's docstring and name
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        duration = time.perf_counter() - start
        print(f"⚡ [{func.__name__}] executed in {duration:.6f}s")
        return result
    return wrapper

@time_it
def compute_heavy_sum(n: int):
    return sum(i * i for i in range(n))

compute_heavy_sum(1_000_000)
```

---

## 📌 4. Closures & Scope (LEGB Rule)

Scope lookup order: **Local $\rightarrow$ Enclosing $\rightarrow$ Global $\rightarrow$ Built-in**.

```python
def make_multiplier(factor: int):
    # Enclosing scope holds 'factor'
    def multiply(number: int) -> int:
        return number * factor
    return multiply

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5)) # 10
print(triple(5)) # 15
```
