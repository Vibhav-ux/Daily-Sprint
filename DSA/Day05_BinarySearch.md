# 🧮 DSA — Day 05: Binary Search Pattern
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 The Core Intuition: Eliminate Half the Search Space

Binary search works **only on sorted arrays or ranges with a monotonic condition**.
At every step, you check the midpoint and eliminate the half that cannot contain your answer.

**Time complexity:** $O(\log N)$ — Searching 1,000,000 elements takes only ~20 comparisons!

---

## Template — Standard Binary Search

```python
def binary_search_template(nums: list[int], target: int) -> int:
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2  # avoids integer overflow (Java-safe pattern)
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1     # target is in right half
        else:
            right = mid - 1    # target is in left half
    return -1   # not found
```

> **Critical detail:** Use `mid = left + (right - left) // 2`, NOT `mid = (left + right) // 2`.
> In Java/C++ the second form can cause integer overflow if `left + right > Integer.MAX_VALUE`.

---

## 📌 Problem 1: Binary Search (LeetCode #704)
> **Problem:** Given a sorted array of distinct integers and a target value, return the index if found, else return -1.

#### 🐍 Python 3
```python
def search(nums: list[int], target: int) -> int:
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

#### ☕ Java
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] < target) left = mid + 1;
            else right = mid - 1;
        }
        return -1;
    }
}
```

---

## 📌 Problem 2: Search in Rotated Sorted Array (LeetCode #33)
> **Problem:** Sorted array was rotated at some pivot unknown to you. Find target in $O(\log N)$.

### 💡 Key Insight
At midpoint, **one half is always sorted**. Check which half, then determine if target is in it.

#### 🐍 Python 3
```python
def search(nums: list[int], target: int) -> int:
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        # Left half is sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1   # target in left half
            else:
                left = mid + 1    # target in right half
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1    # target in right half
            else:
                right = mid - 1   # target in left half
    return -1
```

#### ☕ Java
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            if (nums[left] <= nums[mid]) {
                if (nums[left] <= target && target < nums[mid]) right = mid - 1;
                else left = mid + 1;
            } else {
                if (nums[mid] < target && target <= nums[right]) left = mid + 1;
                else right = mid - 1;
            }
        }
        return -1;
    }
}
```

---

## 📌 Problem 3: Find Minimum in Rotated Sorted Array (LeetCode #153)
> **Problem:** Find the minimum element in a rotated sorted array.

### 💡 Key Insight
The minimum is at the inflection point (where array stops being ascending).
If `nums[mid] > nums[right]`, the minimum is in the right half.

#### 🐍 Python 3
```python
def findMin(nums: list[int]) -> int:
    left, right = 0, len(nums) - 1
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] > nums[right]:
            left = mid + 1    # min is in right half
        else:
            right = mid       # mid could itself be the min
    return nums[left]
```

#### ☕ Java
```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) left = mid + 1;
            else right = mid;
        }
        return nums[left];
    }
}
```

---

## 📌 Problem 4: Find Peak Element (LeetCode #162)
> **Problem:** Find any peak element (greater than its neighbors). Return its index.

### 💡 Key Insight
If `nums[mid] < nums[mid + 1]`, slope goes up → peak is to the right.
Otherwise, slope goes down → peak is at or to the left of mid.

#### 🐍 Python 3
```python
def findPeakElement(nums: list[int]) -> int:
    left, right = 0, len(nums) - 1
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] < nums[mid + 1]:
            left = mid + 1
        else:
            right = mid
    return left
```

#### ☕ Java
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < nums[mid + 1]) left = mid + 1;
            else right = mid;
        }
        return left;
    }
}
```

---

## 🧠 Binary Search Mental Model Summary

| Condition | Action | Why |
|---|---|---|
| `nums[mid] == target` | Return mid | Found! |
| `nums[mid] < target` | `left = mid + 1` | Go right |
| `nums[mid] > target` | `right = mid - 1` | Go left |
| `nums[mid] > nums[right]` | `left = mid + 1` | Min is in right half |
| `nums[mid] < nums[mid+1]` | `left = mid + 1` | Peak is in right half |
