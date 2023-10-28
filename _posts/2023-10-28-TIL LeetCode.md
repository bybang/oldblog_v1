---
layout: single
title: "[LeetCode] TIL LeetCode: 1089"
categories: codingTest
tag: [codingTest, LeetCode]
toc: true
published: true
---

# 1089. Duplicate Zeros

## Problem type - Array

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/82036ed9-f75c-491b-9352-59c6fbdd36ff/image.png)

# 🎯 Strategy

### Understand the concept

This was not an **_easy_** question, whether it was valued as the easy question.

The idea of this question is when we meet the zero, put another zero in the next index(or before) and make the array the same size as the original.

### `pop()` and `insert()` approach

To be straightforward, when we iterate through the array, we can `pop()` the last element of the array when `array[i] == 0`. Then put another `0` in the same index of `array[i] == 0`

We can use `insert(i, 0)` in order to achieve the pseudocode above. When we perform insert, increase `i` by `1` to avoid an infinite loop. Lastly, increase `i` by `1` at the end of each loop so that we can skip the consecutive zeros in the array after the insertion.

```python
def duplicateZeros(self, arr: List[int]) -> None:
        """
        Do not return anything, modify arr in-place instead.
        """
        i = 0

        while i < len(arr):
        	if arr[i] == 0:
            	arr.pop()
                arr.insert(i, 0)
                i += 1
            i += 1
```

The time complexity is `O(n^2)`, because `arr.pop()` removes an element from the list and `arr.insert()` insert an element in a specific position.
Both operations may require all the subsequent elements to be shifted, hence `O(n) * O(n) = O(n^2)`.

The space complexity is `O(1)` because we directly modify the input `arr`, and not creating any data structure nor use a variable amount of memory.

#### **Important reminder**

Since `.pop()` modifies the array, this approach is not the correct answer since the problem stated to modify the array in place.

### In-place approach

To submit the right answer in terms of modifying the array in place, we need to think computationally.

How do we not modify the array and move numbers?

The key to this approach is the `index` of each element.
To set the offset, we are going to count `0` in the array and use it as a reference.

```python
'''
Pseudocode

let zeroCount = arr.count(0)
n = len(arr)

loop through the array, and using the index and zeroCount
loop from the end of the array using range(n-1, -1, -1)

For example, if the original array is
arr = [1, 0, 2, 3, 0, 4, 5, 0]

if the condition (arr[i] == 0) matches, reduce the zeroCount by 1
If the reference (i + zeroCount) < n, place 0 next to it(zeroCount was reduced)

arr[i+zeroCount] = 0

if arr[i] != 0, and the reference (i + zeroCount) < n,
bring the value from the array corresponding to the current index

get the value from the same array because we can't modify the array

arr[i + zeroCount] = arr[i],


Hence,
if n == 8, i == n-1(7), zeros == 3
arr == [1, 0, 2, 3, 0, 4, 5, 0]

if n == 8, i == 6, zeros == 2
arr == [1, 0, 2, 3, 0, 4, 5, 0]

if n == 8, i == 5, zeros == 2
arr == [1, 0, 2, 3, 0, 4, 5, 4]

if n == 8, i == 4, zeros == 2
arr[4+2] = arr[4]
arr == [1, 0, 2, 3, 0, 4, 0, 4]

...
if n == 8, i == 2, zeros == 1
arr == [1, 0, 2, 2, 3, 0, 0, 4]

if n == 8, i == 1, zeros == 1
arr == [1, 0, 0, 2, 3, 0, 0, 4]

if n == 8, i == 0, zeros == 0
arr == [1, 0, 0, 2, 3, 0, 0, 4]

and the loop ends because i becomes -1
'''
```

Note that `range()` has three attributes, which are `start`, `stop`, `step`

Implementing the above pseudocode in actual code:

```python
def duplicateZeros(self, arr: List[int]) -> None:
        """
        Do not return anything, modify arr in-place instead.
        """
        zeros = arr.count(0)
        n = len(arr)

        for i in range(n-1, -1, -1):
        	if i + zeros < n:
            	arr[i + zeros] = arr[i]
            if arr[i] == 0:
            	zeros -= 1
                if i + zeros < n:
                	arr[i + zeros] = 0

```

The time complexity is `O(n)` because the operations within the loop are simple assignments and comparisons, which are all constant time.
Therefore, the dominant factor of this complexity is from the `.count(0)` method since it iterates through the whole `arr`

The space complexity is `O(1)` because the code only uses a constant amount of space such as `zeros`, `n` and `i`. It does not create data structure or use memory.

# 📌 Thoughts

Initially, I was unable to come up with either of the methods for the given problem, which was quite disappointing. Later, I found out that the question was indeed challenging, but I still believed that I should have been able to come up with at least the first approach.

Through this problem, I learned about the `in-place` approach, which involves using a reference like `i + zeros` to bring the value from the same array.

# 💻 Solution

### `pop()` and `insert()` approach

```python
def duplicateZeros(self, arr: List[int]) -> None:
        """
        Do not return anything, modify arr in-place instead.
        """
        i = 0

        while i < len(arr):
        	if arr[i] == 0:
            	arr.pop()
            	arr.insert(i, 0)
                i += 1
            i += 1
```

### In-place approach

```python
def duplicateZeros(self, arr: List[int]) -> None:
        """
        Do not return anything, modify arr in-place instead.
        """
        n = len(arr)
        zeros = arr.count(0)

        for i in range(n-1, -1, -1):
            if i + zeros < n:
                arr[i + zeros] = arr[i]

            if arr[i] == 0:
                zeros -= 1
                if i + zeros < n:
                    arr[i + zeros] = 0
```
