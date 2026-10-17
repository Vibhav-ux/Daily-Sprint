# 🧮 DSA — Day 02: Two Pointers Pattern
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 Problem 1: Valid Palindrome (LeetCode #125)
> **Problem:** Determine if a string is a palindrome after converting all uppercase letters into lowercase and removing all non-alphanumeric characters.

### 💡 Optimal Pattern: Inward Two Pointers ($O(N)$ Time, $O(1)$ Space)
Place `left = 0` and `right = len - 1`. Advance past non-alphanumerics, compare characters.

#### 🐍 Python 3 Solution
```python
def isPalindrome(s: str) -> bool:
    left, right = 0, len(s) - 1
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        if s[left].lower() != s[right].lower():
            return False
        left += 1
        right -= 1
    return True
```

#### ☕ Java Solution
```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;
        while (left < right) {
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

---

## 📌 Problem 2: Two Sum II — Sorted Array (LeetCode #167)
> **Problem:** Array is already sorted in non-decreasing order. Find two numbers such that they add up to `target`. Return 1-based indices.

### 💡 Optimal Pattern: Two Pointers ($O(N)$ Time, $O(1)$ Space)
Because the array is sorted:
- If `sum < target`, increment `left` to increase sum.
- If `sum > target`, decrement `right` to decrease sum.

#### 🐍 Python 3 Solution
```python
def twoSum(numbers: list[int], target: int) -> list[int]:
    left, right = 0, len(numbers) - 1
    while left < right:
        current_sum = numbers[left] + numbers[right]
        if current_sum == target:
            return [left + 1, right + 1]  # 1-indexed
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return []
```

#### ☕ Java Solution
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0, right = numbers.length - 1;
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) {
                return new int[] { left + 1, right + 1 };
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        return new int[0];
    }
}
```

---

## 📌 Problem 3: Container With Most Water (LeetCode #11)
> **Problem:** Find two vertical lines that together with the x-axis form a container that contains the most water.

### 💡 Optimal Pattern: Greedy Inward Two Pointers ($O(N)$ Time, $O(1)$ Space)
The area is limited by `min(height[left], height[right]) * (right - left)`.
Always shift the pointer with the smaller height, since shifting the taller one can never increase area.

#### 🐍 Python 3 Solution
```python
def maxArea(height: list[int]) -> int:
    left, right = 0, len(height) - 1
    max_water = 0
    while left < right:
        width = right - left
        current_water = min(height[left], height[right]) * width
        max_water = max(max_water, current_water)
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    return max_water
```

#### ☕ Java Solution
```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1;
        int maxWater = 0;
        while (left < right) {
            int width = right - left;
            int currentWater = Math.min(height[left], height[right]) * width;
            maxWater = Math.max(maxWater, currentWater);
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return maxWater;
    }
}
```
