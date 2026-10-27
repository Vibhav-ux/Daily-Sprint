# 🧮 DSA — Day 11: Dynamic Programming — Advanced Patterns
### 0/1 Knapsack · Grid DP · String DP | Dual Language Solutions (Python 3 + Java)

---

## 📌 Problem 1: 0/1 Knapsack — Partition Equal Subset Sum (LeetCode #416)
> **Problem:** Given array `nums`, determine if it can be partitioned into two subsets with equal sum.

### 💡 Reduce to: "Can we pick numbers that sum to total/2?"
This is the classic 0/1 Knapsack — each number is used at most once.

#### 🐍 Python 3
```python
def canPartition(nums: list[int]) -> bool:
    total = sum(nums)
    if total % 2 != 0:
        return False
    target = total // 2

    # dp[j] = True if we can form sum j using some subset
    dp = [False] * (target + 1)
    dp[0] = True  # base case: empty subset sums to 0

    for num in nums:
        # Traverse right to left to prevent reusing the same number!
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]

    return dp[target]
```

#### ☕ Java
```java
class Solution {
    public boolean canPartition(int[] nums) {
        int total = 0;
        for (int n : nums) total += n;
        if (total % 2 != 0) return false;
        int target = total / 2;

        boolean[] dp = new boolean[target + 1];
        dp[0] = true;

        for (int num : nums) {
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        return dp[target];
    }
}
```

---

## 📌 Problem 2: Unique Paths (LeetCode #62) — Grid DP
> **Problem:** A robot on an `m × n` grid starts at top-left. It can only move right or down. How many unique paths to the bottom-right?

### 💡 Each cell = paths from left + paths from above
`dp[i][j] = dp[i-1][j] + dp[i][j-1]`

#### 🐍 Python 3
```python
def uniquePaths(m: int, n: int) -> int:
    # Space-optimised: only keep one row
    dp = [1] * n   # first row is all 1s (only one way: go all right)
    for _ in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j - 1]   # dp[j] (from above) + dp[j-1] (from left)
    return dp[-1]
```

#### ☕ Java
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        java.util.Arrays.fill(dp, 1);
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[j] += dp[j - 1];
            }
        }
        return dp[n - 1];
    }
}
```

---

## 📌 Problem 3: Minimum Path Sum (LeetCode #64)
> **Problem:** Given a `m × n` grid of non-negative numbers, find the path from top-left to bottom-right that minimizes the sum. Only right and down moves allowed.

#### 🐍 Python 3
```python
def minPathSum(grid: list[list[int]]) -> int:
    m, n = len(grid), len(grid[0])
    # Build prefix sums in-place (modify grid directly to save space)
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                continue
            elif i == 0:
                grid[i][j] += grid[i][j - 1]   # first row: only from left
            elif j == 0:
                grid[i][j] += grid[i - 1][j]   # first col: only from above
            else:
                grid[i][j] += min(grid[i-1][j], grid[i][j-1])
    return grid[m-1][n-1]
```

#### ☕ Java
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) continue;
                else if (i == 0) grid[i][j] += grid[i][j-1];
                else if (j == 0) grid[i][j] += grid[i-1][j];
                else grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
            }
        }
        return grid[m-1][n-1];
    }
}
```

---

## 📌 Problem 4: Edit Distance (LeetCode #72) — Classic String DP
> **Problem:** Given two strings `word1` and `word2`, return the minimum number of operations (insert, delete, replace) to transform `word1` into `word2`.

### 💡 2D DP — Build up from empty strings

**Recurrence:**
- If `word1[i-1] == word2[j-1]`: `dp[i][j] = dp[i-1][j-1]` (free — no operation needed)
- Else: `dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`
  - `dp[i-1][j]` → delete from word1
  - `dp[i][j-1]` → insert into word1
  - `dp[i-1][j-1]` → replace

#### 🐍 Python 3
```python
def minDistance(word1: str, word2: str) -> int:
    m, n = len(word1), len(word2)
    # dp[i][j] = edit distance between word1[:i] and word2[:j]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # Base cases
    for i in range(m + 1): dp[i][0] = i  # delete all chars from word1
    for j in range(n + 1): dp[0][j] = j  # insert all chars of word2

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])

    return dp[m][n]
```

#### ☕ Java
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) dp[i][0] = i;
        for (int j = 0; j <= n; j++) dp[0][j] = j;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i-1) == word2.charAt(j-1)) dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = 1 + Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1]));
            }
        }
        return dp[m][n];
    }
}
```
