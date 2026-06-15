

# Contains Duplicate

## Problem

Given an integer array `nums`, return `true` if any value appears more than once in the array. Otherwise, return `false`.

### Example 1

```python
Input: nums = [1, 2, 3, 3]
Output: true
```

### Example 2

```python
Input: nums = [1, 2, 3, 4]
Output: false
```

### Example 3

```python
Input: nums = [1, 1, 1, 3, 3, 4, 3, 2, 4, 2]
Output: true
```

---

## Solution

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        hashset = set()

        for n in nums:
            if n in hashset:
                return True
            hashset.add(n)

        return False
```

---

## Explanation

The idea is to use a **hash set** to keep track of the numbers we have already seen.

1. Create an empty set called `hashset`.
2. Iterate through each number in the array.
3. For each number:

   * If it already exists in the set, we have found a duplicate, so return `True`.
   * Otherwise, add it to the set.
4. If we finish iterating through the entire array without finding any duplicates, return `False`.

### Dry Run

For `nums = [1, 2, 3, 3]`:

| Current Number | Hash Set  | Duplicate Found?    |
| -------------- | --------- | ------------------- |
| 1              | {1}       | No                  |
| 2              | {1, 2}    | No                  |
| 3              | {1, 2, 3} | No                  |
| 3              | {1, 2, 3} | Yes → Return `True` |

---

## Why a Hash Set?

A hash set provides **O(1)** average-time lookup and insertion operations.

This allows us to quickly check whether a number has already appeared without having to scan the entire array each time.

---

## Complexity Analysis

### Time Complexity: O(n)

* We iterate through the array once.
* Each set lookup and insertion takes **O(1)** on average.
* Therefore, the total time complexity is **O(n)**.

### Space Complexity: O(n)

* In the worst case, all elements are unique.
* We store all `n` elements in the hash set.
* Therefore, the space complexity is **O(n)**.

---

## Key Takeaway

Using a hash set allows us to detect duplicates in a single pass through the array, making it much more efficient than a brute-force approach that compares every pair of elements (`O(n²)`).
