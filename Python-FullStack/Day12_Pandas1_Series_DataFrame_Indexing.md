# 🐍 Python Full Stack — Day 12: Pandas Part 1 — Data Wrangling Foundations
### Phase 1: Data Analyst | Series · DataFrame · Loading Data · Indexing · Filtering · Cleaning

---

> 📌 **Setup:** `pip install pandas openpyxl`
> File to create: `day12_pandas1.py`

---

## PART 1 — Why Pandas? The Core Data Structures

Pandas is built on top of NumPy and provides two essential structures:

| Structure | Analogy | Use Case |
|---|---|---|
| **Series** | Single Excel column with a labeled index | 1D data — prices, scores, a single feature |
| **DataFrame** | Full Excel spreadsheet | 2D tabular data — rows × columns |

```python
import pandas as pd
import numpy as np

# ── Series ──
scores = pd.Series([85, 92, 78, 95, 88], index=["Alice", "Bob", "Carol", "Dave", "Eve"])
print(scores["Alice"])   # 85
print(scores.mean())     # 87.6
print(scores[scores > 85])  # Boolean indexing → Bob, Dave, Eve

# ── DataFrame ──
data = {
    "name":   ["Vibhav", "Alice", "Bob", "Carol"],
    "score":  [92, 85, 78, 95],
    "track":  ["Python", "JS", "DSA", "Python"],
    "active": [True, True, False, True]
}
df = pd.DataFrame(data)
print(df)
print(df.shape)    # (4, 4)   → 4 rows, 4 columns
print(df.dtypes)   # column data types
```

---

## PART 2 — Loading Real Data

```python
import pandas as pd

# ── CSV (most common) ──
df = pd.read_csv("students.csv")

# ── CSV with options ──
df = pd.read_csv(
    "sales.csv",
    sep=",",                  # delimiter
    header=0,                 # row index of column names
    index_col="student_id",   # set a column as the row index
    usecols=["name", "score", "track"],   # load only needed columns
    na_values=["N/A", "none", ""],        # treat these as NaN
    parse_dates=["enrolled_date"]         # parse as datetime
)

# ── JSON ──
df_json = pd.read_json("data.json")

# ── Excel ──
df_excel = pd.read_excel("data.xlsx", sheet_name="Sheet1")

# First look at any dataset
print(df.head(10))    # first 10 rows
print(df.tail(5))     # last 5 rows
print(df.info())      # dtypes, non-null counts, memory usage
print(df.describe())  # mean, std, min, max, quartiles for numeric columns
```

---

## PART 3 — Selecting: `[]`, `.loc[]`, `.iloc[]`

This is the most important section. Know these cold.

```python
# ── Selecting COLUMNS ──
df["name"]                   # Series (single column)
df[["name", "score"]]        # DataFrame (multiple columns)

# ── .loc[] → Label-based indexing (row label OR boolean mask, column name) ──
df.loc[0]                    # Row with index label 0
df.loc[0:2, "name":"score"]  # Rows 0-2, columns name through score (INCLUSIVE)
df.loc[df["track"] == "Python", ["name", "score"]]  # Filter rows + pick columns

# ── .iloc[] → Integer position-based (like Python list slicing) ──
df.iloc[0]                   # First row
df.iloc[0:3, 0:2]            # Rows 0-2, columns 0-1 (exclusive end, like Python)
df.iloc[[0, 2, -1]]          # Rows 0, 2, and last row

# ── Key difference ──
# .loc[0:3]  → includes row 3 (label inclusive)
# .iloc[0:3] → excludes row 3 (position exclusive, like Python slicing)
```

---

## PART 4 — Filtering Rows (Boolean Masking)

```python
# ── Single condition ──
high_scorers = df[df["score"] >= 90]

# ── Multiple conditions: use & (AND) and | (OR), always parenthesise! ──
active_python = df[(df["track"] == "Python") & (df["active"] == True)]

# ── .isin() — membership test ──
top_tracks = df[df["track"].isin(["Python", "DSA"])]

# ── .str methods for string columns ──
df[df["name"].str.startswith("V")]
df[df["name"].str.contains("vi", case=False)]  # case-insensitive

# ── .between() ──
df[df["score"].between(80, 95)]   # 80 ≤ score ≤ 95

# ── .query() — clean string syntax ──
result = df.query("score >= 90 and track == 'Python'")
```

---

## PART 5 — Handling Missing Data

```python
# ── Detect missing values ──
print(df.isnull().sum())       # count of NaN per column
print(df.isnull().sum() / len(df) * 100)  # percentage missing

# ── Drop rows with any NaN ──
df_clean = df.dropna()

# ── Drop rows where specific columns are NaN ──
df_clean = df.dropna(subset=["score", "name"])

# ── Fill NaN with a value ──
df["score"] = df["score"].fillna(df["score"].median())   # fill with median
df["track"] = df["track"].fillna("Unknown")              # fill with string

# ── Forward fill (useful for time series) ──
df["score"] = df["score"].ffill()

# ── Drop columns with too many NaN (>50%) ──
threshold = len(df) * 0.5
df = df.dropna(axis=1, thresh=threshold)
```

---

## PART 6 — Adding & Modifying Columns

```python
# ── Add a computed column ──
df["grade"] = df["score"].apply(lambda s: "A" if s >= 90 else ("B" if s >= 80 else "C"))

# ── map() for replacing values ──
df["track_code"] = df["track"].map({"Python": 1, "JS": 2, "DSA": 3})

# ── Rename columns ──
df.rename(columns={"name": "student_name", "score": "final_score"}, inplace=True)

# ── Drop columns ──
df.drop(columns=["track_code"], inplace=True)

# ── Reorder columns ──
df = df[["student_name", "track", "final_score", "active"]]
```

---

## 💡 Day 12 Challenge

Load a CSV with student data (create a sample one manually), then:
1. Find the top 3 students by score per track.
2. Count how many students are in each track.
3. Replace all scores below 50 with `NaN`, then fill with the column median.

```python
# Starter: create sample data
import pandas as pd
data = {
    "name": ["Vibhav","Alice","Bob","Carol","Dave","Eve","Frank"],
    "score": [92, 85, 45, 95, 78, 88, 30],
    "track": ["Python","JS","Python","DSA","JS","Python","DSA"]
}
df = pd.DataFrame(data)
```
