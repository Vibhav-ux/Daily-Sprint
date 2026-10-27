# 🧮 DSA — Day 10: Dynamic Programming — Memoization & Tabulation
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 The Mental Model: "Solve once, remember forever"

DP solves problems with **overlapping subproblems** and **optimal substructure**.

- **Overlapping Subproblems:** The same smaller problem is solved repeatedly.
- **Optimal Substructure:** The optimal solution is built from optimal solutions to subproblems.

### Two Approaches
| | Memoization (Top-Down) | Tabulation (Bottom-Up) |
|---|---|---|
| **Direction** | Recursive, start from target | Iterative, start from base case |
| **Style** | More natural to think about | More space-efficient |
| **Space** | Call stack + memo dict | Only the table |

---

## 📌 Problem 1: Climbing Stairs (LeetCode #70)
> **Problem:** You climb `n` stairs. Each time you can climb 1 or 2 steps. How many distinct ways can you reach the top?

### 💡 Key Insight: `ways(n) = ways(n-1) + ways(n-2)` — this is Fibonacci!

#### 🐍 Python — Memoization (Top-Down)
```python
from functools import lru_cache

def climbStairs(n: int) -> int:
    @lru_cache(maxsize=None)
    def dp(i: int) -> int:
        if i <= 1:
            return 1
        return dp(i - 1) + dp(i - 2)
    return dp(n)
```

#### 🐍 Python — Tabulation (Bottom-Up, $O(1)$ space)
```python
def climbStairs(n: int) -> int:
    if n <= 2:
        return n
    prev2, prev1 = 1, 2
    for _ in range(3, n + 1):
        prev2, prev1 = prev1, prev1 + prev2
    return prev1
```

#### ☕ Java
```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 2) return n;
        int prev2 = 1, prev1 = 2;
        for (int i = 3; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }
}
```

---

## 📌 Problem 2: House Robber (LeetCode #198)
> **Problem:** An array `nums` represents money in each house. You cannot rob two adjacent houses. Return the maximum amount you can rob.

### 💡 At each house, choose max between: skip this house, or rob it + best from 2 houses back.
`dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

#### 🐍 Python
```python
def rob(nums: list[int]) -> int:
    if len(nums) == 1:
        return nums[0]
    prev2, prev1 = 0, 0
    for num in nums:
        prev2, prev1 = prev1, max(prev1, prev2 + num)
    return prev1
```

#### ☕ Java
```java
class Solution {
    public int rob(int[] nums) {
        int prev2 = 0, prev1 = 0;
        for (int num : nums) {
            int curr = Math.max(prev1, prev2 + num);
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }
}
```

---

## 📌 Problem 3: Coin Change (LeetCode #322)
> **Problem:** Given coin denominations and a target amount, return the minimum number of coins needed. Return -1 if impossible.

### 💡 Classic Unbounded Knapsack DP
`dp[amount] = min(dp[amount - coin] + 1)` for all valid coins.

#### 🐍 Python
```python
def coinChange(coins: list[int], amount: int) -> int:
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0   # base case: 0 coins needed for amount 0
    for a in range(1, amount + 1):
        for coin in coins:
            if a - coin >= 0:
                dp[a] = min(dp[a], dp[a - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```

#### ☕ Java
```java
import java.util.Arrays;

class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);   // sentinel value (impossible)
        dp[0] = 0;
        for (int a = 1; a <= amount; a++) {
            for (int coin : coins) {
                if (a - coin >= 0) {
                    dp[a] = Math.min(dp[a], dp[a - coin] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

---

## 📌 Problem 4: Longest Common Subsequence (LeetCode #1143)
> **Problem:** Given two strings `text1` and `text2`, return the length of their longest common subsequence.

### 💡 2D DP Table
```
       ""  a  c  e
  ""    0  0  0  0
  a     0  1  1  1
  b     0  1  1  1
  c     0  1  2  2
  e     0  1  2  3
```
- If characters match: `dp[i][j] = dp[i-1][j-1] + 1`
- Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`

#### 🐍 Python
```python
def longestCommonSubsequence(text1: str, text2: str) -> int:
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[m][n]
```

#### ☕ Java
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return dp[m][n];
    }
}
```

---

## 🧠 DP Pattern Recognition Cheat Sheet

| Problem Type | Recurrence Clue | Classic Problems |
|---|---|---|
| **1D Linear** | `dp[i]` depends on `dp[i-1]`, `dp[i-2]` | Fibonacci, Climbing Stairs, House Robber |
| **Unbounded Knapsack** | "Use coin/item unlimited times" | Coin Change, Rod Cutting |
| **0/1 Knapsack** | "Use each item once" | Subset Sum, Partition Equal Subset |
| **2D String DP** | Match characters of two strings | LCS, Edit Distance |
| **Grid DP** | Move right/down in a grid | Unique Paths, Min Path Sum |
