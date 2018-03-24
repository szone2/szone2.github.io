---
title: python基础
date: 2017-11-28 21:59:42
tags: 复习
copyright: true
---

# pyhon的文件类型

类型一：源代码

python源代码文件以py为扩展名，由python程序解释，不需要编译

<!--more-->

类型二：字节代码

python源码文件经编译后生成的扩展名为pyc的文件

编译方法：

import py_compile

py_compile.compile('hello.py')

类型三：优化代码

经过优化的源码文件，扩展名为"pyo"

-O 表示优化，-m表示因为模块py_compile
python -O -m py_compile hello.py

# python的变量

## 变量的概念

变量是计算机内存中的一块区域，变量可以存储规定范围内的值，而且值可以改变

python下变量是对一个数据的引用

## 变量的命名

变量名由字母、数字、下划线组成

变量不能以数字开头

不可以使用关键字

变量的赋值是变量的声明和定义的过程

## 运算符与表达式

赋值运算符、算术运算符、关系运算符、逻辑运算符

表达式是将不同的数据（包括变量、函数）用运算符号按一定规则连接起来的一种式子

## 算术运算符

+ - \* / // % \*\*

## 逻辑运算符

and : True and False

or ： False and True

not : not True

## 练习

写一个四则运算器(加减乘除)

要求从键盘读取数字

```
#!/usr/bin/python

num1 = input("Please a number: ")
num2 = input("Please a number: ")

print "%s + %s = %s" %(num1, num2, num1+num2)
print "%s + %s = %s" %(num1, num2, num1-num2)
print "%s + %s = %s" %(num1, num2, num1*num2)
print "%s + %s = %s" %(num1, num2, num1/num2)

```

# Python的数值和字符串

## 数值类型

整型、长整型、浮点型（0.0, 12.0, -18.8, 3e+7)、复数型（后面多个j：例如23j）

## 字符串类型

有三种方法定义字符串类型

str = 'this is string'

str = "this is string"

str = '''this is string'''

注： 三重印号除了能定义字符串还可以用作注释。

# if

if expression
    statement(s)

```
#!/usr/bin/python

# not优先级比and高

if not 1 > 2 and 1 == 1:
    print 'hello python'
    print 'True'
elif 'a':
    print a
else:
    print 'ha'
print 'main'

```

```
#!/usr/bin/python

score = raw_input("Please a num: ")
if score >= 90:
    print 'A'
    print 'very good'
elif score >= 80:
    print 'B'
    print 'good'
elif score >= 70:
    print 'C'
    print 'pass'
else:
    print 'D'

print 'End'

```

逻辑值(bool)包含了两个值：

True：表示非空的量(比如:string, tuple, list, set dictonary), 所有非零数

False: 表示0, None, 空的量等

# for遍历列表

for循环：

在序列里，使用for循环遍历

语法：

for iterating_var in sequence:
    statement(s)

步长为2：
range(0, 10, 2)


```
#!/usr/bin/python

print [i for i in range(1, 11) if i % 2 == 0]

```

```
#!/usr/bin/python

# 先对i进行遍历，然后筛选出符合条件（if i % 2 != 0）的，最后进行操作(i乘方)

for i in [i**2 for i in range(1, 11) if i % 2 != 0]:
    print i,

```

# for遍历字典

```
#!/usr/bin/python
dic = {'a':1, 'b':100, 'c':200}

for k in dic:
    print k, dic[k]

# items方法，返回的是一个列表
for k, v in dic.items():
    print k, v
    
# iteritems方法,返回的是对象
for k, v in dic.iteritems():
    print k, v

```

```
# 乘法表

#!/usr/bin/python

# 外层的for控制有9行

for i in xrange(1, 10):
# 内层for控制列
    for j in xrange(1, i+1):
        # pirnt默认带有换行符 ,号不让输出换行符
        print "%s x %s = %s" % (j, i, j*i),
    # 内层循环结束后再输出换行符
    print


```

# 文件访问

open

r: 以读方式打开

w: 以写方式打开

a: 以追加模式

r+: 以读写模式打开

w+: 以读写模式打开

a+: 以读写模式打开

rb: 以二进制读模式打开

wb: 以二进制写模式打开

ab: 以二进制追加模式打开

rb+: 以二进制读写模式打开

wb+: 以二进制读写模式打开

ab+: 以二进制读写模式打开

with open

## for循环遍历文件

```
# /tmp/tmp.txt内容：
# 123 456

#!/usr/bin/python

# fd.readlines生成一个列表，占用内存
fd = open('/tmp/tmp.txt')
for line in fd.readlines():
# 加,抑制print自带的换行
    print line,

```

```
#!/usr/bin/python

# fd是一个文件对象
fd = open('/tmp/tmp.txt')
# for循环可以对fd做遍历，遍历一次取一行，比上面那种节省内存
for line in fd:
# 加,抑制print自带的换行
    print line,
fd.close()

```

## while循环遍历文件

```
#!/usr/bin/python

fd = open('/tmp/tmp.txt')
while True:
# readline一行一行读
    line = fd.readline()
#当读到空时结束，文件结尾为空
    if not line:
        break
    print line,
fd.close()

```

```
#!/usr/bin/python

with open('/tmp/tmp.txt') as fd:
    while True:
    # readline一行一行读
        line = fd.readline()
    #当读到空时结束，文件结尾为空
        if not line:
            break
        print line,
    fd.close()

```

# 统计系统剩余的内存

```
# free命令其实是查看/proc/meminfo文件
# 字符串的方法startswith，判断是以什么开头的，返回一个布尔值
#!/usr/bin/python

with open('/proc/meminfo') as fd:
    for line in fd:
        # 总内存
        if line.startswith('MemTotal')
        # 字符串切片
            total = line.split()[1]
        # 如果满足条件则不用执行下面的语句
            continue

        if line.startswith('MemFree'):
            free = line.split()[1]
            break

# free是字符串类型，加int转换为整型,格式化输出%.2f保留两位小数
print "%.2f" % (int(free)/1024.0)+'M'

```
