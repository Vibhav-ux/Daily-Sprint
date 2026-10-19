# 🐍 Python Full Stack — Day 11: NumPy — Numerical Computing
### Phase 1: Data Analyst | Arrays · Broadcasting · Vectorized Operations · Linear Algebra

---

> 📌 **Setup:** `pip install numpy`
> File to create: `day11_numpy.py`

---

## PART 1 — Why NumPy? The Performance Story

Python's built-in `list` is slow for math because it:
- Stores Python objects (large memory overhead)
- Loops in Python bytecode (slow interpreter)

NumPy arrays are stored as raw C memory blocks. Operations run in compiled C code → **10–100× faster**.

```python
import numpy as np

# ── Creating arrays ──
arr1d = np.array([1, 2, 3, 4, 5])
arr2d = np.array([[1, 2, 3],
                   [4, 5, 6]])

print(arr1d.dtype)    # int64  (or int32 on Windows)
print(arr2d.shape)    # (2, 3)  → 2 rows, 3 columns
print(arr2d.ndim)     # 2
print(arr2d.size)     # 6  (total elements)
```

---

## PART 2 — Array Creation Functions

```python
import numpy as np

zeros   = np.zeros((3, 4))           # 3×4 array of 0.0
ones    = np.ones((2, 2))            # 2×2 array of 1.0
full    = np.full((3, 3), 7)         # 3×3 array of 7
eye     = np.eye(4)                  # 4×4 identity matrix
rand    = np.random.rand(3, 3)       # random 0.0–1.0
randn   = np.random.randn(1000)      # standard normal distribution
arange  = np.arange(0, 10, 2)       # [0  2  4  6  8]
linspace = np.linspace(0, 1, 5)     # [0.   0.25  0.5  0.75  1.0]
```

---

## PART 3 — Indexing, Slicing & Boolean Masking

```python
arr = np.array([[10, 20, 30],
                [40, 50, 60],
                [70, 80, 90]])

# Basic indexing
print(arr[1, 2])          # 60 (row 1, col 2)

# Slicing rows/columns
print(arr[:, 1])          # [20 50 80]  — all rows, column 1
print(arr[0:2, 0:2])     # top-left 2×2 submatrix

# Boolean masking (vectorized filtering!)
mask = arr > 50
print(arr[mask])          # [60  70  80  90] — flattened result

# Fancy indexing (select specific rows)
print(arr[[0, 2]])        # rows 0 and 2
```

---

## PART 4 — Broadcasting: The Power Feature

Broadcasting allows NumPy to perform arithmetic between arrays of **different shapes** by automatically expanding the smaller array.

```python
# ── Scalar broadcast ──
arr = np.array([1, 2, 3, 4, 5])
print(arr * 2)             # [2  4  6  8  10] — no loop needed!

# ── Column broadcast ──
matrix = np.ones((3, 4))
row_vector = np.array([1, 2, 3, 4])
print(matrix + row_vector)  # adds row to EACH row of matrix

# ── Real use: feature normalization ──
data = np.array([[175.0, 70.0],    # height, weight
                 [180.0, 85.0],
                 [160.0, 55.0]])

mean = data.mean(axis=0)   # mean of each column
std  = data.std(axis=0)    # std of each column
normalized = (data - mean) / std  # broadcast subtraction & division
print(normalized)
```

---

## PART 5 — Vectorized Operations & Math Functions

```python
import numpy as np

x = np.linspace(0, 2 * np.pi, 100)

# Trigonometry
sin_x = np.sin(x)
cos_x = np.cos(x)

# Aggregations
grades = np.array([85, 92, 78, 95, 88, 72, 91])
print(f"Mean:   {grades.mean():.1f}")
print(f"Median: {np.median(grades):.1f}")
print(f"Std:    {grades.std():.1f}")
print(f"Max:    {grades.max()}, Min: {grades.min()}")
print(f"Cumulative sum: {grades.cumsum()}")

# Matrix operations
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(A @ B)               # Matrix multiplication (dot product)
print(np.dot(A, B))        # Same as A @ B
print(A.T)                 # Transpose
print(np.linalg.det(A))    # Determinant: -2.0
```

---

## 💡 Day 11 Challenge

1. Create a 5×5 matrix of random integers (0–100) using `np.random.randint`.
2. Find: the row with the highest sum, the column with the lowest mean.
3. Normalize the matrix (subtract mean, divide by std).
