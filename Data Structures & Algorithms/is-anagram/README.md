# Valid Anagram

## Problem

Given two strings `s` and `t`, return `true` if the two strings are anagrams of each other, otherwise return `false`.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

### Example 1

```python
Input: s = "racecar", t = "carrace"
Output: true
```

### Example 2

```python
Input: s = "jar", t = "jam"
Output: false
```

### Constraints

* `1 <= s.length, t.length <= 5 * 10^4`
* `s` and `t` consist of lowercase English letters.

---

# Solution 1: Sorting

## Intuition

If two strings are anagrams, they must contain exactly the same characters with the same frequencies.

Sorting both strings places the characters in a consistent order. If the sorted strings are identical, then they are anagrams.

## Algorithm

1. Check if the lengths are different.
2. If they are, return `False`.
3. Sort both strings.
4. Compare the sorted strings.
5. Return the comparison result.

## Code

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        return sorted(s) == sorted(t)
```

## Dry Run

```python
s = "racecar"
t = "carrace"

sorted(s) = ['a', 'a', 'c', 'c', 'e', 'r', 'r']
sorted(t) = ['a', 'a', 'c', 'c', 'e', 'r', 'r']
```

Both sorted strings are equal, so return `True`.

## Complexity Analysis

### Time Complexity: O(n log n)

* Sorting requires `O(n log n)` time.
* Comparing sorted strings takes `O(n)` time.

Overall:

```text
O(n log n)
```

### Space Complexity: O(n)

Additional space may be required by the sorting algorithm.

## Pros

* Very simple and easy to understand.
* Short implementation.

## Cons

* Sorting is slower than counting frequencies.

---

# Solution 2: Hash Map

## Intuition

If two strings are anagrams, every character must appear the same number of times in both strings.

Instead of sorting, we can count character frequencies using hash maps (dictionaries).

If the frequency maps are identical, the strings are anagrams.

## Algorithm

1. Check if lengths are equal.
2. Create two hash maps.
3. Count occurrences of characters in both strings.
4. Compare the frequency maps.
5. Return the result.

## Code

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)

        for c in countS:
            if countS[c] != countT.get(c, 0):
                return False

        return True
```

## Explanation

The idea is to count how many times each character appears in both strings using two hash maps.

1. If the lengths are different, return `False` immediately.
2. Create two dictionaries:

   * `countS` stores character frequencies in `s`.
   * `countT` stores character frequencies in `t`.
3. Iterate through both strings:

   * Increment the count of each character in `countS`.
   * Increment the count of each character in `countT`.
4. Loop through every character in `countS`:

   * Compare its frequency with the corresponding frequency in `countT`.
   * If they differ, return `False`.
5. If all frequencies match, return `True`.

## Dry Run

For:

```python
s = "racecar"
t = "carrace"
```

After building the frequency maps:

```python
countS = {
    'r': 2,
    'a': 2,
    'c': 2,
    'e': 1
}

countT = {
    'c': 2,
    'a': 2,
    'r': 2,
    'e': 1
}
```

Comparison:

| Character | countS | countT | Match? |
| --------- | ------ | ------ | ------ |
| r         | 2      | 2      | Yes    |
| a         | 2      | 2      | Yes    |
| c         | 2      | 2      | Yes    |
| e         | 1      | 1      | Yes    |

All frequencies match, so return `True`.

## Complexity Analysis

### Time Complexity: O(n)

* One pass to build both hash maps.
* One pass through the unique characters to compare frequencies.
* Overall complexity is **O(n)**.

### Space Complexity: O(1)

* Since the input contains only lowercase English letters, each dictionary can contain at most 26 keys.
* Therefore, the extra space used is **O(1)**.


# Solution 3: Frequency Array (Most Efficient)

## Intuition

The problem guarantees lowercase English letters only.

Since there are only 26 possible characters, we can use a fixed-size array instead of hash maps.

As we process both strings:

* Increment for characters in `s`.
* Decrement for characters in `t`.

If the strings are anagrams, every increment will be canceled by a corresponding decrement.

At the end, all values should be `0`.

## Algorithm

1. Check if lengths are equal.
2. Create an array of size 26 initialized with zeros.
3. Traverse both strings simultaneously.
4. Increment count for characters from `s`.
5. Decrement count for characters from `t`.
6. Verify all counts are zero.
7. Return the result.

## Code

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        count = [0] * 26

        for i in range(len(s)):
            count[ord(s[i]) - ord('a')] += 1
            count[ord(t[i]) - ord('a')] -= 1

        for value in count:
            if value != 0:
                return False

        return True
```

## Dry Run

```python
s = "ab"
t = "ba"
```

Initial:

```python
count = [0, 0, 0, ..., 0]
```

Process:

```python
'a' -> +1
'b' -> -1

'b' -> +1
'a' -> -1
```

Final:

```python
count = [0, 0, 0, ..., 0]
```

All values are zero, so return `True`.

## Complexity Analysis

### Time Complexity: O(n)

* One pass through both strings.

### Space Complexity: O(1)

* Fixed-size array of 26 elements.

## Pros

* Fastest solution.
* Constant extra memory.
* No hashing overhead.

## Cons

* Only works efficiently because the character set is limited.

---

# Comparison of Solutions

| Approach        | Time Complexity | Space Complexity | Recommended         |
| --------------- | --------------- | ---------------- | ------------------- |
| Sorting         | O(n log n)      | O(n)             | Good for interviews |
| Hash Map        | O(n)            | O(1)             | Better              |
| Frequency Array | O(n)            | O(1)             | Best                |

---

# Common Pitfalls

## 1. Forgetting Length Check

Always check length first.

```python
if len(s) != len(t):
    return False
```

If lengths differ, the strings cannot be anagrams.

---

## 2. Using Sorting Unnecessarily

Sorting works but is slower than counting frequencies.

Prefer frequency counting when possible.

---

## 3. Case Sensitivity

This problem only contains lowercase letters.

If uppercase letters were allowed, you would need to normalize the strings:

```python
s = s.lower()
t = t.lower()
```

---

# Most Efficient Solution

For this problem, the **Frequency Array** approach is the optimal solution because:

* Time Complexity: **O(n)**
* Space Complexity: **O(1)**
* Uses only a fixed-size array of 26 elements.
* Avoids sorting and hash map overhead.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        count = [0] * 26

        for i in range(len(s)):
            count[ord(s[i]) - ord('a')] += 1
            count[ord(t[i]) - ord('a')] -= 1

        return all(value == 0 for value in count)
```

---

# Key Takeaway

The core idea is to verify that both strings contain exactly the same characters with the same frequencies.

* Sorting achieves this by arranging characters into a common order.
* Hash maps achieve this by counting frequencies.
* A fixed-size frequency array is the most efficient approach when the character set is limited (such as lowercase English letters).
