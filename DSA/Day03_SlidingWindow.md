# 🧮 DSA — Day 03: Sliding Window Pattern
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 Problem 1: Maximum Sum Subarray of Size K (Fixed Window)
> **Problem:** Given an array of integers and a number $k$, find the maximum sum of any contiguous subarray of size $k$.

### 💡 Optimal Pattern: Fixed Sliding Window ($O(N)$ Time, $O(1)$ Space)
Instead of re-summing $k$ elements each time, subtract element leaving window and add element entering window.

#### 🐍 Python 3 Solution
```python
def maxSumSubarray(nums: list[int], k: int) -> int:
    if len(nums) < k:
        return 0
    window_sum = sum(nums[:k])
    max_sum = window_sum
    for i in range(k, len(nums)):
        window_sum += nums[i] - nums[i - k]
        max_sum = max(max_sum, window_sum)
    return max_sum
```

#### ☕ Java Solution
```java
class Solution {
    public int maxSumSubarray(int[] nums, int k) {
        if (nums.length < k) return 0;
        int windowSum = 0;
        for (int i = 0; i < k; i++) {
            windowSum += nums[i];
        }
        int maxSum = windowSum;
        for (int i = k; i < nums.length; i++) {
            windowSum += nums[i] - nums[i - k];
            maxSum = Math.max(maxSum, windowSum);
        }
        return maxSum;
    }
}
```

---

## 📌 Problem 2: Longest Substring Without Repeating Characters (LeetCode #3)
> **Problem:** Find the length of the longest substring without duplicate characters.

### 💡 Optimal Pattern: Dynamic Sliding Window + Last Seen Map ($O(N)$ Time, $O(\min(N, M))$ Space)
Keep track of the last index where each character was seen. If duplicate is within current window `[left, right]`, jump `left` to `last_seen[ch] + 1`.

#### 🐍 Python 3 Solution
```python
def lengthOfLongestSubstring(s: str) -> int:
    last_seen = {}
    left = 0
    max_len = 0
    for right, ch in enumerate(s):
        if ch in last_seen and last_seen[ch] >= left:
            left = last_seen[ch] + 1
        last_seen[ch] = right
        max_len = max(max_len, right - left + 1)
    return max_len
```

#### ☕ Java Solution
```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> lastSeen = new HashMap<>();
        int left = 0;
        int maxLen = 0;
        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);
            if (lastSeen.containsKey(ch) && lastSeen.get(ch) >= left) {
                left = lastSeen.get(ch) + 1;
            }
            lastSeen.put(ch, right);
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }
}
```
