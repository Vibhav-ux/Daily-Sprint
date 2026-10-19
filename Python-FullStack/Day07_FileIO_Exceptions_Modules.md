# 🐍 Python Full Stack — Day 07: File I/O, Exceptions & Standard Library
### Phase 0: Foundations | Read/Write Files, try/except/finally, os · json · datetime

---

> 📌 **How to use:** Read → VS Code → type code → run → next section.
> Files to create: `day7_fileio.py` | `day7_exceptions.py` | `day7_modules.py`

---

## PART 1 — Reading & Writing Files

### Why files matter in real projects
Every backend app reads config files, logs errors to disk, exports data to CSV.
Knowing file I/O is non-negotiable.

```python
# ──────────────────────────────────────────
# Writing a file
# ──────────────────────────────────────────
with open("notes.txt", "w", encoding="utf-8") as f:
    f.write("Day 07 — File I/O\n")
    f.write("Python is awesome!\n")

# The 'with' block auto-closes the file even if an error occurs.
# Never open files without 'with' in production code.

# ──────────────────────────────────────────
# Reading the whole file at once
# ──────────────────────────────────────────
with open("notes.txt", "r", encoding="utf-8") as f:
    content = f.read()          # entire text as one string
print(content)

# ──────────────────────────────────────────
# Reading line by line (memory-efficient for large files)
# ──────────────────────────────────────────
with open("notes.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())     # strip() removes \n at the end

# ──────────────────────────────────────────
# Appending (not overwriting)
# ──────────────────────────────────────────
with open("notes.txt", "a", encoding="utf-8") as f:
    f.write("Day 07 complete ✅\n")
```

### File Modes Cheat Sheet

| Mode | Meaning |
|------|---------|
| `"r"` | Read (default). File must exist. |
| `"w"` | Write. Creates or **overwrites** file. |
| `"a"` | Append. Creates or adds to end of file. |
| `"rb"` | Read **binary** (images, PDFs, etc.) |
| `"x"` | Exclusive create — fails if file already exists. |

---

## PART 2 — Exception Handling (`try / except / else / finally`)

### The Real-World Mental Model
Imagine your code drives a car:
- `try` → start the engine and drive
- `except` → what to do if a crash happens
- `else` → runs only if NO crash (arrived safely)
- `finally` → always runs (like unbuckling seatbelt — happens no matter what)

```python
def read_user_file(filepath: str) -> str:
    try:
        with open(filepath, "r", encoding="utf-8") as f:
            data = f.read()
    except FileNotFoundError:
        print(f"❌ File not found: {filepath}")
        return ""
    except PermissionError:
        print(f"🔒 No permission to read: {filepath}")
        return ""
    except Exception as e:
        print(f"💥 Unexpected error: {type(e).__name__}: {e}")
        return ""
    else:
        print(f"✅ File read successfully ({len(data)} chars)")
        return data
    finally:
        print("📂 Finished file operation (always runs).")

result = read_user_file("notes.txt")
```

### Raising Your Own Exceptions

```python
def set_age(age: int):
    if not isinstance(age, int):
        raise TypeError(f"Age must be int, got {type(age).__name__}")
    if age < 0 or age > 150:
        raise ValueError(f"Age {age} is out of realistic range")
    return age

try:
    set_age(-5)
except ValueError as e:
    print(f"Caught: {e}")
```

---

## PART 3 — The `os` Module (Filesystem Navigation)

```python
import os

# Current working directory
print(os.getcwd())

# List all files in a directory
files = os.listdir(".")
print(files)

# Check if path exists
print(os.path.exists("notes.txt"))    # True / False
print(os.path.isfile("notes.txt"))    # is it a file?
print(os.path.isdir("Daily-Sprint"))  # is it a directory?

# Join paths (cross-platform — works on Windows AND Linux)
base = os.path.join("Daily-Sprint", "Python-FullStack", "notes.txt")
print(base)   # Daily-Sprint\Python-FullStack\notes.txt on Windows

# Create a directory (no error if already exists)
os.makedirs("output/logs", exist_ok=True)

# Get file size in bytes
size = os.path.getsize("notes.txt")
print(f"File size: {size} bytes")
```

---

## PART 4 — JSON: Reading & Writing Structured Data

JSON is the language of the web. Every API sends and receives JSON.

```python
import json

# ── Python dict → JSON string ──
user = {
    "name": "Vibhav Shukla",
    "streak": 7,
    "skills": ["Python", "DSA", "JavaScript"],
    "active": True
}

json_string = json.dumps(user, indent=2)
print(json_string)

# ── Save dict to .json file ──
with open("user.json", "w", encoding="utf-8") as f:
    json.dump(user, f, indent=2)

# ── Load .json file back into Python dict ──
with open("user.json", "r", encoding="utf-8") as f:
    loaded = json.load(f)

print(loaded["name"])      # "Vibhav Shukla"
print(loaded["skills"])    # ['Python', 'DSA', 'JavaScript']

# ── Parse JSON string → dict ──
raw = '{"day": 7, "done": true}'
parsed = json.loads(raw)
print(parsed["day"])       # 7
```

---

## PART 5 — `datetime` Module: Working With Time

```python
from datetime import datetime, timedelta

# Current date and time
now = datetime.now()
print(now)                             # 2026-10-19 11:00:00.000000

# Formatting (strftime = string-from-time)
formatted = now.strftime("%d %B %Y | %H:%M")
print(formatted)                       # 19 October 2026 | 11:00

# Parsing a date string → datetime object (strptime = string-parse-time)
due_date = datetime.strptime("2026-12-31", "%Y-%m-%d")

# Time arithmetic
one_week_later = now + timedelta(weeks=1)
days_until_deadline = (due_date - now).days
print(f"Days until year end: {days_until_deadline}")
```

---

## 💡 Day 07 Challenge

Write a function `save_daily_log(day: int, notes: str)` that:
1. Creates a `logs/` directory if it doesn't exist.
2. Saves a JSON file `logs/day_{day:02d}.json` with keys: `day`, `notes`, `timestamp`.
3. Returns the absolute path of the saved file.

```python
import json, os
from datetime import datetime

def save_daily_log(day: int, notes: str) -> str:
    os.makedirs("logs", exist_ok=True)
    log = {
        "day": day,
        "notes": notes,
        "timestamp": datetime.now().isoformat()
    }
    path = os.path.join("logs", f"day_{day:02d}.json")
    with open(path, "w", encoding="utf-8") as f:
        json.dump(log, f, indent=2)
    return os.path.abspath(path)

print(save_daily_log(7, "Learned file I/O and JSON today!"))
```
