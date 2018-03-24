---
title: python基础进阶
date: 2017-12-01 22:33:36
tags: python
copyright: true
---

# 函数定义

函数就是完成特定功能的一个语句组，这组语句可以作为一个单位使用，并且给它取一个名字。

为什么使用函数？降低编程难度，通常将一个复杂的大问题分解成一系列的小问题，然后将小问题划分成更小的问题，当问题细化为足够简单时，我们就可以分而治之。各个小问题解决了，大问题就迎刃而解。

<!--more-->

代码重用 - 避免重复劳作，提供效率

函数的定义和调用：

定义函数：def 函数名([参数列表])：

调用函数：函数名([参数列表])


```

def fun():
    print "hello, world"
    
fun()

```


```
#!/usr/bin/python
#判断键盘输入是否为数字

def fun():
    sth = raw_input("Please input something: ")
    
    # 判断sth是否数字
    try:
        #如果sth是纯数字则int()不会报错
        if type(int(sth)) == type(1):
            print "%s is number" % sth
    # 否则抛出异常
    except ValueError:
        print "%s is not number" % sth

# 调用函数
fun()

```

# 函数的参数

形式参数和实际参数:

在定义函数时，函数名后面括号中的变量名称叫做形式参数，或者成为“形参”

在调用函数时，函数后面括号中的变量名称叫做“实际参数”，或者称为“实参”

```
# x, y为形式参数
def fun(x, y):
    print x + y

# 2, 3为形式参数
fun(2, 3)

```

```
# 脚本识别参数

#!/usr/bin/python

import sys

def isNum(s):
    for i in s:
        if i in '0123456789':
            pass
        else:
            print "%s is not a number" % s
            sys.exit()
    # for循环正常结束执行else后的内容
    else:
        print "%s is a number" % s

isNum(sys.argv[1])

```

# 函数的默认参数

注意默认参数只可以形参的最后

例如，不能这样定义：def fun(x=2, y):

```
def fun(x, y=100):
    print x, y

fun(1, 2)
fun(1)

```



练习：

打印系统的所有PID，要求从/proc读取。提示:os.listdir()

import os

# 返回目录下的内容（类型是列表）
os.listdir('/proc')


```
import sys
import os

def isNum(s):
    for i in s:
        if i in '0123456789':
            pass
        else:
            # 不是数字退出本次循环
            break
    # for循环正常结束执行else后的内容
    else:
    # 打印pid
        print s

for i in os.listdir('/proc'):
    isNum(i)

```

# 函数的变量

局部变量和全局变量

python中的任何变量都有特定的作用域

在函数中定义的变量一般只能在该函数内部使用，这些只能在程序的特定部分使用的变量我们称之为局部变量

在一个文件顶部定义的变量可以供文件中的任何函数调用，这些可以为整个程序所使用的变量称为全局变量


```
#!/usr/bin/python

# 全局变量
x = 'global var'

def fun():
    # x 为局部变量
    x = 100
    print x

fun()

# 在函数外部打印x
print x

```

```
#!/usr/bin/python

x = 100

def fun():
# global关键字，声明x为全局变量。如果不声明，则x在函数内部只能被print，不能做实际的操作例如x += 1
    global x
    x += 1
    print x

fun()
print x

```

```
#!/usr/bin/python
# locals返回函数的变量，如果放在函数内返回函数内的变量，如果放在外部，返回的是全部变量（类型是字典）

x = 100

def fun():
    x = 1
    y = 1
    # 统计fun函数内部的变量
    print locals()

fun()
# 统计整个程序的变量
print locals()

```

# 打印目录下所有文件

递归注意的事项：

必须有最后的默认结果(if n == 0)

递归参数必须向默认结果收敛的 factorial(n-1)

创建一个名为print_files.py文件

```
#!/usr/bin/python
# 必须有最后的默认结果：操作系统的目录的深度不会是无限的
# 递归参数必须向默认结果收敛的：一层一层地向最深的目录靠近
# 使用到的方法：isdir()、isfile()判断是否是目录、文件，返回一个布尔值；
# os.path.join连接文件路径，无判断是否存在的功能


# 使用方法：python print_files.py 目录名

import os
import sys

def print_files(path):
    lsdir = os.listdir(path)
    # 列表重写,作用是过滤出目录。通过path和i连接出目录的路径
    dirs = [i for i in lsdir if os.path.isdir(os.path.join(path, i))]
    # 列表重写，作用是过滤出文件
    files = [i for i in lsdir if os.path.isfile(os.path.join(path,i))]
    
    #判断是否有子目录，如果有子目录，对子目录做遍历
    if dirs:
        for d in dirs:
            #每个子目录在递归调用这个函数
            print_files(os.path.join(path, d))
    
    #把文件打印出来
    if files:
        for f in files:
            print os.path.join(path, f)
            
print_files(sys.argv[1])

```

# 匿名函数

lambda函数是一种快速定义单行的最小函数，可以用在任何需要函数的地方。

匿名函数的优点：

使用python写一些脚本时，使用lambda可以省去定义函数的过程，让代码更加精简。

对于一些抽象的，不会被别的地方再重复使用的函数，有时候函数起个名字也是个难题，使用lambda不需要考虑命名的问题

使用lambda在某些时候让代码更容易理解

```

# lambda语句中，冒号前是参数，可以有很多歌，逗号隔开，冒号右边是返回值。
# lambda语句构建的其实是一个函数对象

def fun(x, y):
    return x * y
fun(2, 3)

r = lambda x, y: x * y
r(2, 3)

```

```
reduce(lambda x, y: x + y, range(1, 101))

```

# 模块使用

模块是python组织代码的基本方式

一个python脚本可以单独运行，也可以导入到另一个脚本中运行，当脚本被导入运行时，我们将其称为模块（moudule）

所有的.py文件都可以作为一个模块导入

模块名与脚本的文件名相同

例如我们编写了一个名为hello.py的脚本，则可以在另一个脚本中以import hello来导入

python的模块可以按目录组织为包

创建包的步骤：

创建一个名字为包名的目录

在该目录下创建一个\_\_init.py\_\_文件

根据需要，在该目录下存放脚本文件或已编译的扩展及子包

import pack.m1, pack.m2, pack.m3

---

sys.path可以看到模块的位置(返回一个list)

export PYTHONPATH


```
#!/usr/bin/python

def wordCount(s):
    chars = len(s)
    words = len(s.split())
    lines = s.count()
    print lines, words, chars

s = open('/etc/passwd').read()
worldCount(s)

```

# 面向对象介绍

面向对象是一种编程方法。在python里一切皆对象。

面向过程编程：函数式编程，C程序等

面向对象编程： C++，java，Python等

类和对象：是面向对象中的两个重要概念

类： 是对事物的抽象，比如：球类

对象：是类的一个实例，比如：足球、篮球

实例说明：球类可以对球的特征和行为进行抽象，然后可以实例化一个真实的球实体出来。

面向对象的主要思想是：封装、继承、多态

# 类的定义

类把需要的变量和函数组合成一起，这种包含称为”封装“

class A(object):

类的结构：

class 类名：
    成员变量-属性
    成员方法-函数
    
class MyClass(object):
    def fun(self):
        print "I am function"
类的方法中至少有一个参数self,self表示类自己

对象的属性和方法与类中的成员变量和成员函数对应

obj = MyClass() //创建类的一个实例（对象）通过对象类调用方法和属性

```
#!/usr/bin/python

class People(object):
    # 静态属性
    color = 'yellow'
    
    def think(self)：
        # 使用上面的属性，self表示自身
        self.color = "black"
        print "I am a %s" % self.color
        print "I am a thinker."
    

#类的实例化（创建一个对象）。赋给变量ren。ren的类型是对象
ren = People()

print ren.color
ren.think()

```

# 类的属性

类的属性按使用范围分为公有属性和私有属性，类的属性范围取决于属性的名称。

公有属性：在类中和类外都能调用的属性。

私有属性：不能在类外及被类意外的函数调用

定义方式：以"__"双下划线开始的成员变量就是私有属性

可以通过instance（实例）._classname__attribute方式访问。

内置属性：由系统在定义类的时候默认添加的，由前后双下划线构成，\_\_dict\_\_,\_\_module\_\_

```
#!/usr/bin/python
# _*_ coding:utf8 _*_

class People(object):
    # 静态属性
    color = 'yellow'
    #私有属性
    __age = 30
    
    def think(self)：
        # 使用上面的属性，self表示自身
        self.color = "black"
        print "I am a %s" % self.color
        print "I am a thinker."
        print self.__age
    

#类的实例化（创建一个对象）。赋给变量ren。ren的类型是对象
ren = People()

ren.color = '白色人'
print ren.color

ren.think()
# 通过对象调用
print ren.__dict__
print '#' *30
print People.color
print '#' *30
# 通过类调用
print People.__dict__

```

# 类的方法

方法的定义和函数一样，但是需要self作为第一个参数

类方法为：

公有方法，在类中和类外都能调用的方法

私有方法，不能被类的外部调用，在方法前面加上"__"双下划线就是私有方法

类方法，被classmethod()函数处理过的函数，能被类所调用，也能被对象所调用（是继承的关系）

静态方法，相当于“全局函数”，可以被类直接调用，可以被所有实例化对象共享，通过staticmethod()定义，静态方法没有“self”参数。

装饰器：
@classmethod
@staticmethod

```
#!/usr/bin/python
# _*_ coding:utf8 _*_

class People(object):
    # 静态属性
    color = 'yellow'
    #私有属性
    __age = 30
    
    # 公有方法
    def think(self)：
        # 使用上面的属性，self表示自身
        self.color = "black"
        print "I am a %s" % self.color
        print "I am a thinker."
        print self.__age
    
    # 私有方法
    def __talk(self):
        print "I am talking with Tom"
    
    # 公有方法
    def test(self):
        print 'Testing...'
    
    # 类方法
    cm = classmethod(test)
        
jack = People()
People.cm()

# 尝试通过对象来访问私有对象,结果是不可以的
jack.__talk()

```


