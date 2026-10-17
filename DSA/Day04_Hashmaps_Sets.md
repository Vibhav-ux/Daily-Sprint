# 🧮 DSA — Day 04: Hashmaps & Sets Pattern
### LeetCode Core Patterns | Dual Language Solutions (Python 3 + Java)

---

## 📌 Problem 1: Valid Anagram (LeetCode #242)
> **Problem:** Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

### 💡 Optimal Pattern: Frequency Counting ($O(N)$ Time, $O(1)$ Space for fixed alphabet)
Count character frequencies. An anagram will have identical character counts.

#### 🐍 Python 3 Solution
```python
def isAnagram(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False
    counts = {}
    for c1, c2 in zip(s, t):
        counts[c1] = counts.get(c1, 0) + 1
        counts[c2] = counts.get(c2, 0) - 1
    return all(v == 0 for v in counts.values())
```

#### ☕ Java Solution
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;
        int[] counts = new int[26];
        for (int i = 0; i < s.length(); i++) {
            counts[s.charAt(i) - 'a']++;
            counts[t.charAt(i) - 'a']--;
        }
        for (int c : counts) {
            if (c != 0) return false;
        }
        return true;
    }
}
```

---

## 📌 Problem 2: Group Anagrams (LeetCode #49)
> **Problem:** Given an array of strings `strs`, group the anagrams together.

### 💡 Optimal Pattern: Hash Map with Canonical Key ($O(N \cdot K \log K)$ or $O(N \cdot K)$ Time)
Two strings are anagrams if and only if their sorted versions are identical. Use the sorted string as the key in a hash map.

#### 🐍 Python 3 Solution
```python
from collections import defaultdict

def groupAnagrams(strs: list[str]) -> list[list[str]]:
    groups = defaultdict(list)
    for word in strs:
        key = "".join(sorted(word))
        groups[key].append(word)
    return list(groups.values())
```

#### ☕ Java Solution
```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> groups = new HashMap<>();
        for (String word : strs) {
            char[] chars = word.toCharArray();
            Arrays.sort(chars);
            String key = new String(chars);
            groups.computeIfAbsent(key, k -> new ArrayList<>()).add(word);
        }
        return new ArrayList<>(groups.values());
    }
}
```

---

## 📌 Problem 3: Longest Consecutive Sequence (LeetCode #128)
> **Problem:** Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence in $O(N)$ time.

### 💡 Optimal Pattern: Hash Set Starting Point ($O(N)$ Time, $O(N)$ Space)
Insert all numbers into a set. A number `x` is the start of a sequence only if `x - 1` is NOT in the set! If it is a sequence start, count forward `x + 1, x + 2...` until break.

#### 🐍 Python 3 Solution
```python
def longestConsecutive(nums: list[int]) -> int:
    num_set = set(nums)
    longest = 0
    for num in num_set:
        # Only start counting if num is the beginning of a sequence
        if (num - 1) not in num_set:
            current_num = num
            current_streak = 1
            while (current_num + 1) in num_set:
                current_num += 1
                current_streak += 1
            longest = max(longest, current_streak)
    return longest
```

#### ☕ Java Solution
```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) set.add(num);
        
        int longest = 0;
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;
                while (set.contains(currentNum + 1)) {
                    currentNum++;
                    currentStreak++;
                }
                longest = Math.max(longest, currentStreak);
            }
        }
        return longest;
    }
}
```
