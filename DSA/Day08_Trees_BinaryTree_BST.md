# 🧮 DSA — Day 08: Trees — Binary Trees & BST
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 Tree Vocabulary

```
        1         ← root (depth 0)
       / \
      2   3       ← internal nodes (depth 1)
     / \    \
    4   5    6    ← leaf nodes (depth 2)
```

- **Root** → top node (no parent)
- **Leaf** → node with no children
- **Height** → longest path from root to a leaf
- **Depth** → distance from root to a node
- **BST (Binary Search Tree)** → left child < parent < right child

### Python Node Definition
```python
class TreeNode:
    def __init__(self, val: int = 0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

---

## 📌 The Three Traversal Orders (Know These Cold)

```python
def inorder(root: TreeNode | None) -> list[int]:
    # Left → Root → Right (gives sorted output for BST!)
    if not root: return []
    return inorder(root.left) + [root.val] + inorder(root.right)

def preorder(root: TreeNode | None) -> list[int]:
    # Root → Left → Right (used to clone/serialize trees)
    if not root: return []
    return [root.val] + preorder(root.left) + preorder(root.right)

def postorder(root: TreeNode | None) -> list[int]:
    # Left → Right → Root (used to delete trees, evaluate expressions)
    if not root: return []
    return postorder(root.left) + postorder(root.right) + [root.val]
```

---

## 📌 Problem 1: Maximum Depth of Binary Tree (LeetCode #104)
> **Problem:** Find the maximum depth (height) of a binary tree.

### 💡 Recursive DFS — $O(N)$ Time, $O(H)$ Space

#### 🐍 Python 3
```python
def maxDepth(root: TreeNode | None) -> int:
    if not root:
        return 0
    left_depth  = maxDepth(root.left)
    right_depth = maxDepth(root.right)
    return 1 + max(left_depth, right_depth)
```

#### ☕ Java
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}
```

---

## 📌 Problem 2: Invert Binary Tree (LeetCode #226)
> **Problem:** Invert (mirror) a binary tree.

#### 🐍 Python 3
```python
def invertTree(root: TreeNode | None) -> TreeNode | None:
    if not root:
        return None
    root.left, root.right = invertTree(root.right), invertTree(root.left)
    return root
```

#### ☕ Java
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);
        return root;
    }
}
```

---

## 📌 Problem 3: Level Order Traversal / BFS (LeetCode #102)
> **Problem:** Return node values level by level (BFS order).

### 💡 BFS with Queue — $O(N)$ Time, $O(N)$ Space

#### 🐍 Python 3
```python
from collections import deque

def levelOrder(root: TreeNode | None) -> list[list[int]]:
    if not root:
        return []
    result = []
    queue = deque([root])
    while queue:
        level_size = len(queue)
        level_values = []
        for _ in range(level_size):
            node = queue.popleft()
            level_values.append(node.val)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level_values)
    return result
```

#### ☕ Java
```java
import java.util.*;

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left  != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            result.add(level);
        }
        return result;
    }
}
```

---

## 📌 Problem 4: Validate BST (LeetCode #98)
> **Problem:** Determine if a binary tree is a valid Binary Search Tree.

### 💡 Pass Min/Max Bounds Down the Recursion — $O(N)$ Time

#### 🐍 Python 3
```python
def isValidBST(root: TreeNode | None) -> bool:
    def validate(node, min_val: float, max_val: float) -> bool:
        if not node:
            return True
        if not (min_val < node.val < max_val):
            return False
        return (validate(node.left, min_val, node.val) and
                validate(node.right, node.val, max_val))

    return validate(root, float('-inf'), float('inf'))
```

#### ☕ Java
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    private boolean validate(TreeNode node, long min, long max) {
        if (node == null) return true;
        if (node.val <= min || node.val >= max) return false;
        return validate(node.left, min, node.val) && validate(node.right, node.val, max);
    }
}
```
