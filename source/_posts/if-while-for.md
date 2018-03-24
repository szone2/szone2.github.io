---
title: if,while,for
date: 2017-10-25 22:36:23
tags: python
copyright: ture
---

# if,while,for

## Python的缩进和冒号

Python并不像其他语言那样要求什么{}，仅仅使用tab键来区分代码的逻辑

<!--more-->

## if

```

# 格式：

if 判断条件:
    执行语句... ...
else:
    执行语句... ...

# 实例：你可以通过不断改变a的值来打印不同的执行结果

a = 3

if a < -1:
    print('a是负数')
elif a == 0:
    print('a等于0')
else:
    print('a是正数')

```

## while循环

```
# 格式

while 条件:
    执行语句... ...
    
```

```
# 例子

abc = 10

print("This scripts is started")

while abc > 0:
    print(abc)
    abc -= 1

print("This scripts is ended")

```

## for循环

for 循环主要在工作中用来遍历列表，字符串，文件等操作，for循环默认是循环到元素完为止

例子：

```
test = dict(a=1, b=2, c=3, d=4)
for k, v in test.iteritems():
    print(k, v)
```

## range与xrange的区别

区别：xrange做循环的性能比range好，尤其是返回很大的时候。尽量用xrange吧，除非你是要返回一个列表。

range([start,] stop[, step])，根据start与stop指定的范围以及step设定的步长，生成一个**序列**。

比如：

```
>>> range(5) 
[0, 1, 2, 3, 4] 
>>> range(1,5) 
[1, 2, 3, 4] 
>>> range(0,6,2)
[0, 2, 4]

```

xrange 用法与 range 完全相同，所不同的是生成的不是一个list对象，而是一个**生成器**。

```
>>> xrange(5)
xrange(5)
>>> list(xrange(5))
[0, 1, 2, 3, 4]
>>> xrange(1,5)
xrange(1, 5)
>>> list(xrange(1,5))
[1, 2, 3, 4]
>>> xrange(0,6,2)
xrange(0, 6, 2)
>>> list(xrange(0,6,2))
[0, 2, 4]

```

由上面的示例可以知道：要生成很大的数字序列的时候，用xrange会比range性能优很多，因为不需要一上来就开辟一块很大的内存空间。

xrange 和 range 这两个基本上都是在循环的时候用。

```
for i in range(0, 100): 
    print i 

for i in xrange(0, 100): 
    print i 
```

这两个输出的结果都是一样的，实际上有很多不同，range会直接生成一个list对象：

```
a = range(0,100) 
print type(a) 
print a 
print a[0], a[1] 

```

输出结果：

<type 'list'>
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
0 1

而xrange则不会直接生成一个list，而是每次调用返回其中的一个值：

```
a = xrange(0,100) 
print type(a) 
print a 
print a[0], a[1] 

```

输出结果：

<type 'xrange'>
xrange(100)
0 1

## continue,break

例子：

```
# debug模式运行，按F8一句一句运行
for i in xrange(1, 11):
    if i == 3:
        print('hello, world')
        continue
        #break
    print('i = %d', % i)
```

### break

在循环中，break语句可以提前**退出**循环。例如，本来要循环打印1～100的数字：

```
n = 1
while n <= 100:
    print(n)
    n = n + 1
print('END')

# 如果要提前结束循环，可以用break语句：

n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')

```

### continue

在循环过程中，也可以通过continue语句，**跳过**当前的这次循环，直接开始下一次循环。


```
n = 0
while n < 10:
    n = n + 1
    print(n)

# 上面的程序可以打印出1～10。但是，如果我们想只打印奇数，可以用continue语句跳过某些循环：

n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)

```

执行上面的代码可以看到，打印的不再是1～10，而是1，3，5，7，9。

---