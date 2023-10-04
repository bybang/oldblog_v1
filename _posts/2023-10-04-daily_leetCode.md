---
layout: single
title: "[LeetCode] Daily LeetCode: 485, 1295"
categories: codingTest
tag: [codingTest, LeetCode]
toc: true
published: true
---

# 485. Max Consecutive Ones

## Problem type - Array(List)

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/4413414d-2ff3-4905-9e9e-a20362f73a96/image.png)

# 🎯 Strategy

### If-else approach

Since this is an easy problem, I chose to iterate over the list `nums` without using the `max()` function.

Notice we need two variables to count current consecutive 1s and max consecutive 1s.

```python
def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
 	count = 0
    maxCount = 0

    for i in range(len(nums)):
    	if nums[i] == 1:
        	count += 1
        elif nums[i] == 0:
        	count = 0

        if count > maxCount:
        	maxCount = count

	return maxCount
```

### `max()` approach

TThe code above is working but feels redundant. I want to condense it by using the `max()` function.

```python
def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
	count = 0
    max_count = 0

    for i in range(len(nums)):
    	if nums[i] == 1:
        	count += 1
        else:
        	max_count = max(count, max_count)
            count = 0

	return max(count, max_count)
```

This approach condensed three conditions into two conditions using the `max()` function.

The time complexity is `O(n)`, and the space complexity is `O(1)`.

# 📌 Thoughts

I just started a new beginner track from LeetCode and was tempted to start with Java, but I decided to stick with Python for now.

This was an easy question, but I spent some time figuring out I needed two variables. I realized I lack base knowledge, so it should be a good track for me to build up the foundation of the algorithm.

# 💻 Solution

### `max()` approach

```python
def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
	count = 0
    max_count = 0

    for i in range(len(nums)):
    	if nums[i] == 1:
        	count += 1
        else:
        	max_count = max(count, max_count)
            count = 0

	return max(count, max_count)
```

# 1295. Find Numbers with Even Number of Digits

## Problem type - Array(List)

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/11e3ac54-ea7c-42a8-8600-ef146d6572e4/image.png)

# 🎯 Strategy

### For-loop approach

A straightforward approach is the for-loop approach.

Since these are the numbers, we can't count the length of each item. In order to count it, we should use `str()`.

```python
def findNumbers(self, nums: List[int]) -> int:
	result = 0

    for item in nums:
    	if len(str(item)) % 2 == 0:
        	result += 1

    return result
```

### List comprehension approach

The code above can be condensed into one line(one-liner code)

To reduce it to one line, we need to know the structure of `ex for i in x if condition`
The `ex` represents the operation you want to execute within the iterable.
The `i` represents each value taken from the iterable.
The `x` represents iterable, which means the list in this case.
The `condition` part is optional, but we will use it to filter even numbers.

Now, loop through the list and check each item. If the condition matches, it will put the item in the list. And count the final list's item should be the answer.

```python
def findNumbers(self, nums: List[int]) -> int:
	return len([i for i in nums if len(str(i)) % 2 == 0])
```

The time complexity is `O(n)`, and the space complexity is `O(1)`.

# 📌 Thoughts

I knew the concept of the list comprehension but never thought to use it.

I should make and think of a one-liner code while following this track.

One another solution was using the `sum()` function, but I need to look up why this `return sum((len(str(i))) % 2 == 0 for i in nums)` condition returns the count of the even numbers, so I didn't use it here.

# 💻 Solution

### List comprehension approach

```python
def findNumbers(self, nums: List[int]) -> int:
    return len([i for i in nums if len(str(i)) % 2 == 0])
```
