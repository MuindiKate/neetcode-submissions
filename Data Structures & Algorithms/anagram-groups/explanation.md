# Group Anagrams

## Problem

Given an array of strings `strs`, group all anagrams together into sublists.

You may return the groups in any order.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

### Example 1

```python
Input: strs = ["act","pots","tops","cat","stop","hat"]

Output: [["hat"],["act","cat"],["stop","pots","tops"]]
```

### Example 2

```python
Input: strs = ["x"]

Output: [["x"]]
```

### Example 3

```python
Input: strs = [""]

Output: [[""]]
```

### Constraints

* `1 <= strs.length <= 1000`
* `0 <= strs[i].length <= 100`
* `strs[i]` consists of lowercase English letters

---

# Solution 1: Sorting

## Intuition

Anagrams contain the same characters.

If we sort the characters of every string, all anagrams will produce the same result.

For example:

```python
"act"  -> "act"
"cat"  -> "act"
```

Since both strings produce the same sorted version, they belong in the same group.

We can use the sorted string as a key in a hash map and store all matching strings together.

## Algorithm

1. Create a hash map.
2. Iterate through each string.
3. Sort the characters in the string.
4. Use the sorted string as the key.
5. Append the original string to the corresponding list.
6. Return all grouped values.

## Code

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)

        for s in strs:
            sortedS = ''.join(sorted(s))
            res[sortedS].append(s)

        return list(res.values())
```

## Dry Run

```python
strs = ["act","cat","hat"]
```

Process:

```python
"act" -> "act"
res = {
    "act": ["act"]
}

"cat" -> "act"
res = {
    "act": ["act", "cat"]
}

"hat" -> "aht"
res = {
    "act": ["act", "cat"],
    "aht": ["hat"]
}
```

Final output:

```python
[
    ["act", "cat"],
    ["hat"]
]
```

## Complexity Analysis

### Time Complexity: O(m × n log n)

* `m` = number of strings
* `n` = length of the longest string

Each string must be sorted.

```text
O(m × n log n)
```

### Space Complexity: O(m × n)

The hash map stores all strings.

```text
O(m × n)
```

## Pros

* Easy to understand.
* Very common interview solution.

## Cons

* Sorting every string adds extra cost.

---

# Solution 2: Character Frequency Hash Map

## Intuition

Instead of sorting each string, we can count how many times each character appears.

Since the problem only contains lowercase English letters, we only need an array of size 26.

Example:

```python
"act"

a = 1
c = 1
t = 1
```

```python
"cat"

a = 1
c = 1
t = 1
```

Both strings generate the exact same frequency array, meaning they belong in the same group.

Because lists cannot be dictionary keys, we convert the frequency array into a tuple.

## Algorithm

1. Create a hash map.
2. For each string:

   * Create a frequency array of size 26.
   * Count each character.
   * Convert the array into a tuple.
3. Use the tuple as the hash map key.
4. Append the string to its group.
5. Return all groups.

## Code

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)

        for s in strs:
            count = [0] * 26

            for c in s:
                count[ord(c) - ord('a')] += 1

            res[tuple(count)].append(s)

        return list(res.values())
```

## Explanation

The frequency array acts as a unique fingerprint for every anagram group.

For example:

```python
"act"
```

Produces:

```python
[1,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0]
```

And:

```python
"cat"
```

Produces the exact same array.

After converting the array to a tuple:

```python
(1,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0)
```

Both strings map to the same key and are grouped together.

## Dry Run

```python
strs = ["act","cat","hat"]
```

Process:

```python
"act"
key = (1,0,1,...,1,...)
res = {
    key: ["act"]
}

"cat"
same key
res = {
    key: ["act","cat"]
}

"hat"
different key
res = {
    key1: ["act","cat"],
    key2: ["hat"]
}
```

Output:

```python
[
    ["act","cat"],
    ["hat"]
]
```

## Complexity Analysis

### Time Complexity: O(m × n)

* We scan every character once.
* No sorting is required.

```text
O(m × n)
```

### Space Complexity: O(m)

Auxiliary hash map storage.

If output is counted:

```text
O(m × n)
```

## Pros

* Faster than sorting.
* Linear time solution.
* Most efficient approach for this problem.

## Cons

* Less intuitive than sorting.
* Relies on the lowercase English letters constraint.

---

# Comparison of Solutions

| Approach           | Time Complexity | Space Complexity | Recommended |
| ------------------ | --------------- | ---------------- | ----------- |
| Sorting            | O(m × n log n)  | O(m × n)         | Good        |
| Frequency Hash Map | O(m × n)        | O(m) auxiliary   | Best        |

---

# Common Pitfalls

## 1. Using a List as a Dictionary Key

Wrong:

```python
count = [0] * 26
res[count].append(s)
```

Lists are mutable and therefore unhashable.

Correct:

```python
res[tuple(count)].append(s)
```

---

## 2. Forgetting the Lowercase Constraint

The frequency-array solution assumes:

```python
'a' <= c <= 'z'
```

If uppercase or special characters are allowed, the indexing logic must change.

---

## 3. Creating Keys That Can Collide

Wrong:

```python
key = ''.join(str(c) for c in count)
```

Example:

```python
[1,11]
```

and

```python
[11,1]
```

Both become:

```python
"111"
```

Correct:

```python
tuple(count)
```

or

```python
','.join(str(c) for c in count)
```

---

# Most Efficient Solution

The **Character Frequency Hash Map** approach is optimal because:

* Time Complexity: **O(m × n)**
* No sorting required.
* Uses a fixed-size frequency array.
* Produces a unique signature for every anagram group.

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)

        for s in strs:
            count = [0] * 26

            for c in s:
                count[ord(c) - ord('a')] += 1

            res[tuple(count)].append(s)

        return list(res.values())
```

---

# Key Takeaway

The key insight is that **all anagrams share the same character frequencies**.

* Sorting works because anagrams become identical after sorting.
* Frequency counting works because anagrams have identical character counts.
* Using a frequency array as a hash map key avoids sorting and achieves linear time complexity.

When you see problems involving grouping words by character composition, think:

**"Can I create a unique frequency signature for each word?"**

That's the pattern that makes this problem efficient.
