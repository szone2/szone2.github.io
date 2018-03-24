---
title: 函数
date: 2017-11-01 21:29:56
tags: python
copyright: ture
---

# 函数的一般形式

在Python中，定义一个函数要使用def语句，依次写出函数名、括号、括号中的参数和冒号:，然后，在缩进块中编写函数体，函数的返回值用return语句返回。

我们以自定义一个求和的sum函数为例：

<!--more-->

```
# 函数的定义

def sum(x, y):
    return x + y

# 写成sum(y=3, x=10)也可以
m = sum(10, 3) 

print(m)

```

# 函数的参数

```
# 给形参b设定一个默认的值

def funcA(a, b=0):
    print a
    print b

# 如果实参传入的时候，没有给定b的值，b会使用默认参数值0
funcA(1)

# 如果实参传入的时候，给定了b的值，b会优先选择传入的实参
# funCA(10, 20)

```

```
# 参数为tuple

def funcD(a, b, *c):
    print a
    print b
    print "length of c is : %d " % len(c)
    print c
    
funcD(1, 2, 3, 4, 5, 6)

# 或者可以先定义好一个tuple,再通过实参传入
# test = ('hello', 'world')
# funcD(1, 2, *test)

```

```
# 参数为dict

def funcE(a, **b):
    print(a)
    print(b)
    for x in b:
        print x + ':' + str(b[x])

funcE(100, x='hello', y='你好')

# 同样可以先定义好一个dict,再通过实参传入
# args = {'1':'hello', '2':'200'}
# funcD(100, **args)

```

# 高阶函数

> 高阶函数就是把函数当成参数传递的一种函数

```
def add(x, y, f):
    return f(x) + f(y)

print(add(-8, 11, abs)

# 结果：19

# 调用add函数，分别执行abs(-8) 和 abs(11)，分别计算出他们的值
# 最后在做和运算

```

## map()函数

map函数是python内置的一个高阶函数，它接受一个函数f 和一个list，并把list的元素以此传递给函数f ，然后返回一个函数f 处理完所有list元素的列表如下所示：

```
def f2(x):
    return x * x

l = [1, 2, 3, 4, 5, 6]

# 第一个参数是一个函数，第二个参数是一个可迭代对象

print(map(f2, l))

# 结果
# [1, 4, 9, 16, 25, 36]
# 解析：
# l是一个list，把此list的元素传入函数f2，求每个元素的平方
# 把最终所有的计算结果合并成一个新的list，就如新的结果所示

```

## reduce()函数

```
# 传入f函数参数接受两个参数
# 把可迭代对象的两个参数作为函数的实参，传入到f函数中
# 把每次f运算的结果作为第一个实参，可迭代对象的下一个元素作为另一个实参，传入函数f中
# 以此类推，最终得到结果

def f(x, y):
    return x + y
print(reduce(f, [1, 2, 3, 4, 5], 10)

```

## filter()函数

filter即过滤的意思，filter()函数接受一个函数f 和一个list，这个函数f 的作用是对每个元素进行判断，返回True或false，filter()根据判断结果自动过滤不符合条件的元素，返回由符合条件元素组成的新list

```
def is_odd(x):
    return x % 2 == 1
print(filter(is_odd, [1, 2, 3, 4, 5, 6, 7]))

# 结果：[1, 3, 5, 7]

```

## sorted()函数

```
# 对字典进行排序
mm = dict(a=1, b=10, c=3, d=9)
# 对key排序
test = sorted(mm)
print(test)
# 对value排序, key参数传进value来进行排序
test = sorted(mm.iteritems(), key = lamda d: d[1])
print(test)
```

# 匿名函数（没有名字的函数）

```
#def sum(x, y):
#    return x + y

#上面的函数作用等同于下面的匿名函数
m = lamda x, y: x + y
print(m(4, 5)
```

# 列表生成式和生成器

## 列表生成式

> 对一组元素进行过滤，还可以对元素进行格式化处理

```
# 列表生成式
# 求1到100所有偶数的平方
# 前面先写你需要的东西x*x ，后面写for处理

li = [ x*x for x in xrange(1, 101) if x%2 == 0 ]
print(li)
```

## 生成器

> 通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的，而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

> 所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间，在Python中，这种一边循环一边计算的机制，称为生成器(Generator)

> 要创建一个生成器，有很多种方法，第一种方法是把一个列表生成式的[]改成（），就创建了一个生成器

```
# 例如把上面的
li = [ x*x for x in xrange(1, 101) if x%2 == 0 ]

# 换成下面这样，就创建了一个生成器
lt = ( x*x for x in xrange(1, 101) if x%2 == 0 )
print(type(lt))

# print(lt.next())

for i in lt:
    print(i)

```

> 另一种方法，如果一个函数定义中包含yield关键字，那么这个函数就不是一个普通函数，而是一个生成器

```
# 普通函数

def fib(n):
    num == 0
    i = 0
    while( i < n ):
        sum = sum + i
        i += 1
        print(sum)

fib(10)
```

```
# 函数中定义列表生成器

def fib(n):
    num == 0
    i = 0
    while( i < n ):
        sum = sum + i
        i += 1
        yield(sum)

print(type(fib(10))

for i in fib(10):
    print(x)

# 结果和上面的一样，但是有什么不同呢。包含yield语句的函数会被特地编译成生成器。当函数被调用时，他们返回一个生成器对象，这个对象支持迭代器接口。每当遇到yield关键字的时候，你可以理解为函数的return语句，yield后面的值，就是返回的值。但是不像一般的函数在return后退出，生成器函数在生成值后会自动挂起并暂停他们的执行和状态，它的本地变量将保持状态信息，这些信息在函数恢复时将再度有效。下次从yield下面的部分开始执行。

```

# 列表生成式和生成器的区别

**生成式：**一次性生成所有的数据，然后保存在内存中，适合小量的数据。

**生成器：**返回一个可迭代的对象，及“generator”对象，必须通过循环才可以一一列出所有结果

**可迭代对象：**可以通过循环调用出来的，就是可迭代对象

**迭代器：**必须通过next()调用的，被next()函数调用并不断返回下一个值的对象称为迭代器。

