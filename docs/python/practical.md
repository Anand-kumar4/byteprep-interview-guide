

# Practical Python Examples

This section contains real-world Python coding problems often asked in interviews or commonly encountered in data engineering tasks. Each example includes a problem statement and sample solution.

---

## Q1: Count the Frequency of Each Word in a String

**Problem:**  
Given a string, return a dictionary containing the frequency of each word.

**Sample Input:**
```python
text = "Data engineering is fun and data is powerful"
```

**Sample Output:**
```python
{'data': 2, 'engineering': 1, 'is': 2, 'fun': 1, 'and': 1, 'powerful': 1}
```

**Solution:**
```python
from collections import Counter

def word_count(text):
    words = text.lower().split()
    return dict(Counter(words))
```

---

## Q2: Merge Two Dictionaries Recursively

**Problem:**  
Write a function to recursively merge two dictionaries. If a key exists in both and the values are dictionaries, merge them recursively.

**Sample Input:**
```python
a = {'x': 1, 'y': {'z': 3}}
b = {'y': {'z': 5, 'w': 6}, 'k': 2}
```

**Sample Output:**
```python
{'x': 1, 'y': {'z': 5, 'w': 6}, 'k': 2}
```

**Solution:**
```python
def merge_dicts(a, b):
    result = a.copy()
    for key, value in b.items():
        if key in result and isinstance(result[key], dict) and isinstance(value, dict):
            result[key] = merge_dicts(result[key], value)
        else:
            result[key] = value
    return result
```

