---
title: Python Interview Questions
description: A curated list of Python coding questions including medium-level problems and string manipulation challenges, commonly asked in data engineering and software interviews.
keywords: Python, interview questions, coding problems, string algorithms, LeetCode, Python strings
---

This section presents hand-picked Python questions and solutions that are commonly asked in data engineering and software interviews. The focus is on practical coding patterns and medium-difficulty challenges.

# Python Interview Questions

## 🧠 Medium Complexity Problems

### Q1. Longest Substring Without Repeating Characters
**Problem:** Given a string, find the length of the longest substring without repeating characters.  
**Approach:** Use a sliding window and a hash set to keep track of characters in the current window.  
💡 **Key Insight:** Use a moving window to avoid repeating characters while tracking maximum length.  
**Code:**
<details>
<summary>Click to view solution</summary>

```python
def length_of_longest_substring(s: str) -> int:
    char_set = set()
    left = 0
    max_len = 0

    for right in range(len(s)):
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        char_set.add(s[right])
        max_len = max(max_len, right - left + 1)
    return max_len
```

</details>

**Example:**  
Input: `"abcabcbb"`  
Output: `3`

### Q2. 3Sum
**Problem:** Given an array `nums` of `n` integers, find all unique triplets in the array which gives the sum of zero.  
**Approach:** Sort the array and use a two-pointer approach inside a loop.  
💡 **Key Insight:** Sort the array and avoid duplicates using two pointers from both ends.  
**Code:**
<details>
<summary>Click to view solution</summary>

```python
def three_sum(nums: list[int]) -> list[list[int]]:
    nums.sort()
    res = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        left, right = i + 1, len(nums) - 1
        while left < right:
            s = nums[i] + nums[left] + nums[right]
            if s < 0:
                left += 1
            elif s > 0:
                right -= 1
            else:
                res.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                left += 1
                right -= 1
    return res
```

</details>

**Example:**  
Input: `[-1,0,1,2,-1,-4]`  
Output: `[[-1,-1,2],[-1,0,1]]`

## 🔤 String-Related Questions

### Q3. Reverse Words in a String
**Problem:** Given an input string `s`, reverse the order of the words.  
💡 **Key Insight:** Split, reverse, and rejoin with single spaces.  
**Code:**
<details>
<summary>Click to view solution</summary>

```python
def reverse_words(s: str) -> str:
    return " ".join(reversed(s.strip().split()))
```

</details>

**Example:**  
Input: `"  hello world  "`  
Output: `"world hello"`

### Q4. Valid Anagram
**Problem:** Given two strings `s` and `t`, return true if `t` is an anagram of `s`, and false otherwise.  
💡 **Key Insight:** Sorting both strings and comparing ensures character counts match.  
**Code:**
<details>
<summary>Click to view solution</summary>

```python
def is_anagram(s: str, t: str) -> bool:
    return sorted(s) == sorted(t)
```

</details>

**Example:**  
Input: `s = "anagram", t = "nagaram"`  
Output: `True`

[⬅️ Back to Python Overview](../index.md)