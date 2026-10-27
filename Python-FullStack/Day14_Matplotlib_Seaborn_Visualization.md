# 🐍 Python Full Stack — Day 14: Matplotlib & Seaborn — Data Visualization
### Phase 1: Data Analyst | Line · Bar · Scatter · Histogram · Heatmap · Subplots

---

> 📌 **Setup:** `pip install matplotlib seaborn`
> File to create: `day14_visualization.py`

---

## PART 1 — Matplotlib: The Foundation Layer

Matplotlib is the base library for all Python visualization. Every other library (Seaborn, Pandas `.plot()`) is built on top of it.

**Two ways to use Matplotlib:**
1. **`pyplot` interface** — quick, state-based (like MATLAB). Good for simple plots.
2. **Object-oriented (OO) interface** — explicit figure/axes control. Use this for anything complex.

```python
import matplotlib.pyplot as plt
import numpy as np

# ── OO interface (RECOMMENDED for real projects) ──
fig, ax = plt.subplots(figsize=(10, 5))   # fig = canvas, ax = drawing area

x = np.linspace(0, 4 * np.pi, 200)

ax.plot(x, np.sin(x), color="#6366f1", linewidth=2,   label="sin(x)")
ax.plot(x, np.cos(x), color="#f59e0b", linewidth=2, linestyle="--", label="cos(x)")

ax.set_title("Trigonometric Functions", fontsize=16, fontweight="bold")
ax.set_xlabel("x (radians)")
ax.set_ylabel("f(x)")
ax.legend(loc="upper right")
ax.grid(True, alpha=0.3)
ax.set_facecolor("#0f0f1a")
fig.patch.set_facecolor("#0f0f1a")
ax.tick_params(colors="white")

plt.tight_layout()
plt.savefig("trig_functions.png", dpi=150, bbox_inches="tight")
plt.show()
```

---

## PART 2 — The Essential Chart Types

```python
import matplotlib.pyplot as plt
import numpy as np

# ── 1. Bar Chart ──
fig, ax = plt.subplots()
tracks = ["Python", "JavaScript", "DSA"]
counts = [42, 35, 28]
bars = ax.bar(tracks, counts, color=["#6366f1","#f59e0b","#10b981"], edgecolor="white")

# Add value labels on bars
for bar, val in zip(bars, counts):
    ax.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.5,
            str(val), ha="center", fontweight="bold")

ax.set_title("Students Per Track")
plt.show()

# ── 2. Scatter Plot ──
fig, ax = plt.subplots()
study_hours = [2, 3, 5, 1, 4, 6, 3.5, 7, 2.5, 8]
test_scores  = [55, 62, 85, 42, 78, 92, 70, 96, 60, 98]

ax.scatter(study_hours, test_scores, color="#6366f1", s=100, alpha=0.8, edgecolors="white")
ax.set_xlabel("Study Hours per Day")
ax.set_ylabel("Test Score")
ax.set_title("Study Hours vs Score")

# Trend line
z = np.polyfit(study_hours, test_scores, 1)
p = np.poly1d(z)
xs = np.linspace(min(study_hours), max(study_hours), 100)
ax.plot(xs, p(xs), "--", color="#f59e0b", alpha=0.7, label="Trend")
ax.legend()
plt.show()

# ── 3. Histogram ──
fig, ax = plt.subplots()
scores = np.random.normal(78, 12, 500)
ax.hist(scores, bins=25, color="#6366f1", edgecolor="white", alpha=0.85)
ax.axvline(scores.mean(), color="#f59e0b", linestyle="--", linewidth=2, label=f"Mean: {scores.mean():.1f}")
ax.set_title("Score Distribution")
ax.legend()
plt.show()

# ── 4. Multiple Subplots ──
fig, axes = plt.subplots(2, 2, figsize=(12, 8))
fig.suptitle("Daily Sprint Analytics Dashboard", fontsize=16, fontweight="bold")

axes[0, 0].bar(tracks, counts, color="#6366f1")
axes[0, 0].set_title("Students Per Track")

axes[0, 1].scatter(study_hours, test_scores, color="#10b981")
axes[0, 1].set_title("Hours vs Score")

axes[1, 0].hist(scores, bins=20, color="#f59e0b")
axes[1, 0].set_title("Score Distribution")

weekly = [85, 87, 82, 90, 91, 88, 93]
axes[1, 1].plot(weekly, marker="o", color="#f43f5e")
axes[1, 1].set_title("Weekly Average Score")

plt.tight_layout()
plt.savefig("dashboard.png", dpi=150)
plt.show()
```

---

## PART 3 — Seaborn: Statistical Visualization Made Easy

Seaborn wraps Matplotlib and adds statistical plots with beautiful defaults.

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd

# Load Seaborn's built-in dataset for practice
df = sns.load_dataset("tips")   # restaurant tip data

# ── Set theme ──
sns.set_theme(style="darkgrid", palette="muted")

# ── Distribution plot ──
fig, ax = plt.subplots()
sns.histplot(df["total_bill"], kde=True, color="#6366f1", ax=ax)
plt.title("Distribution of Total Bills")
plt.show()

# ── Box plot (shows median, quartiles, outliers) ──
fig, ax = plt.subplots(figsize=(10, 5))
sns.boxplot(data=df, x="day", y="total_bill", hue="sex", palette="Set2", ax=ax)
plt.title("Bill Amount by Day & Gender")
plt.show()

# ── Scatter with regression line ──
sns.lmplot(data=df, x="total_bill", y="tip", hue="sex", height=5, aspect=1.5)
plt.title("Tip vs Bill Amount")
plt.show()

# ── Heatmap: Correlation Matrix ──
fig, ax = plt.subplots(figsize=(8, 6))
numeric_df = df.select_dtypes(include="number")
correlation = numeric_df.corr()
sns.heatmap(
    correlation,
    annot=True,      # show values
    fmt=".2f",
    cmap="coolwarm",
    center=0,
    square=True,
    linewidths=0.5,
    ax=ax
)
plt.title("Feature Correlation Heatmap")
plt.show()

# ── Pairplot (all pairwise relationships at once) ──
sns.pairplot(df, hue="sex", vars=["total_bill", "tip", "size"])
plt.show()
```

---

## PART 4 — Pandas `.plot()` Shortcut

Pandas DataFrames have a `.plot()` method that wraps Matplotlib for quick inline visualizations:

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "week":    [1, 2, 3, 4, 5, 6, 7],
    "Python":  [80, 83, 87, 85, 90, 91, 93],
    "JS":      [75, 79, 81, 84, 86, 88, 90],
    "DSA":     [70, 74, 78, 82, 84, 87, 89]
})

df.set_index("week").plot(
    kind="line", marker="o", figsize=(10, 5),
    title="Average Score Progress by Track",
    color=["#6366f1", "#f59e0b", "#10b981"]
)
plt.ylabel("Score")
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig("progress.png", dpi=150)
plt.show()
```

---

## 💡 Day 14 Challenge

Load the Seaborn `penguins` dataset (`sns.load_dataset("penguins")`):
1. Plot a histogram of `body_mass_g` for each `species` on the same axes.
2. Create a heatmap of the correlation matrix.
3. Create a 2×2 subplot dashboard showing different aspects of the data.
