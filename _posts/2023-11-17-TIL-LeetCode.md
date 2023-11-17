---
layout: single
title: "[LeetCode] TIL LeetCode: 1346"
categories: codingTest
tag: [codingTest, LeetCode]
toc: true
published: true
---

# 1346. Check If N and Its Double Exist

## Problem type - Array

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/79ea728b-29c1-4731-9a5e-77802cbb9d78/image.png)

# 🎯 Strategy

### Understand the concept

The problem's concept is pretty straightforward.

We have to return True if the number in the given array's one integer is double to the other integer.

Otherwise, return False if none of the integers matches the condition `arr[i] == arr[j] * 2` and deliberately mentions that `i != j`.

## Basic approach

### Nested loop approach

While the two indices are different, we have to check the `i` index and `j` index at the same time.

This means we have to ask if `arr[i] == arr[j] * 2`, each time j visits the numbers. When the `j` finishes the loop, we should increase the `i` index by one and do the same thing again.

![](https://velog.velcdn.com/images/devbang/post/c11ce0d7-27a4-4a76-890b-f9217f6718fc/image.jpeg)

When the condition matches, we can return True because we are checking if the number exists, not how many numbers exist.

```python
def checkIfExist(self, arr: List[int]) -> bool:
	N = len(arr)

    for i in range(N):
    	for j in range(i+1, N):
        	if arr[i] == arr[j] * 2 or arr[j] / 2 == arr[i]:
            	return True

    return False
```

The time complexity is `O(n^2)` because the outer loop takes `O(n)` times, and the inner loop also takes `O(n)` times. The nested loop usually has the time complexity of `O(n^2)`.

The space complexity is `O(1)`, since it doesn't create any additional data structures or use extra memory.

## `set()` approach

### Create set()

There are many ways to solve this problem, such as binary search or hashmap, but I have yet to learn it, so I decided to choose this solution.

Initially, we create an empty set that collects the data after the checking process is done.

```python
def checkIfExist(self, arr: List[int]) -> bool:
	checked = set()
```

Then, loop through numbers in the array `arr` with the conditional. The condition chain is unwieldy, but it checks all the conditions.

At the beginning, it asks if `number * 2` is in the checked set.
Secondly, it asks if `number % 2 == 0` since we are looking for multiples of 2.
Simultaneously, it asks if `number / 2` is in the checked set, which the loop only checks forward, but this condition checks the same condition above `if arr[j] / 2 == arr[i]`.

If none of the conditions above matches, we add the `number` into the checked set to check for the `n // 2` or `2 * n` condition.

```python
def checkIfExist(self, arr: List[int]) -> bool:
	checked = set()

    for n in arr:
    	if 2 * n in checked or n % 2 == 0 and n // 2 in checked:
        	return True
        else:
        	checked.add(n)

    return False
```

The for-loop iterates through each element, and the checking operations are constant time. Hence, the time complexity is `O(n)` because the loop runs linearly with respect to the size of the input list.

The space complexity is `O(n)` because the `set()` creates additional storage to store unique elements encountered during the iteration. It may grow to the size of the input array.

# 📌 Thoughts

I couldn't solve the problem without the nested loop, although I knew it takes `O(n^2)` times. It seems like there are many different ways to solve this problem.

I'm currently in the `Array 101` track, but I hope to learn another technique in another track and use it in a similar problem.

# 💻 Solution

### Nesetd loop approach

```python
def checkIfExist(self, arr: List[int]) -> bool:
	N = len(arr)

    for i in range(N):
    	for j in range(i+1, N):
        	if arr[i] == arr[j] * 2 or arr[j] / 2 == arr[i]:
            	return True

	return False
```

### `set()` approach

```python
def checkIfExist(self, arr: List[int]) -> bool:
	checked = set()

    for n in arr:
    	if 2 * n in checked or n % 2 == 0 and n // 2 in checked:
        	return True
        else:
        	checked.add(n)

	return False
```
