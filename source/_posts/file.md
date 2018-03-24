---
title: 文件处理
date: 2017-10-27 22:23:17
tags: python
copyright: true
---
# 读取文件内容

当前目录下有一个test.txt文件，文件内容如下：

<!--more-->

111111111111
222222222222
333333333333
444444444444
555555555555
666666666666

```
import codecs

# 打开文件需要几步：
# 1. open文件（codecs 这个模块主要用来解决文件乱码问题的类）
# 2. 文件操作（读或者写）
# 3. 关闭文件
f = codecs.open('test.txt')
txt = f.read()
print(type(text)
result = txt.replace('1', 'A')
print(result)
# print(f.read())
f.close()

```

# 写文件

```
# open(filename, mode)
# mode 参数：r读(默认），w写，b二进制, a 追加

import codes

f = codecs.open('2.txt', w)
f.write('hello, world. \n')
f.close()

```

# file常用方法


```
# readlines方法:读取文件内容，文件内容的每一行都是一个字符串，最后返回一个list
f = codecs.open('2.txt', 'rb')
print(dir(f))

text_list = f.readlines()
print(type(text_list))
print(text_list)

f.close()


```

```
# readline 读取文件一行内容，返回一个字符串
# next() 读取文件下一行内容返回一个字符串

f = codecs.open('2.txt', 'rb')
print(f.readline())
print(f.next())

f.close()

```

```
# write() 参数必须是字符串
# writelines()参数必须是一个序列

f = codecs.open('file3.txt', 'wb')

f.write('hello, world\nsdfasdfasefaf\n')

f.writelines(['aaa\n', 'bbb\n', 'ccc\n'])

f.close()

```

```

f = codecs.open('file4.txt', 'wb')

f.write('hello world\nfasdfaef\n')

print(f.tell())

f.writelines(['aaaaaa', 'bbbbbb'. 'ccccc'])

print(f.tell())

# seek(0)光标回到文件开始位置

f.seek(0)

file.write('aaaaaaaaaaaaaaaaaaaaaaaaaaa\n')

print(f.name)

f.close()

```

# file的with用法

```
import codecs

with codecs.open('1.txt', 'rb') as f:
    print(f.read())
    print(f.closed)
print(f.closed)

```

```
# 打印文件行号

with codecs.open('1.txt', 'rb') as f:
    for line, value in enumetare(f):
        # print(line, value)
        if line == 4-1:
            print(value)

```

```
# 打印指定行

import linecache

count = linecache.getline('1.txt', 4)
print(count)

```