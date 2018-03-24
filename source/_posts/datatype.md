---
title: 数据类型
date: 2017-10-20 22:05:34
tags: python
copyright: true
---

# 整型

```
name = raw_input("Please input your name: ")
print(name)
print(type(name)
age = input("Please input your age: ")
print(age)
print(type(age))
```

<!--more-->

raw_input()把输入的数据转换为字符串

input()只接受数字类型

```
a = 100
b = -20
print(a)
print(b)
print(a-b)
print(abs(a) + abs(b))
print(a/b)
```

# 浮点型

用round()内置的方法来取小数点的精度是最常用的。

当round(float)只包含数字的时候，默认保留1位小数，采用四舍五入的方式，例子如下：


```
#默认保留一位小数
#采用四舍五入的方法进行计算
a = 3.00
b = 2.53
c = 2.43
print(round(a))
print(round(b))
print(round(c))
```

```
#round(float, 精度)
#先进行四舍五入运算。如果精度最后一位为偶数，符合条件；如果小数点精度的最后一位为奇数，则舍弃原小数点精度以后的所有数据。
#用一句话说就是保证精度最后一位必须为偶数。
c = 2.555
b = 1.545
print(round(c, 2))
print(round(b, 2))
```

# 布尔类型

布尔型只有两个值，一个是true，另一个是false


```
print(not True)
a = 10
b = 20
c = 100
print(a > b and c > a)
print(not (a > b and c > a))
```

# 字符串

## 例子

```
str1 = 'aaa'
str2 = 'bbb'
str3 = 'ccc'
print(str1, str2, str3)
print(type(str1))
print(dir(str1))
```

## 字符串常用方法

```
str1 = 'abcd'
print(str[0], str1[2], str1[3]. str1[4])
```


```
#find
#replace
#split
#join
#strip
#format
a = 'adfeliaaangfasj213aaa13jlidjfsdfae'
print(a.find('fas'))
print(a.replase('fas', 'nbc'))
print(a.split('aaa')
print('hello  '.join(a.split('aaa')))

b = '   sdfdslkfj sdkfjlse sldkfsl lskdjfls  '
#把左右两边的空格去掉
print(b.strip())
#把左边的空格去掉
print(b.lstrip())
#把右边的空格去掉
print(b.rstrip())

name = 'abc'
age = 10
print('hello ' + name)
# %s 代表的是字符串，%d代表的是整型，%f代表的是浮点型
print('hello ' %s) % name
print('hello  {0}').format(name)

print('hello  {0}, my age is: {1}'.format(name, age))
print('{name}:{age}'.format(name='andy', age = 20))

```

## 字符串的注释

在python中，注释用#号标识，#号后面的内容都会被python解释器忽略，也可以在头文件后面直接添加字符串，来解释该项目或者文件的作用或者解释说明等。


```
'''this file is a test file.'''
```