# 🧮 DSA — Day 12: Graphs — BFS, DFS & Core Algorithms
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 Graph Fundamentals

A graph is a set of **nodes (vertices)** connected by **edges**.

```
Adjacency List representation (most common for LeetCode):
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0, 4],
    3: [1],
    4: [2]
}
```

| Representation | Space | Best For |
|---|---|---|
| Adjacency List | $O(V + E)$ | Sparse graphs (most LC problems) |
| Adjacency Matrix | $O(V^2)$ | Dense graphs, fast edge lookup |

---

## 📌 BFS Template — Level-by-Level Traversal

BFS explores the nearest nodes first. It finds the **shortest path** in an unweighted graph.

```python
from collections import deque

def bfs(graph: dict, start: int) -> list[int]:
    visited = set([start])
    queue = deque([start])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return order
```

## 📌 DFS Template — Deep Exploration

DFS explores as far as possible before backtracking. Used for cycle detection, path finding, topological sort.

```python
def dfs(graph: dict, node: int, visited: set, result: list):
    visited.add(node)
    result.append(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited, result)
```

---

## 📌 Problem 1: Number of Islands (LeetCode #200)
> **Problem:** Given a 2D grid of `'1'`s (land) and `'0'`s (water), count the number of islands.

### 💡 DFS flood-fill — sink each visited land cell

#### 🐍 Python 3
```python
def numIslands(grid: list[list[str]]) -> int:
    if not grid:
        return 0
    rows, cols = len(grid), len(grid[0])
    count = 0

    def dfs(r: int, c: int):
        if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] != "1":
            return
        grid[r][c] = "#"   # sink to avoid revisiting
        dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1)

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1":
                dfs(r, c)
                count += 1
    return count
```

#### ☕ Java
```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for (int r = 0; r < grid.length; r++) {
            for (int c = 0; c < grid[0].length; c++) {
                if (grid[r][c] == '1') { dfs(grid, r, c); count++; }
            }
        }
        return count;
    }
    void dfs(char[][] grid, int r, int c) {
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length || grid[r][c] != '1') return;
        grid[r][c] = '#';
        dfs(grid, r+1, c); dfs(grid, r-1, c); dfs(grid, r, c+1); dfs(grid, r, c-1);
    }
}
```

---

## 📌 Problem 2: Clone Graph (LeetCode #133)
> **Problem:** Clone (deep copy) a connected undirected graph. Return the clone of the given node.

### 💡 BFS with a hash map: `original_node → cloned_node`

#### 🐍 Python 3
```python
from collections import deque

class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors or []

def cloneGraph(node: Node | None) -> Node | None:
    if not node:
        return None
    clones = {node: Node(node.val)}
    queue = deque([node])

    while queue:
        curr = queue.popleft()
        for neighbor in curr.neighbors:
            if neighbor not in clones:
                clones[neighbor] = Node(neighbor.val)
                queue.append(neighbor)
            clones[curr].neighbors.append(clones[neighbor])

    return clones[node]
```

---

## 📌 Problem 3: Course Schedule (LeetCode #207) — Cycle Detection
> **Problem:** There are `numCourses` courses. `prerequisites[i] = [a, b]` means you must take `b` before `a`. Can you finish all courses?

### 💡 Directed Graph Cycle Detection with DFS
3 states for each node: **0 = unvisited**, **1 = visiting (in current path)**, **2 = visited (done)**

#### 🐍 Python 3
```python
def canFinish(numCourses: int, prerequisites: list[list[int]]) -> bool:
    graph = {i: [] for i in range(numCourses)}
    for course, pre in prerequisites:
        graph[course].append(pre)

    state = [0] * numCourses  # 0=unvisited, 1=visiting, 2=done

    def dfs(node: int) -> bool:
        if state[node] == 1: return False  # cycle detected!
        if state[node] == 2: return True   # already fully explored
        state[node] = 1
        for neighbor in graph[node]:
            if not dfs(neighbor):
                return False
        state[node] = 2
        return True

    return all(dfs(i) for i in range(numCourses))
```

#### ☕ Java
```java
import java.util.*;

class Solution {
    int[] state;
    List<List<Integer>> graph;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        state = new int[numCourses];
        graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) graph.add(new ArrayList<>());
        for (int[] pre : prerequisites) graph.get(pre[0]).add(pre[1]);

        for (int i = 0; i < numCourses; i++) {
            if (!dfs(i)) return false;
        }
        return true;
    }

    boolean dfs(int node) {
        if (state[node] == 1) return false;
        if (state[node] == 2) return true;
        state[node] = 1;
        for (int neighbor : graph.get(node)) {
            if (!dfs(neighbor)) return false;
        }
        state[node] = 2;
        return true;
    }
}
```

---

## 📌 Problem 4: Shortest Path in Binary Matrix (LeetCode #1091)
> **Problem:** Find the shortest clear path from top-left to bottom-right in a binary matrix. Move in 8 directions. Return -1 if no path.

### 💡 BFS — guaranteed shortest path in unweighted grid

#### 🐍 Python 3
```python
from collections import deque

def shortestPathBinaryMatrix(grid: list[list[int]]) -> int:
    n = len(grid)
    if grid[0][0] == 1 or grid[n-1][n-1] == 1:
        return -1

    queue = deque([(0, 0, 1)])  # (row, col, path_length)
    grid[0][0] = 1              # mark visited by setting to 1

    directions = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]

    while queue:
        r, c, dist = queue.popleft()
        if r == n - 1 and c == n - 1:
            return dist
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < n and grid[nr][nc] == 0:
                grid[nr][nc] = 1   # mark visited
                queue.append((nr, nc, dist + 1))

    return -1
```
