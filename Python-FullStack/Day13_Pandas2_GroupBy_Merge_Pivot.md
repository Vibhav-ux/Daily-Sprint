# 🐍 Python Full Stack — Day 13: Pandas Part 2 — Data Analysis & Transformation
### Phase 1: Data Analyst | groupby · merge · pivot · apply · datetime · Export

---

> 📌 File to create: `day13_pandas2.py`

---

## PART 1 — `groupby`: The Split-Apply-Combine Pattern

`groupby` is one of the most powerful operations in data analysis.
It works in three steps:
1. **Split** — divide DataFrame by unique values in a column
2. **Apply** — compute a function on each group
3. **Combine** — merge results back into a single DataFrame

```python
import pandas as pd

df = pd.DataFrame({
    "student": ["Vibhav","Alice","Bob","Carol","Dave","Eve"],
    "track":   ["Python","JS","Python","DSA","JS","Python"],
    "score":   [92, 85, 78, 95, 80, 88],
    "week":    [1, 1, 1, 2, 2, 2]
})

# ── Basic groupby ──
track_stats = df.groupby("track")["score"].agg(["mean", "max", "min", "count"])
print(track_stats)

# ── Multiple columns groupby ──
weekly = df.groupby(["track", "week"])["score"].mean()
print(weekly)

# ── Named aggregations (cleaner syntax) ──
summary = df.groupby("track").agg(
    avg_score   = ("score", "mean"),
    top_score   = ("score", "max"),
    total_students = ("student", "count")
).round(1)

# ── Filter groups by aggregate condition ──
active_tracks = df.groupby("track").filter(lambda g: g["score"].mean() >= 85)

# ── Transform: apply group-level computation back to original rows ──
df["score_vs_track_avg"] = df.groupby("track")["score"].transform(
    lambda x: x - x.mean()
)
```

---

## PART 2 — Merging & Joining DataFrames

```python
import pandas as pd

students = pd.DataFrame({
    "student_id": [1, 2, 3, 4],
    "name": ["Vibhav","Alice","Bob","Carol"]
})

scores = pd.DataFrame({
    "student_id": [1, 2, 3, 5],  # note: id 4 missing, id 5 doesn't exist in students
    "score": [92, 85, 78, 91]
})

# ── Inner join: only matching rows in BOTH tables ──
inner = pd.merge(students, scores, on="student_id", how="inner")
# student_id 4 (Carol) and 5 are excluded

# ── Left join: ALL rows from left, NaN where no match in right ──
left = pd.merge(students, scores, on="student_id", how="left")
# Carol gets NaN for score

# ── Outer join: ALL rows from BOTH tables ──
outer = pd.merge(students, scores, on="student_id", how="outer")

# ── Merging on different column names ──
pd.merge(students, scores, left_on="student_id", right_on="id")

# ── Concatenating DataFrames (stacking rows) ──
batch1 = students.iloc[:2]
batch2 = students.iloc[2:]
combined = pd.concat([batch1, batch2], ignore_index=True)
```

---

## PART 3 — Pivot Tables

```python
# ── pivot_table: aggregated cross-tabulation ──
pivot = df.pivot_table(
    values="score",
    index="track",
    columns="week",
    aggfunc="mean",
    fill_value=0
)
print(pivot)
# Output:
# week       1     2
# track
# DSA      0.0  95.0
# JS      85.0  80.0
# Python  85.0  88.0

# ── crosstab: frequency counts ──
ct = pd.crosstab(df["track"], df["week"])
```

---

## PART 4 — `apply` — Custom Transformations

```python
# ── apply to a column (Series) ──
def categorize_score(score: float) -> str:
    if score >= 90:   return "Distinction"
    elif score >= 75: return "Merit"
    elif score >= 50: return "Pass"
    else:             return "Fail"

df["category"] = df["score"].apply(categorize_score)

# ── apply to entire rows (axis=1) ──
def full_label(row) -> str:
    return f"{row['student']} [{row['track']}] — {row['category']}"

df["label"] = df.apply(full_label, axis=1)

# ── Vectorized alternative with np.select (faster than apply for numeric ops) ──
import numpy as np
conditions = [df["score"] >= 90, df["score"] >= 75, df["score"] >= 50]
choices    = ["Distinction", "Merit", "Pass"]
df["category_fast"] = np.select(conditions, choices, default="Fail")
```

---

## PART 5 — Working With Dates (datetime)

```python
import pandas as pd

df_time = pd.DataFrame({
    "student":  ["Vibhav", "Alice", "Bob"],
    "enrolled": ["2026-10-01", "2026-10-05", "2026-10-15"],
    "last_active": ["2026-10-27", "2026-10-20", "2026-10-27"]
})

# Convert string columns to datetime
df_time["enrolled"]    = pd.to_datetime(df_time["enrolled"])
df_time["last_active"] = pd.to_datetime(df_time["last_active"])

# Date arithmetic
df_time["days_active"] = (df_time["last_active"] - df_time["enrolled"]).dt.days

# Extract components
df_time["enroll_month"] = df_time["enrolled"].dt.month_name()
df_time["enroll_week"]  = df_time["enrolled"].dt.isocalendar().week
df_time["is_weekend"]   = df_time["enrolled"].dt.dayofweek >= 5

# Filter by date range
start = pd.Timestamp("2026-10-01")
end   = pd.Timestamp("2026-10-10")
recent = df_time[df_time["enrolled"].between(start, end)]
```

---

## PART 6 — Sorting & Exporting

```python
# ── Sorting ──
df.sort_values("score", ascending=False, inplace=True)
df.sort_values(["track", "score"], ascending=[True, False], inplace=True)  # multi-level

# ── Export ──
df.to_csv("results.csv", index=False)       # don't save the pandas index as a column
df.to_json("results.json", orient="records")
df.to_excel("results.xlsx", index=False, sheet_name="Students")

# ── Method chaining pipeline (clean, readable) ──
final = (
    df
    .query("score >= 70")
    .assign(grade=lambda x: x["score"].apply(categorize_score))
    .groupby("track")
    .agg(avg_score=("score","mean"), count=("student","count"))
    .sort_values("avg_score", ascending=False)
    .round(1)
)
```

---

## 💡 Day 13 Challenge

Using the student DataFrame:
1. Create a pivot table showing average score by `track` and `week`.
2. Add a `percentile_rank` column using `.rank(pct=True)` on the score column.
3. Export the top-5-per-track DataFrame to a CSV.
