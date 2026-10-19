# 🧮 DSA — Day 09: Recursion & Backtracking
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 The Mental Model: Decision Trees

Recursion means solving a problem by **breaking it into the same smaller problem**.
Backtracking means exploring every path in a decision tree and **undoing (backtracking) when a path fails**.

```
                    [ ]
                  /     \
              [1]         [2]
             /   \       /   \
          [1,2] [1,3] [2,1] [2,3]
```

**Template:**
```python
def backtrack(state, choices):
    if base_case(state):
        add_to_result(state)
        return
    for choice in choices:
        make_choice(choice, state)
        backtrack(state, remaining_choices)
        undo_choice(choice, state)   # ← THIS is "backtracking"
```

---

## 📌 Problem 1: Subsets (LeetCode #78)
> **Problem:** Given an integer array of unique elements, return all possible subsets (the power set).

#### 🐍 Python 3
```python
def subsets(nums: list[int]) -> list[list[int]]:
    result = []

    def backtrack(start: int, current: list[int]):
        result.append(current[:])   # snapshot of current subset
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()           # backtrack

    backtrack(0, [])
    return result

# subsets([1,2,3]) → [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

#### ☕ Java
```java
import java.util.*;

class Solution {
    List<List<Integer>> result = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        backtrack(nums, 0, new ArrayList<>());
        return result;
    }

    void backtrack(int[] nums, int start, List<Integer> current) {
        result.add(new ArrayList<>(current));
        for (int i = start; i < nums.length; i++) {
            current.add(nums[i]);
            backtrack(nums, i + 1, current);
            current.remove(current.size() - 1);
        }
    }
}
```

---

## 📌 Problem 2: Combinations (LeetCode #77)
> **Problem:** Given two integers n and k, return all possible combinations of k numbers from 1 to n.

#### 🐍 Python 3
```python
def combine(n: int, k: int) -> list[list[int]]:
    result = []

    def backtrack(start: int, current: list[int]):
        if len(current) == k:
            result.append(current[:])
            return
        for i in range(start, n + 1):
            current.append(i)
            backtrack(i + 1, current)
            current.pop()

    backtrack(1, [])
    return result
```

---

## 📌 Problem 3: Permutations (LeetCode #46)
> **Problem:** Given an array of distinct integers, return all possible permutations.

### 💡 Track which elements are "used" with a boolean visited array

#### 🐍 Python 3
```python
def permute(nums: list[int]) -> list[list[int]]:
    result = []
    used = [False] * len(nums)

    def backtrack(current: list[int]):
        if len(current) == len(nums):
            result.append(current[:])
            return
        for i, num in enumerate(nums):
            if not used[i]:
                used[i] = True
                current.append(num)
                backtrack(current)
                current.pop()
                used[i] = False

    backtrack([])
    return result
```

#### ☕ Java
```java
import java.util.*;

class Solution {
    List<List<Integer>> result = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        backtrack(nums, new boolean[nums.length], new ArrayList<>());
        return result;
    }

    void backtrack(int[] nums, boolean[] used, List<Integer> current) {
        if (current.size() == nums.length) { result.add(new ArrayList<>(current)); return; }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            current.add(nums[i]);
            backtrack(nums, used, current);
            current.remove(current.size() - 1);
            used[i] = false;
        }
    }
}
```

---

## 📌 Problem 4: Word Search (LeetCode #79)
> **Problem:** Given a 2D board and a word, find if the word exists in the grid by tracing horizontally/vertically adjacent cells.

### 💡 DFS + Backtracking on a Grid

#### 🐍 Python 3
```python
def exist(board: list[list[str]], word: str) -> bool:
    rows, cols = len(board), len(board[0])

    def dfs(r: int, c: int, idx: int) -> bool:
        if idx == len(word):
            return True
        if r < 0 or r >= rows or c < 0 or c >= cols or board[r][c] != word[idx]:
            return False
        temp = board[r][c]
        board[r][c] = "#"    # mark as visited (backtrack marker)
        found = (dfs(r+1, c, idx+1) or dfs(r-1, c, idx+1) or
                 dfs(r, c+1, idx+1) or dfs(r, c-1, idx+1))
        board[r][c] = temp   # restore (backtrack)
        return found

    for r in range(rows):
        for c in range(cols):
            if dfs(r, c, 0):
                return True
    return False
```
