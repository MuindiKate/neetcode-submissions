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

### Constraints

```python
1 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9
```

---

# Prerequisites

Before attempting this problem, you should be comfortable with:

* **Hash Sets** – Using sets for O(1) average-time lookups.
* **Sorting** – Sorting an array and checking adjacent elements.
* **Basic Array Traversal** – Using loops to compare elements.

---

# Solution 1: Brute Force

## Intuition

Compare every pair of elements in the array.

If any two elements are equal, a duplicate exists.

This is the most straightforward solution but also the least efficient.

## Algorithm

1. Iterate through the array using two nested loops.
2. Compare every pair of distinct indices.
3. If a matching pair is found, return `True`.
4. If no duplicates are found, return `False`.

## Code

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] == nums[j]:
                    return True
        return False
```

## Dry Run

For:

```python
nums = [1, 2, 3, 3]
```

| i | j | Comparison | Result |
| - | - | ---------- | ------ |
| 0 | 1 | 1 == 2     | False  |
| 0 | 2 | 1 == 3     | False  |
| 0 | 3 | 1 == 3     | False  |
| 1 | 2 | 2 == 3     | False  |
| 1 | 3 | 2 == 3     | False  |
| 2 | 3 | 3 == 3     | True   |

Return `True`.

## Complexity Analysis

### Time Complexity: O(n²)

* Every element is compared with every other element.

### Space Complexity: O(1)

* No extra data structures are used.

---

# Solution 2: Sorting

## Intuition

If the array is sorted, duplicate values become adjacent.

We only need to compare neighboring elements.

## Algorithm

1. Sort the array.
2. Iterate from index `1` to the end.
3. Compare the current element with the previous one.
4. If they match, return `True`.
5. Otherwise return `False` after the loop.

## Code

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        nums.sort()

        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                return True

        return False
```

## Dry Run

```python
nums = [1, 2, 3, 3]
```

After sorting:

```python
[1, 2, 3, 3]
```

Compare neighbors:

| Current | Previous | Duplicate? |
| ------- | -------- | ---------- |
| 2       | 1        | No         |
| 3       | 2        | No         |
| 3       | 3        | Yes        |

Return `True`.

## Complexity Analysis

### Time Complexity: O(n log n)

* Sorting dominates the runtime.

### Space Complexity: O(1)

* Depends on the sorting algorithm.
* Some implementations may require O(n) auxiliary space.

## Pros

* Simple logic.
* No hash set needed.

## Cons

* Slower than the optimal solution.
* Modifies the input array.

---

# Solution 3: Hash Set (Optimal)

## Intuition

A hash set allows O(1) average-time lookups.

As we iterate through the array, we check whether the current number has already been seen.

If it has, we found a duplicate.

## Algorithm

1. Create an empty hash set.
2. Traverse the array.
3. For each number:

   * If it already exists in the set, return `True`.
   * Otherwise add it to the set.
4. If the loop completes, return `False`.

## Code

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

## Explanation

The idea is to use a **hash set** to keep track of the numbers we have already seen.

1. Create an empty set called `hashset`.
2. Iterate through each number in the array.
3. For each number:

   * If it already exists in the set, we have found a duplicate, so return `True`.
   * Otherwise, add it to the set.
4. If we finish iterating through the entire array without finding any duplicates, return `False`.

## Dry Run

For `nums = [1, 2, 3, 3]`:

| Current Number | Hash Set  | Duplicate Found?  |
| -------------- | --------- | ----------------- |
| 1              | {1}       | No                |
| 2              | {1, 2}    | No                |
| 3              | {1, 2, 3} | No                |
| 3              | {1, 2, 3} | Yes → Return True |

## Why a Hash Set?

A hash set provides **O(1)** average-time lookup and insertion operations.

This allows us to quickly check whether a number has already appeared without scanning the entire array.

## Complexity Analysis

### Time Complexity: O(n)

* We traverse the array once.
* Set lookup and insertion are O(1) on average.

### Space Complexity: O(n)

* In the worst case, all elements are unique.

## Pros

* Fastest practical solution.
* Easy to understand.
* Only one pass through the array.

## Cons

* Requires additional memory.

---

# Solution 4: Hash Set Length

## Intuition

A set automatically removes duplicates.

If duplicates exist, the set will contain fewer elements than the original array.

## Algorithm

1. Convert the array into a set.
2. Compare the set size with the array size.
3. If the set is smaller, duplicates existed.

## Code

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        return len(set(nums)) < len(nums)
```

## Example

```python
nums = [1, 2, 3, 3]

set(nums) = {1, 2, 3}

len(set(nums)) = 3
len(nums) = 4
```

Since:

```python
3 < 4
```

Return `True`.

## Complexity Analysis

### Time Complexity: O(n)

### Space Complexity: O(n)

## Pros

* Shortest implementation.
* Easy to remember.

## Cons

* Less explicit about the logic.
* Not ideal for explaining in interviews.

---

# Comparison of Solutions

| Approach        | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| Brute Force     | O(n²)           | O(1)             |
| Sorting         | O(n log n)      | O(1)*            |
| Hash Set        | O(n)            | O(n)             |
| Hash Set Length | O(n)            | O(n)             |

* Depending on the sorting algorithm, auxiliary space may be O(n).

---

# Common Pitfalls

## 1. Comparing an Element With Itself

Wrong:

```python
for i in range(len(nums)):
    for j in range(len(nums)):
        if nums[i] == nums[j]:
            return True
```

This compares an element with itself.

Correct:

```python
for i in range(len(nums)):
    for j in range(i + 1, len(nums)):
        if nums[i] == nums[j]:
            return True
```

---

## 2. Modifying the Input Array

The sorting solution changes the original array:

```python
nums.sort()
```

If preserving the original order is important:

```python
sorted_nums = sorted(nums)
```

---

# Most Efficient Solution

The **Hash Set** approach is the recommended interview solution because:

* Time Complexity: **O(n)**
* Easy to explain
* Performs lookups in constant time
* Requires only a single pass through the array

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

# Key Takeaway

The core idea is to determine whether any value appears more than once.

* Brute Force compares every pair.
* Sorting groups duplicates together.
* Hash Sets provide fast duplicate detection.
* Comparing `len(set(nums))` with `len(nums)` is a concise variation of the hash set approach.

For interviews, the **Hash Set solution** is typically the best balance of efficiency and clarity.
