---
layout: single
title: "[LeetCode] TIL LeetCode: 27, 26"
categories: codingTest
tag: [codingTest, LeetCode]
toc: true
published: true
---

# 27. Remove Element

## Problem type - Array

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/8b13aa7a-1238-446f-a54d-45d5d9322732/image.png)

![](https://velog.velcdn.com/images/devbang/post/4b4f2194-d941-445e-b46c-045a25f9f37c/image.png)

# 🎯 Strategy

### Understand the concept

Read the given instructions carefully.

Two arguments are given: an integer array `nums` and an integer `val`.

The `val` should be removed from the `nums` array, and the removal process should be in place.

The important note is that we are returning the first `k` elements of the `num` array, which derive our answer as a single integer.

The `k` is the length of the changed array or the index as we change the array in place.

## Basic approach

### `remove()` approach

Let's use a Python library to simplify and get used to this concept.

The `remove()` function is indeed not an in-place approach, but we can make simple code as below:

```python
def removeElement(self, nums: List[int], val: int) -> int:
	while val in nums:
    	nums.remove(val)
```

This could be a one-liner code and shows the power of the Python library. But it takes much more time and is not an in-place algorithm.

The time complexity is `O(n^2)` because the `.remove()` operation has a time complexity of `O(n)`. And the while loop has a time complexity of `O(n)`, hence the overall time complexity is `O(n^2)`.

The space complexity is `O(1)`, since it doesn't create any additional data structures or use extra memory.

## Two pointer approach

### For loop and pointer

Although the above code does the job, there are better answers than this one.

What we can do instead is use a pointer to achieve the same goal. The picture below is the typical two-pointer structure in the algorithm.

![](https://velog.velcdn.com/images/devbang/post/f28d0c72-90df-4ddf-bd64-6bcd7ad6f173/image.png)

We use the same technique above while putting the pointer `k = 0` and looping through the nums array with `for-loop`.

We will skip if the number in the `nums` array equals `val`. If the number in the `nums` array is not equal to `val`, we change the value.

![](https://velog.velcdn.com/images/devbang/post/502b1269-fecd-4644-8bd1-358e46939cec/image.jpeg)

In this case of `0, 1`, we assigned each value in-place. But when it comes to `k = 2`, k didn't go further, but the `num` goes further the list.

When the `num` reaches `3`, it assigns `3` to the current `k`'s place. This is the logic for the pointer approach.

```python
def removeElement(self, nums: List[int], val: int) -> int:
	k = 0

    for num in nums:
    	if num != val:
        	nums[k] = num
        	k += 1

    return k
```

As the loop finishes, the `k` goes to the expected index, or we can say `k == expectedNums.length`.

The time complexity is `O(n)` because we use a for-loop to iterate through the `nums` array.

The space complexity is `O(1)` because the code operates in place, modifying the `nums` array without creating a new data structure.

# 📌 Thoughts

I was trying hard to get exactly the same output in example 2, but it turns out I didn't read the question carefully.

The custom judge part mentions that `assert k` is the length of the expected array and `sort()` the nums, so the order of numbers doesn't really matter in this problem.

# 💻 Solution

### `.remove()` approach / not in-place

```python
def removeElement(self, nums: List[int], val: int) -> int:
	while val in nums:
    	nums.remove(val)
```

### Two-pointer approach / in-place

```python
def removeElement(self, nums: List[int], val: int) -> int:
	k = 0

    for num in nums:
    	if num != val:
        	nums[k] = num
            k += 1

     return k
```

# 26. Remove Duplicates from Sorted Array

## Problem type - Array

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/2553035d-5050-432d-a157-68801aa791e0/image.png)

![](https://velog.velcdn.com/images/devbang/post/2cbb50ff-0766-4ea4-a197-95b2031c072a/image.png)

# 🎯 Strategy

## Concept of the problem

### Given conditions

Given that the array is `nums`, we need to remove the recurrent numbers in the array. The array is sorted in ascending order, and we need to use the `in-place` algorithm to derive the `k` - the first k elements of `nums` that are not replicated.

## Basic approach

### `set()` approach

Similar to the previous problem, we can solve the current situation with the Python library but not in an in-place manner.

```python
def removeDuplicates(self, nums: List[int]) -> int:
	nums[:] = set(nums)
    return len(nums)
```

Notice the `set()` method cuts off the duplicated data.

```python
original:  [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]
modified:  [0, 1, 2, 3, 4]
```

The time complexity is `O(n)` since converting the array `nums` to a set has a time complexity of `O(n)`.

The space complexity is `O(n)`, because the `set()` creates and stores the modified array.

## Two pointer approach

### For-loop and pointer

The two-pointer approach is very similar to the previous problem. The difference is the usage of the index.

```python
def removeDuplicates(self, nums: List[int]) -> int:
	k = 0

    for num in nums:
    	if nums[k] != num:
        	nums[k + 1] = num
            k += 1

    return k + 1
```

Notice that we return `k + 1` to match the custom judge's assert condition.

The time complexity is `O(n)`, because the for-loop iterates through the `nums` array.

The space complexity is `O(1)`, because the code operates in place, modifying the `nums` array without using the additional data structure.

# 📌 Thoughts

I didn't draw the data structure until now, but drawing the data structure helped me understand the algorithm.

If allowed, I will draw until I get comfortable with the data structure and algorithm. Also, this was quite easy to solve since the problem was very similar to the previous one.

# 💻 Solution

### `set()` approach / not in-place

```python
def removeDuplicates(self, nums: List[int]) -> int:
	nums[:] = set(nums)
    return len(nums)
```

### Two-pointer approach / in-place

```python
def removeDuplicates(self, nums: List[int]) -> int:
	k = 0

    for num in nums:
    	if nums[k] != num:
        	nums[k + 1] = num
            k += 1

    return k + 1
```
