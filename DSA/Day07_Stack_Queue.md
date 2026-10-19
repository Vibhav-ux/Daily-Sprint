# 🧮 DSA — Day 07: Stack & Queue
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 Stack — LIFO (Last In, First Out)

Think of a stack of plates. You add to the top, you remove from the top.
- `push(val)` → add to top — $O(1)$
- `pop()` → remove from top — $O(1)$
- `peek()` → look at top without removing — $O(1)$

**In Python:** Use a `list` as a stack (`append` = push, `pop()` = pop from top).
**In Java:** Use `Deque<Integer> stack = new ArrayDeque<>()`.

---

## 📌 Problem 1: Valid Parentheses (LeetCode #20)
> **Problem:** Given a string of `(`, `)`, `{`, `}`, `[`, `]`, determine if the brackets are correctly matched and nested.

### 💡 Stack Pattern: Push openers, match closers — $O(N)$ Time, $O(N)$ Space

#### 🐍 Python 3
```python
def isValid(s: str) -> bool:
    stack = []
    pairs = {")": "(", "}": "{", "]": "["}
    for ch in s:
        if ch in "({[":
            stack.append(ch)
        else:
            if not stack or stack[-1] != pairs[ch]:
                return False
            stack.pop()
    return len(stack) == 0
```

#### ☕ Java
```java
import java.util.*;

class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char ch : s.toCharArray()) {
            if (ch == '(' || ch == '{' || ch == '[') {
                stack.push(ch);
            } else {
                if (stack.isEmpty()) return false;
                char top = stack.pop();
                if ((ch == ')' && top != '(') ||
                    (ch == '}' && top != '{') ||
                    (ch == ']' && top != '[')) return false;
            }
        }
        return stack.isEmpty();
    }
}
```

---

## 📌 Problem 2: Min Stack (LeetCode #155)
> **Problem:** Design a stack that supports `push`, `pop`, `top`, and `getMin` — all in $O(1)$.

### 💡 Dual Stack: Main stack + "minimum tracker" stack

#### 🐍 Python 3
```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []   # tracks min at each level

    def push(self, val: int) -> None:
        self.stack.append(val)
        current_min = min(val, self.min_stack[-1] if self.min_stack else val)
        self.min_stack.append(current_min)

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```

#### ☕ Java
```java
class MinStack {
    private Deque<Integer> stack = new ArrayDeque<>();
    private Deque<Integer> minStack = new ArrayDeque<>();

    public void push(int val) {
        stack.push(val);
        int currentMin = minStack.isEmpty() ? val : Math.min(val, minStack.peek());
        minStack.push(currentMin);
    }
    public void pop() { stack.pop(); minStack.pop(); }
    public int top() { return stack.peek(); }
    public int getMin() { return minStack.peek(); }
}
```

---

## 📌 Problem 3: Daily Temperatures (LeetCode #739)
> **Problem:** Given an array of daily temperatures, return an array where `result[i]` is the number of days until a warmer temperature. If there's no future warmer day, `result[i] = 0`.

### 💡 Monotonic Decreasing Stack — $O(N)$ Time, $O(N)$ Space

Push indices onto the stack. When current temp is warmer than the temp at stack top, pop and record the gap.

#### 🐍 Python 3
```python
def dailyTemperatures(temperatures: list[int]) -> list[int]:
    result = [0] * len(temperatures)
    stack = []   # stores indices; temps decrease from bottom to top
    for i, temp in enumerate(temperatures):
        while stack and temperatures[stack[-1]] < temp:
            prev_idx = stack.pop()
            result[prev_idx] = i - prev_idx
        stack.append(i)
    return result
```

#### ☕ Java
```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] result = new int[n];
        Deque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
                int prevIdx = stack.pop();
                result[prevIdx] = i - prevIdx;
            }
            stack.push(i);
        }
        return result;
    }
}
```

---

## 📌 Problem 4: Evaluate Reverse Polish Notation (LeetCode #150)
> **Problem:** Evaluate an expression in Reverse Polish Notation (postfix notation).
> Example: `["2","1","+","3","*"]` → `(2 + 1) * 3 = 9`

#### 🐍 Python 3
```python
def evalRPN(tokens: list[str]) -> int:
    stack = []
    ops = {
        "+": lambda a, b: a + b,
        "-": lambda a, b: a - b,
        "*": lambda a, b: a * b,
        "/": lambda a, b: int(a / b),   # truncates towards zero
    }
    for token in tokens:
        if token in ops:
            b, a = stack.pop(), stack.pop()
            stack.append(ops[token](a, b))
        else:
            stack.append(int(token))
    return stack[0]
```

#### ☕ Java
```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stack = new ArrayDeque<>();
        for (String token : tokens) {
            if ("+-*/".contains(token)) {
                int b = stack.pop(), a = stack.pop();
                switch (token) {
                    case "+" -> stack.push(a + b);
                    case "-" -> stack.push(a - b);
                    case "*" -> stack.push(a * b);
                    case "/" -> stack.push(a / b);
                }
            } else {
                stack.push(Integer.parseInt(token));
            }
        }
        return stack.pop();
    }
}
```
