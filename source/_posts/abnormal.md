---
title: abnormal
date: 2017-11-06 23:33:27
tags: python
copyright: true
---

# 异常常用形式

异常即是一个事件，该事件会在程序执行过程中发生，影响了程序的正常执行。

一般情况下，在Python无法正常处理程序时就会发生一个异常。异常是Python对象，表示一个错误。当Python脚本发生异常时我们需要捕获处理它，否则程序会终止执行。

<!--more-->

```
#try:
#    正常的操作
#except
#    发生异常，执行这块代码。无异常不执行这块代码
#else：
#    如果没有异常执行这块代码
#finally:
#    不管如何，最后一定要执行的代码

# 例子：

try:
    a = 10
    b = 0
    a/b
# Exception是所有异常类的基类
except Exception as e:
    print(e)
else:
    print("this is ok ！")
finally:
    print("end.")

```

```
# 例子:

a = [1, 2, 3]
try:
    print(a[4])
except IndexError as e:
    print(e)

```

# 异常的处理方法

Exception是所有的异常的基础类（），对于python的标准异常，我们列出如下，以做参考：


异常名称 | 描述
---|---
BaseException  |  所有异常的基类
SystemExit  |  解释器请求退出
KeyboardInterrupt  |  用户中断执行(通常是输入^C)
Exception  |  常规错误的基类
StopIteration  |  迭代器没有更多的值
GeneratorExit  |  生成器(generator)发生异常来通知退出
StandardError  |  所有的内建标准异常的基类
ArithmeticError  |  所有数值计算错误的基类
FloatingPointError  |  浮点计算错误
OverflowError  |  数值运算超出最大限制
ZeroDivisionError  |  除(或取模)零 (所有数据类型)
AssertionError  |  断言语句失败
AttributeError  |  对象没有这个属性
EOFError  |  没有内建输入,到达EOF 标记
EnvironmentError  |  操作系统错误的基类
IOError  |  输入/输出操作失败
OSError  |  操作系统错误
WindowsError  |  系统调用失败
ImportError  |  导入模块/对象失败
LookupError  |  无效数据查询的基类
IndexError  |  序列中没有此索引(index)
KeyError  |  映射中没有这个键
MemoryError  |  内存溢出错误(对于Python 解释器不是致命的)
NameError  |  未声明/初始化对象 (没有属性)
UnboundLocalError  |  访问未初始化的本地变量
ReferenceError  |  弱引用(Weak reference)试图访问已经垃圾回收了的对象
RuntimeError  |  一般的运行时错误
NotImplementedError  |  尚未实现的方法
SyntaxError  |  Python 语法错误
IndentationError  |  缩进错误
TabError  |  Tab 和空格混用
SystemError  |  一般的解释器系统错误
TypeError  |  对类型无效的操作
ValueError  |  传入无效的参数
UnicodeError  |  Unicode 相关的错误
UnicodeDecodeError  |  Unicode 解码时的错误
UnicodeEncodeError  |  Unicode 编码时错误
UnicodeTranslateError  |  Unicode 转换时错误
Warning  |  警告的基类
DeprecationWarning  |  关于被弃用的特征的警告
FutureWarning  |  关于构造将来语义会有改变的警告
OverflowWarning  |  旧的关于自动提升为长整型(long)的警告
PendingDeprecationWarning  |  关于特性将会被废弃的警告
RuntimeWarning  |  可疑的运行时行为(runtime behavior)的警告
SyntaxWarning  |  可疑的语法的警告
UserWarning  |  用户代码生成的警告

# raise关键字的使用

raise用来触发异常，语法如下：

> raise [Exception [, args [, traceback]]]

语句中Exception是异常的类型（例如，NameError）参数是一个异常参数值。该参数是可选的，如果不提供，异常的参数是"None"。
最后一个参数是可选的（在实践中很少使用），如果存在，是跟踪异常对象。


```
try:
    10/0
except Exception as e:
    print 'aaaaaaa', e
    raise e
else:
    print('ok')
finally:
    print('finally')
print('hello world')

```

结果：
aaaaaaa integer division or modulo by zero
Traceback (most recent call last):
finally
  File "E:/test/test.py", line 11, in <module>
    raise e
ZeroDivisionError: integer division or modulo by zero

解释：
raise关键字就是捕获到异常，并抛出。程序运行终止。最后的hello world没有打印出来。但是finally还是会执行的。
