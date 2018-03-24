---
title: 练习题1
date: 2017-10-26 21:51:27
tags: ex
copyright: true
---


```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2017/10/26 21:11
# @Author   : keshen
# @File     : test1.py

<!--more-->

# 1. 实现1-100的所有的和

sum = 0

for i in xrange(1, 101):
    sum += i
print(sum)

# 2. 实现1-500所有奇数的和

sum = 0

for i in xrange(1, 501):
    if i%2 != 0:
        sum += i

print(sum)

# 3. 求1+ 2! + 3! + 4! + ……20！的和
# 方法一：

def fac(x):
    if x <= 1:
        return 1
    else:
        return fac(x-1) * x

sum = 0

for i in xrange(1, 21):
    sum += fac(i)

print(sum)

# 方法二：

import math

sum = 0

for i in xrange(1, 21):
    sum += math.factorial(i)

print(sum)

# 4.对指定一个list进行排序[2,32,43,453,54,6,576,5,7,6,8,78,7,89]

l = [2,32,43,453,54,6,576,5,7,6,8,78,7,89]

print(l)
print(sorted(l))

```
