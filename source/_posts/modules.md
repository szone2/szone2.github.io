---
layout: port
title: modules
date: 2017-11-09 21:43:08
tags: true
---

# import

为了方便管理方便管理模块，python中又引了包（Package）这个概念。每个包下面都有一个\_\_init\_\_.py文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。\_\_init\_\_.py可以是空文件，也可以有Python代码，因为\_\_init\_\_.py本身就是一个模块，举个例子：test目录下面有\_\_init\_\_.py， aaa.py，bbb.py三个文件，如下所示：

<!--more-->

```

[root@localhost ~]# tree test
test
├── aaa.py
├── bbb.py
└── __init__.py

0 directories, 3 files

```

## import导入

有时候一个文件或者一个包中已经出现了一个函数，我们在另一个python代码中需要引入该文件或者该文件的某个函数，那怎么解决呢？python给我们提供一个关键字import，下面我们来了解一下它的用法：
1，	如果是本地导入文件，直接使用：import filename
2，	如果导入的是一个包，该包下面必须是有\_\_init\_\_.py文件才可以导入，否则报错，只有有了\_\_init\_\_.py文件，python解析器才会把这个目录当成是的包
常用的导入模块常用的格式：
form xxx import xxx
import xxx
在导入的时候，.py的后缀直接省略，如果是多级的包，或者想导入包里面的函数等，可以使用from进行导入，举个例子：
from aaa import bbb
import os
解释：第一个例子是导入aaa包下面的bbb模块或者导入aaa文件下面的bbb类或者函数
第二个import是直接导入系统模块os模块

\# import 一般我们用作导入模块来用，常用的快捷键是alt + enter可以直接导入模块

\# from ... import ... 这个是从什么模块中导入什么,最终你可以导入的是一个函数，也可以是一个类，也可以是一个模块。

总结：就是一层一层的调用就可以了

**注意** import后面导入的是什么，在调用时就必须写什么，除非你用from进行导入。

# datetime模块


```
# The distinction between naive and aware doesn’t apply to timedelta objects.

# Subclass relationships:

# object
#    timedelta
#    tzinfo
#    time
#    date
#        datetime
# time
# 在 Python 文档里，time是归类在Generic Operating System Services中，换句话说，它提供的功能是更加接近于操作系统层面的。通读文档可知，time 模块是围绕着 Unix Timestamp 进行的。

# time模块基本不用于取时间，取时间推荐使用datetime模块

# time独有的用法：

for i in xrange(1, 10):
    print(i)
    time.sleep(3)

# datetime

from datetime import datetime

now_time = datetime.now()

print(now_time)

# 自定义时间格式

new_t = now_time.strftime('%Y-%m-%d %H:%M:%S')

# 标准时间格式

a = now_time.strftime('%c')

print(new_t)

# now 获取当前的时间，strftime用来表示显示时间的格式


# 如果我们要取昨天或明天的时间，可以使用timedelay

now_time2 = datetime.now()

yesterday = now_time + timedelta(days = -1)

tomorrow_1 = now_time + timedelta(days = +1)

tomorrow = tomorrow_1.strftime('%Y-%m-%d')

print(yesterday)

print(tomorrow)

```

#  时间格式的相互转换

```
from datetime import datetime

now_time = datetime.now()
print(now_time)
print(type(now_time))

# 时间对象转换为字符串
_time = datetime.strftime(now_time, '%Y-%m-%d')
print(_time)
print(type(_time))

# 字符串转换为时间对象

_dtime = datetime.strptime(_time, '%Y-%m-%d')
print(_dtime)
print(type(_dtime))

```

```
import time

# 时间戳转换为时间对象

_a = time.time()
print(_a)

_n_time = datetime.fromtimestamp(_a)

print(_n_time)
print(type(_n_time))

```

# logging的使用

> 日志是我们排查问题的关键利器，写好日志记录，当我们发生问题时，可以快速定位代码范围进行修改。Python有给我们开发者们提供好的日志模块，下面我们就来介绍一下logging模块：

```
import logging

# 默认级别是info
# 日志级别顺序debug,info,warning,error,critical

logging.debug('this is debug message.')
logging.info('this is info message.')
logging.warning('this is warning message.')
logging.error('this is error message.')
logging.critical('this is critical message.')

```

```
import logging

logging.basicConfig(level=logging.DEBUG, format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',datefmt=' %Y/%m/%d %H:%M:%S', filename='myapp.log', filemode='w')

logger = logging.getLogger(__name__)
logging.debug('This is debug message')
logging.info('This is info message')
logging.warning('This is warning message')
结果：
在当前文件新增了一个myapp.log文件。


# 解释：
# 主要是通过logging.basicConfig函数进行操作，现在我们来介绍一下该函数参数的用法：

# level: 设置日志级别，默认为logging.WARNING
# filename: 指定日志文件名。
# filemode: 和file函数意义相同，指定日志文件的打开模式，'w'或'a'
# format: 指定输出的格式和内容，format可以输出很多有用信息，如上例所示:
#  %(levelname)s: 打印日志级别名称
#  %(filename)s: 打印当前执行程序名
#  %(funcName)s: 打印日志的当前函数
#  %(lineno)d: 打印日志的当前行号
#  %(asctime)s: 打印日志的时间
#  %(thread)d: 打印线程ID
#  %(process)d: 打印进程ID
#  %(message)s: 打印日志信息
#datefmt: 指定时间格式，同time.strftime()
#stream: 指定将日志的输出流，可以指定输出到sys.stderr,sys.stdout或者文件，默认输出到sys.stderr，当stream和filename同时指定时，stream被忽略。

# logging.getLogger(__name__)
# 返回一个logger实例，如果没有指定name，返回root logger。只要name相同，返回的logger实例都是同一个而且只有一个，即name和logger实例是一一对应的。这意味着，无需把logger实例在各个模块中传递。只要知道name，就能得到同一个logger实例。
logging.getLogger(__name__)。在上述实例中__name__就指的是__main__。

```

# os模块

```
import os

# os.name可判断当前系统类型
# linux 系统os.name是posix
# linux 系统os.name是nt
print(os.name)


# 执行系统命令

print(os.system('ifconfig'))

print(os.systtem('ipconfig'))

# popen()方法返回的是一个文件对象，我们可以通过file.read()来获取最后系统命令最终的结果

context = os.popen('ipconfig').read()
print(context.find('192.168.1.100'))

```

```
import os

#获得路径
print(os.getcwd())

#当前目录切换到d:目录下，在切换回来
os.chdir('d:')
print(os.getcwd())
# r不需要进行转义
os.chdir(r'E:\test')

#列出当前目录的文件
print(os.listdir(os.getcwd()))

#列出E:\Python27目录下的文件
print(os.listdir('E:\Python27'))

#在当前目录下创建abc目录
os.mkdir('abc')

# 删除当前目录下的1.txt文件，（如果文件不存在会报错）
os.remove('1.txt')

#打印操作系统的分隔符，linux系统的分隔符\n，windows系统的分隔符\r\n，mac系统的分隔符\r
print(os.linesep)

# E:\test\abc.txt
print(os.path.join(os.getcwd(), 'abc.txt'))

# False
print(os.path.islink(os.getcwd()))

#只是拼接，并不出创建 # E:\test\abc.txt
print(os.path.join(os.getcwd(), 'abc.txt'))

#把最后文件和目录分开# ('E:\\test', 'abc.txt')
path1 = os.path.join(os.getcwd(), 'abc.txt')
print(os.path.split(path1))

Print(os.path.splitdrive(path1))     #把最初的目录和后面分开
# ('E:', '\\test\\abc.txt')
Print(os.path.splitext(path1))       #把目录和后缀名分开
# ('E:\\test\\abc', '.txt')

# 当前目录下存在aaa目录，不创建，当前不存在aaa目录，创建aaa目录

if not os.path.exists(r'E:\test\aaa'):
    os.mkdir(r'E:\test\aaa')

#获得E:\test\test.py文件的目录
print(os.path.dirname(r'E:\test\test.py'))


```
解释：
1，	os.getcwd()	获得目录的当前系统程序工作路劲
2，	os. chdir(‘目标目录’)	切换到目标目录
3，	os.listdir(‘字符串目录’)	列出字符串目录下的所有文件
4，	os.mkdir('目录')	创建目录
5，	os.remove('1.txt')	删除文件，文件不存在时会报错
6，	os.linesep	打印操作系统的分隔符，linux系统的分隔符\n，windows系统的分隔符\r\n，mac系统的分隔符\r
7，	os.path.join(os.getcwd(), 'aaa', ‘bbb’, ‘ccc’)	拼接出来多级目录：E:\test\aaa\bbb\ccc
8，	os.path.exists(‘目录’)	判断目录是否存在
9，	os.path.split(‘文件或者目录’)	把最后的一个目录或者文件和前面的目录分开，返回一个tuple
10，os.path.splitext(‘文件’)	把文件的后缀名和前面分开，返回一个tuple

# commands模块

```
#!/usr/bin/env python
#-*-coding:utf-8-*-

# commands.getoutput 的返回值只有返回结果，没法进行判断执行结果是否正常。

import commands

cmd = 'ls /home/'

result = commands.getoutput(cmd)

print(type(result))
print(result)


# commands.getstatusoutput返回值是一个tuple类型
# 第一个值接受状态码，返回结果是一个int类型，返回值为0说明执行正常，返回值为非0说明结果异常
# 第二个值接受返回结果，返回结果是一个str类型

cmd = 'ps xf'

status, result = commands.getstatusoutput(cmd)

print(type(staus))
print(status)

print(type(result))
print(result)

```

# sys模块

```
# 新建test.py文件，写入以下内容：

import sys

if __name__ == '__main__':
    print('argv[0] = {0}     argv [1] = {1}'.format(sys.argv[0], sys.argv[1]))
    

# 执行：python test.py hello
结果：
argv[0] = E:/test/test.py     argv [1] = hello

# 解释：和其他语言一样，python的sys模块默认是把第一个参数默认是程序本省，从第二个参数起都是代码后面跟着的参数，通过sys.arg[n]就可以获得传入到程序中的参数。

sys.stdout.write('hello world\n')
print('hello')

name = raw_input("Please input your name: ")
print('hello ' + name)
address = sys.stdin.readline("Please input your address: ")
print(address)

# 从控制台重定向到文件
# 在当前文件下新生成一个文件out.log，文件内容为hello，
Import sys
f_handler=open('out.log', 'w')
sys.stdout=f_handler
print 'hello'

```