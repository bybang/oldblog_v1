---
layout: single
title: "[LeetCode] Daily LeetCode: 1221"
categories: codingTest
tag: [codingTest, LeetCode]
toc: true
published: true
---

# 1221. Split a String in Balanced Strings

## Problem type - Greedy

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/83c98b1a-54ad-4914-81a7-769846bdae63/image.png)

# 🎯 Strategy

### Greedy Algorithm

To solve this problem, we need to design this problem by using greedy algorithm.

### Set variables

We need two variables: the counter to counting the letters and the result variable. When the condition matches, the result variable goes up by one.

In order to sort the number, we should change the number into a string first.

```python
 def balancedStringSplit(self, s: str) -> int:
 	count = 0
    result = 0
```

### Use the counter

Now we need to set the logic, which can be by iterating over the string `s`.

The function will increase the counter if the letter is `R`.
The function will decrease the counter if the letter is `L`.

When the `count == 0`, the `result` should increase since we made `RL` the balanced string.

```python
def balancedS... -> int:
	...
    for letter in s:
    	if letter == "R":
        	count += 1
        elif letter == "L":
        	count -= 1

        if count == 0:
        	result += 1

     return result
```

If you know the `stack` method, this logic is similar to the Python `DFS` method. As this reminds me of the stack method, let's try the stack method.

First, we set up the `stack` and `result`.
Then iterate over the string as previously we did, and if the stack is empty, the result goes up by one(same as if the `count == 0`)

Next, we will append the letter to the stack.

```python
def balanced.. -> int:
	stack, result = [], 0

    for letter in s:
    	if stack == []:
        	result += 1
            stack.append(letter)
```

Second, since we appended the letter in the stack, we are checking the second letter here. We will pass over the first if statement since there is a letter in the stack.

If the current `letter` is equal to the appended letter in the stack, which means if they are the same letter, append the letter in the stack and go to the third letter.

If the current `letter` is not equal to the appended letter, then just `pop()` the letter from the `stack`

```python
        elif letter == stack[-1]:
        	stack.append(letter)

        else stack.pop()

	return result
```

If all the letter pops out from the `stack`, we check the second balanced string. The result goes up by one at this point, and in the end, the for loop loops until the end of the string.

# 📌 Thoughts

It was a good question regarding practicing a greedy method. I struggled to manage the counter. Next time, I should remember the counter can be used in various ways.

# 💻 Solution

### Normal Greedy

```python
 def balancedStringSplit(self, s: str) -> int:
        count = 0
        result = 0

        for letter in s:
        	if letter == "R":
            	count += 1
            if letter == "L":
            	count -= 1

            if count == 0:
            	result += 1

		return result
```

### Stack Method

```python
 def balancedStringSplit(self, s: str) -> int:
        stack, result = [], 0

        for letter in s:
        	if stack == []:
            	result += 1
            	stack.append(letter)

            elif letter == stack[-1]:
            	stack.append(letter)

            else:
            	stack.pop()

        return result
```
