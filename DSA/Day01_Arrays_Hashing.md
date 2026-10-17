# 🧮 DSA — Day 01: Arrays & Hashing
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 Problem 1: Two Sum (LeetCode #1)
> **Problem:** Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

### 💡 Optimal Pattern: Hash Map Complement ($O(N)$ Time, $O(N)$ Space)
Instead of checking all pairs with a nested loop ($O(N^2)$), compute `need = target - num`. If `need` was already stored in our hash map, we found our pair!

#### 🐍 Python 3 Solution
```python
def twoSum(nums: list[int], target: int) -> list[int]:
    seen = {}  # Map: value -> index
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []
```

#### ☕ Java Solution
```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> seen = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (seen.containsKey(complement)) {
                return new int[] { seen.get(complement), i };
            }
            seen.put(nums[i], i);
        }
        return new int[0];
    }
}
```

---

## 📌 Problem 2: Contains Duplicate (LeetCode #217)
> **Problem:** Return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

### 💡 Optimal Pattern: Hash Set ($O(N)$ Time, $O(N)$ Space)
Insert elements into a set. If an element already exists, duplicate detected.

#### 🐍 Python 3 Solution
```python
def containsDuplicate(nums: list[int]) -> bool:
    seen = set()
    for num in nums:
        if num in seen:
            return True
        seen.add(num)
    return False

# One-liner Python alternative:
# return len(nums) != len(set(nums))
```

#### ☕ Java Solution
```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        for (int num : nums) {
            if (!seen.add(num)) {
                return true; // add returns false if item already exists
            }
        }
        return false;
    }
}
```

---

## 📌 Problem 3: Best Time to Buy and Sell Stock (LeetCode #121)
> **Problem:** Maximize profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

### 💡 Optimal Pattern: Single Pass Greedy ($O(N)$ Time, $O(1)$ Space)
Track the minimum price seen so far, and at each step calculate profit if sold today.

#### 🐍 Python 3 Solution
```python
def maxProfit(prices: list[int]) -> int:
    min_price = float('inf')
    max_profit = 0
    for price in prices:
        if price < min_price:
            min_price = price
        else:
            max_profit = max(max_profit, price - min_price)
    return max_profit
```

#### ☕ Java Solution
```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;
        for (int price : prices) {
            if (price < minPrice) {
                minPrice = price;
            } else {
                maxProfit = Math.max(maxProfit, price - minPrice);
            }
        }
        return maxProfit;
    }
}
```

---

## 📌 Problem 4: Maximum Subarray — Kadane's Algorithm (LeetCode #53)
> **Problem:** Find the subarray with the largest sum and return its sum.

### 💡 Optimal Pattern: Kadane's Algorithm ($O(N)$ Time, $O(1)$ Space)
At index `i`, decide: start fresh at `nums[i]` or extend the previous running subarray sum?

#### 🐍 Python 3 Solution
```python
def maxSubArray(nums: list[int]) -> int:
    current_sum = nums[0]
    max_sum = nums[0]
    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    return max_sum
```

#### ☕ Java Solution
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int currentSum = nums[0];
        int maxSum = nums[0];
        for (int i = 1; i < nums.length; i++) {
            currentSum = Math.max(nums[i], currentSum + nums[i]);
            maxSum = Math.max(maxSum, currentSum);
        }
        return maxSum;
    }
}
```
