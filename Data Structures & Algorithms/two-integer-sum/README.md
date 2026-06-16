# Two Sum

## Problem

Given an array of integers `nums` and an integer `target`, return the indices `i` and `j` such that:

```python
nums[i] + nums[j] == target
```

and

```python
i != j
```

You may assume that every input has exactly one valid solution.

Return the answer with the smaller index first.

### Example 1

```python
Input: nums = [3,4,5,6], target = 7
Output: [0,1]
```

### Example 2

```python
Input: nums = [4,5,6], target = 10
Output: [0,2]
```

### Example 3

```python
Input: nums = [5,5], target = 10
Output: [0,1]
```

### Constraints

* `2 <= nums.length <= 1000`
* `-10,000,000 <= nums[i] <= 10,000,000`
* `-10,000,000 <= target <= 10,000,000`
* Exactly one valid answer exists.

---

# Solution 1: Brute Force

## Intuition

The most straightforward approach is to check every possible pair of numbers in the array.

If any pair adds up to the target, return their indices.

Although simple, this approach becomes slow for larger arrays because every element is compared with every other element.

## Algorithm

1. Iterate through the array using index `i`.
2. For each element, iterate through the remaining elements using index `j`.
3. Check whether:

```python
nums[i] + nums[j] == target
```

4. If true, return `[i, j]`.

## Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]

        return []
```

## Dry Run

```python
nums = [3,4,5,6]
target = 7
```

Check pairs:

```python
3 + 4 = 7 ✓
```

Return:

```python
[0,1]
```

## Complexity Analysis

### Time Complexity: O(n²)

Nested loops examine every pair.

```text
O(n²)
```

### Space Complexity: O(1)

No extra data structures are used.

## Pros

* Easy to understand.
* No additional memory required.

## Cons

* Slow for large inputs.
* Repeats many unnecessary comparisons.

---

# Solution 2: Sorting + Two Pointers

## Intuition

If the array is sorted, we can use two pointers:

* One starting from the left.
* One starting from the right.

Depending on the current sum:

* Move left pointer right if the sum is too small.
* Move right pointer left if the sum is too large.

Because sorting changes the order, we must store the original indices.

## Algorithm

1. Store `[value, index]` pairs.
2. Sort the pairs by value.
3. Initialize two pointers:

```python
left = 0
right = len(nums) - 1
```

4. Calculate the current sum.
5. Move pointers based on comparison with target.
6. Return original indices when target is found.

## Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        A = []

        for i, num in enumerate(nums):
            A.append([num, i])

        A.sort()

        left, right = 0, len(nums) - 1

        while left < right:
            current = A[left][0] + A[right][0]

            if current == target:
                return [
                    min(A[left][1], A[right][1]),
                    max(A[left][1], A[right][1])
                ]

            elif current < target:
                left += 1

            else:
                right -= 1

        return []
```

## Dry Run

```python
nums = [3,4,5,6]
target = 7
```

Create pairs:

```python
[[3,0],[4,1],[5,2],[6,3]]
```

Sorted:

```python
[[3,0],[4,1],[5,2],[6,3]]
```

Pointers:

```python
left = 0
right = 3

3 + 6 = 9 > 7
```

Move right:

```python
right = 2

3 + 5 = 8 > 7
```

Move right:

```python
right = 1

3 + 4 = 7 ✓
```

Return:

```python
[0,1]
```

## Complexity Analysis

### Time Complexity: O(n log n)

Sorting dominates the runtime.

```text
O(n log n)
```

### Space Complexity: O(n)

Additional array is required.

```text
O(n)
```

## Pros

* Faster than brute force.
* Introduces the useful two-pointer pattern.

## Cons

* Sorting adds overhead.
* Requires storing original indices.

---

# Solution 3: Hash Map (Two Pass)

## Intuition

A hash map allows constant-time lookups.

First, store every number and its index.

Then, for each number, calculate its complement:

```python
target - current_number
```

If the complement exists in the hash map and isn't the same element, we've found the answer.

## Algorithm

1. Create a hash map.
2. Store each value and its index.
3. Iterate through the array again.
4. Compute the complement.
5. Check whether the complement exists.
6. Return the pair of indices.

## Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        indices = {}

        for i, n in enumerate(nums):
            indices[n] = i

        for i, n in enumerate(nums):
            diff = target - n

            if diff in indices and indices[diff] != i:
                return [i, indices[diff]]

        return []
```

## Explanation

The hash map stores:

```python
value -> index
```

Example:

```python
nums = [3,4,5,6]
```

Hash map:

```python
{
    3: 0,
    4: 1,
    5: 2,
    6: 3
}
```

For:

```python
n = 3
```

Compute:

```python
diff = 7 - 3 = 4
```

Since `4` exists in the map, return:

```python
[0,1]
```

## Complexity Analysis

### Time Complexity: O(n)

Building the map:

```text
O(n)
```

Searching:

```text
O(n)
```

Overall:

```text
O(n)
```

### Space Complexity: O(n)

Hash map stores all elements.

```text
O(n)
```

---

# Solution 4: Hash Map (One Pass)

## Intuition

We can improve the previous solution by combining lookup and insertion into a single pass.

For each number:

1. Calculate its complement.
2. Check if the complement has already been seen.
3. If yes, return the answer.
4. Otherwise store the current number.

This guarantees we never use the same element twice.

## Algorithm

1. Create an empty hash map.
2. Iterate through the array.
3. Calculate:

```python
diff = target - nums[i]
```

4. Check if `diff` exists in the map.
5. If yes, return the indices.
6. Otherwise store the current number and index.

## Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}

        for i, n in enumerate(nums):
            diff = target - n

            if diff in prevMap:
                return [prevMap[diff], i]

            prevMap[n] = i
```

## Dry Run

```python
nums = [3,4,5,6]
target = 7
```

Start:

```python
prevMap = {}
```

Iteration 1:

```python
n = 3
diff = 4

4 not in prevMap

prevMap = {3:0}
```

Iteration 2:

```python
n = 4
diff = 3

3 in prevMap ✓
```

Return:

```python
[0,1]
```

## Complexity Analysis

### Time Complexity: O(n)

Only one traversal of the array.

```text
O(n)
```

### Space Complexity: O(n)

Hash map stores previously seen values.

```text
O(n)
```

## Pros

* Fastest practical solution.
* Single pass through the array.
* Most common interview solution.

## Cons

* Uses additional memory.

---

# Comparison of Solutions

| Approach               | Time Complexity | Space Complexity | Recommended   |
| ---------------------- | --------------- | ---------------- | ------------- |
| Brute Force            | O(n²)           | O(1)             | Learning      |
| Sorting + Two Pointers | O(n log n)      | O(n)             | Good Practice |
| Hash Map (Two Pass)    | O(n)            | O(n)             | Better        |
| Hash Map (One Pass)    | O(n)            | O(n)             | Best          |

---

# Common Pitfalls

## 1. Using the Same Element Twice

Wrong:

```python
if diff in indices:
    return [i, indices[diff]]
```

This may return the same index twice.

Correct:

```python
if diff in indices and indices[diff] != i:
    return [i, indices[diff]]
```

---

## 2. Returning Values Instead of Indices

Wrong:

```python
return [n, diff]
```

The problem asks for positions, not values.

Correct:

```python
return [i, indices[diff]]
```

---

## 3. Calculating the Complement Incorrectly

Wrong:

```python
diff = n - target
```

Correct:

```python
diff = target - n
```

---

## 4. Forgetting Duplicate Numbers

Example:

```python
nums = [5,5]
target = 10
```

A one-pass hash map handles duplicates naturally because it checks for the complement before inserting the current element.

---

# Most Efficient Solution

The **One-Pass Hash Map** approach is the optimal solution because:

* Time Complexity: **O(n)**
* Space Complexity: **O(n)**
* Only one traversal of the array.
* Constant-time lookups using a hash map.

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}

        for i, n in enumerate(nums):
            diff = target - n

            if diff in prevMap:
                return [prevMap[diff], i]

            prevMap[n] = i
```

---

# Key Takeaway

The core idea behind Two Sum is finding a complement.

For each number:

```python
complement = target - current_number
```

If we've already seen that complement, we've found the answer.

* Brute Force checks every pair.
* Sorting uses two pointers after ordering the array.
* Hash Maps provide constant-time lookups.
* The One-Pass Hash Map solution is the most efficient and the standard interview approach.
