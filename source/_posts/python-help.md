---
title: 帮助
date: 2017-10-24 22:21:15
tags: python
copyright: ture
---

# 帮助

```
# 当我们要查看python的变量属于哪个数据类型的时候，可以使用：
print(type(variable))
```

<!--more-->

```
# 当我们要查看一个变量具体有哪些方法，可以使用：
variable = dict(a = 1, b = 2)
print(dir(variable))
```

1. 在pycharm中使用ctrl + 鼠标左键，点击函数可以看到它的用法和源码
2. dir() 可以查看有哪些方法
3. help() 帮助
4. type() 类型

# 类型强制转换

```
# age是整型，print打印的是字符串类型，所以会报错
age = 20
print('My age is ' + age)

# 可以对age进行类型强制转换
age = 20
print('My age is ' + str(age))

# int() str() dict() list() tuple() ,类型转换
```