---
layout: single
title: "[LeetCode] Daily LeetCode: 1323"
categories: codingTest
tag: [codingTest, LeetCode]
toc: true
published: true
---

# 1323. Maximum 69 Number

## Problem type - Greedy

# 🧩 Problem

![](https://velog.velcdn.com/images/devbang/post/0b3c637a-614e-4835-8ab5-3fb8eb70b9c8/image.png)

# 🎯 Strategy

### Setting up the logic

We need to understand the concept and the details of this problem. According to examples, we can derive that checking each digit and changing the biggest digit, which is 6, is the answer.

### Replace method

Python has a useful function on this problem: ` .replace()`.

In order to use the replace function, we need to change the number into a string. `str(num)`

Then, we can assign it to the variable `result` and use the replace function.

To replace the function's variables, we put the string to search for(old value), the string to replace the old value, and how many old values you want to replace.

From our logic, we are finding the biggest digit, that is '6'.

```python
def maximum69Number (self, num: int) -> int:
	result = str(num).replace("6", "9", 1)
```

To conclude, we can simply return with the replaced integer value.

```python
def maximum69Number (self, num: int) -> int:
 	result = str(num).replace("6", "9", 1)
    return int(result)
```

### List method

The previous method was simple, but we couldn't practice the basic logic since we used the Python library.

So, let's use the list method to prepare a general coding interview.

```python
def maximum69Number (self, num: int) -> int:
	numList = list(str(num))

    for i in range(len(numList)):
```

To iterate over the num, we have to put the single numbers in the list as a string. Then, iterate over each number in the range of the length of the list.

Each index is a digit, and we want to know the biggest digit, which is 6. We will stop the loop once we find the biggest digit, 6.

```python
def maximum69Number (self, num: int) -> int:
	numList = list(str(num))

    for i in range(len(numList)):
    	if numList[i] == '6':
        	numList[i] = '9'
            break

	return int("".join(numList))
```

Lastly, we can return the modified list by using the `.join()` method to obtain the final numbers.

# 📌 Thoughts

I spent time considering how to iterate through the number, but I realized that iterating over the 'num' itself was not possible. Therefore, I attempted to use the second method, which could be applied in various programming languages.

# 💻 Solution

### List method

```python
 def maximum69Number (self, num: int) -> int:
        numList = list(str(num))

        for i in range(len(numList)):
        	if numList[i] == '6':
            	numList[i] = '9'
                break

        return int("".join(numList))
```

### Python replace Method

```python
 def balancedStringSplit(self, s: str) -> int:
 	result = str(num).replace('6', '9', 1)

    return int(result)
```
