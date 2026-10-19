# 🐍 Python Full Stack — Day 08: Advanced Python Internals
### Phase 0: Foundations | Generators · asyncio · Type Hints · Context Managers · dataclasses

---

> 📌 Files to create: `day8_generators.py` | `day8_async.py` | `day8_types.py`

---

## PART 1 — Generators: Lazy Evaluation & Memory Efficiency

### The Problem with Regular Lists
A list materializes ALL items into memory at once.
If you need the first 10 numbers from 1,000,000,000 — a list wastes enormous memory.

A **generator** is a function that uses `yield` instead of `return`. It produces values **one at a time, on demand**.

```python
# ── Regular function: loads everything into memory
def square_list(n: int) -> list[int]:
    return [x ** 2 for x in range(n)]  # all n items in RAM

# ── Generator function: produces one value at a time
def square_generator(n: int):
    for x in range(n):
        yield x ** 2    # pauses here, returns value, resumes on next()

# Usage
gen = square_generator(1_000_000_000)   # creates generator object instantly
print(next(gen))   # 0   ← only computes first item
print(next(gen))   # 1   ← computes second item on demand
print(next(gen))   # 4

# ── Iterate naturally
for val in square_generator(5):
    print(val)    # 0  1  4  9  16

# ── Generator expression (like list comp but lazy)
evens = (x for x in range(100) if x % 2 == 0)  # parentheses, not brackets
```

### Generator Pipeline Pattern (Unix-pipe style)
```python
def read_large_file(filepath: str):
    with open(filepath, "r") as f:
        for line in f:
            yield line.strip()

def filter_errors(lines):
    for line in lines:
        if "ERROR" in line:
            yield line

def count_lines(lines):
    return sum(1 for _ in lines)

# Chain generators without loading file into memory
lines = read_large_file("server.log")
errors = filter_errors(lines)
total = count_lines(errors)
print(f"Total errors: {total}")
```

---

## PART 2 — `asyncio`: Cooperative Concurrency

### Sync vs Async Mental Model
- **Synchronous (sync):** Waiter takes your order, walks to kitchen, WAITS for food, brings food, then takes next table's order. (Blocking)
- **Asynchronous (async):** Waiter takes your order, sends it to kitchen, takes next table's order while food is being prepared, serves when ready. (Non-blocking)

```python
import asyncio

# ── Define coroutines with async def ──
async def fetch_data(url: str, delay: float) -> str:
    print(f"📡 Fetching {url}...")
    await asyncio.sleep(delay)   # await = "pause, let others run"
    return f"✅ Data from {url}"

# ── Run coroutines SEQUENTIALLY (slow way)
async def main_sequential():
    result1 = await fetch_data("api/users", 2.0)
    result2 = await fetch_data("api/products", 1.5)
    print(result1, result2)  # Takes 3.5 seconds total

# ── Run coroutines CONCURRENTLY (fast way)
async def main_concurrent():
    result1, result2 = await asyncio.gather(
        fetch_data("api/users", 2.0),
        fetch_data("api/products", 1.5)
    )
    print(result1, result2)  # Takes ~2 seconds (overlap!)

asyncio.run(main_concurrent())
```

### When to use asyncio
✅ Waiting for network calls (APIs, databases)
✅ Reading/writing files asynchronously
✅ FastAPI routes (built on asyncio)
❌ CPU-heavy computation (use `multiprocessing` instead)

---

## PART 3 — Type Hints & Annotations

Python is dynamically typed, but type hints make code self-documenting and enable IDE error detection.

```python
from typing import Optional, Union
from collections.abc import Callable, Generator

# ── Basic type hints ──
def greet(name: str, times: int = 1) -> str:
    return f"Hello, {name}! " * times

# ── Optional (can be None) ──
def find_user(user_id: int) -> Optional[dict]:
    db = {1: {"name": "Vibhav"}}
    return db.get(user_id)  # returns dict or None

# ── Union (multiple types accepted) ──
def process(data: Union[str, list[str]]) -> list[str]:
    if isinstance(data, str):
        return [data]
    return data

# ── Type hints for functions passed as arguments ──
def apply_twice(fn: Callable[[int], int], x: int) -> int:
    return fn(fn(x))

double = lambda n: n * 2
print(apply_twice(double, 3))   # 12
```

---

## PART 4 — Context Managers (`with` Protocol)

The `with` statement guarantees cleanup code runs even if an exception occurs. You can write your own!

```python
from contextlib import contextmanager

@contextmanager
def timer(label: str):
    import time
    start = time.perf_counter()
    try:
        yield  # code inside the 'with' block runs here
    finally:
        elapsed = time.perf_counter() - start
        print(f"⏱  [{label}] took {elapsed:.4f}s")

# Usage
with timer("data processing"):
    total = sum(i ** 2 for i in range(1_000_000))
    print(f"Sum = {total}")
```

---

## PART 5 — `dataclasses`: Clean Data Containers

Before `dataclass`, you had to write `__init__`, `__repr__`, and `__eq__` manually every time. `dataclass` generates all of this automatically.

```python
from dataclasses import dataclass, field
from datetime import date

@dataclass
class Student:
    name: str
    email: str
    gpa: float
    enrolled_date: date = field(default_factory=date.today)
    skills: list[str] = field(default_factory=list)

    def is_honour_roll(self) -> bool:
        return self.gpa >= 9.0

vibhav = Student("Vibhav Shukla", "vibhav@aktu.in", gpa=9.2)
vibhav.skills.append("Python")
print(vibhav)              # Auto-generated __repr__
print(vibhav.is_honour_roll())  # True
```

---

## 💡 Day 08 Challenge

Build an async function that:
1. Defines 3 async "API calls" that each sleep for 0.5s and return a mock response.
2. Runs all 3 **concurrently** using `asyncio.gather`.
3. Prints total time taken (should be ~0.5s, not ~1.5s).
