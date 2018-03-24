---
title: 正则
date: 2017-11-14 23:07:14
tags: python
copyright: true
---

# 正则

## 常用正则表达式

正则表达式本身是一种小型的、高度专业化的编程语言，而在python中，通过内嵌集成re模块，程序员们可以直接调用来实现正则匹配。正则表达式模式被编译成一系列的字节码，然后由用C编写的匹配引擎执行。

<!--more-->

![正则表达式](http://ww4.sinaimg.cn/large/0060lm7Tly1fli0fbc4yuj30m71brdkp.jpg)

> 正则表达式测试网站：www.regex101.com

## re正则对象和正则匹配效率比较

> re 模块是Python中处理正则表达式的一个模块，通过re模块的方法，把正则表达式pattern编译成正则对象，以便使用正则对象的方法。使用re模块可以提高代码的执行效率。

```
import timeit
print timeit.timeit(setup='''import re; reg = re.compile('<(?P<tagname>\w*)>.*</(?P=tagname)>')''', stmt='''reg.match('<h1>xxx</h1>')''', number=1000000)
print timeit.timeit(setup='''import re''', stmt='''re.match('<(?P<tagname>\w*)>.*</(?P=tagname)>', '<h1>xxx</h1>')''', number=1000000)

# 结果：
# 0.504413156462
# 1.20703154234
# 解释：timeit.timeit是用来统计程序执行的时间的，和明显第一个print的执行时间要比第二个的执行时间快好多，这个就是把正则表达是表示成正则对象最明显的好处。

```

## 编译正则对象

正则匹配总写一个r是什么意思？

r表示raw的简及raw string 意思是原生字符，也就是说是这个字符串中间的特殊字符不用转义
比如你要表示‘\n’,可以这样：r'\n'
但是如果你不用原生字符 而是用字符串你得这样：‘\\\n’

### re.compile(pattern[, flags])

这个方法是就是将字符串的正则表达式编译城正则对象，第二个参数flag是匹配模式，取值可以使用按位或者运算符“|”表示同时生效，比如：re.I | re.M，flag的可选值有：

re.I(re.IGNORECASE): 忽略大小写（括号内是完整写法，下同）

M(MULTILINE): 多行模式，改变'^'和'$'的行为

S(DOTALL): 点任意匹配模式，改变'.'的行为

L(LOCALE): 使预定字符类 \w \W \b \B \s \S 取决于当前区域设定

U(UNICODE): 使预定字符类 \w \W \b \B \s \S \d \D 取决于unicode定义的字符属性

X(VERBOSE): 详细模式。这个模式下正则表达式可以是多行，忽略空白字符，并可以加入注释。以下两个正则表达式是等价的：

a = re.compile(r"""\d +  # the integral part

\\.    # the decimal point
                   
\d *  # some fractional digits""", re.X)
                   
b = re.compile(r"\d+\\.\d*")

```
import re

#正则匹配时，优先编译成正则对象，然后再进行匹配，这样程序的效率更高。

reg = re.compile('abc.*')

#reg是一个正则对象
print(type(reg))
print(reg)

```

## re的matche方法和search方法

match(string[, pos[, endpos]])

string:匹配使用的文本，

pos: 文本中正则表达式开始搜索的索引。及开始搜索string的下标

endpos: 文本中正则表达式结束搜索的索引。

如果不指定pos，默认是从开头开始匹配，如果匹配不到，直接返回None

```
# match(string[, pos[, endpos]])

import re
pattern = re.compile(r'\w*(hello w.*)(hello a.*)')
result = pattern.match(r'aahello world hello alice')
print(result)
result2 = pattern.match(r'hello world hello alice')
print(result2.groups())

#结果：
#None
#('hello world ', 'hello ling')
#解释：如果不指定pos的话，默认是从字符串开始位置匹配，匹配不到就返回None，以上所有的pattern都是一个match对象，他在查找结果的时候有自己的方法。

```
search(string[, pos[, endpos]])

这个方法用于查找字符串中可以匹配成功的子串。从string的pos下标处起尝试匹配pattern，如果pattern结束时仍可匹配，则返回一个Match对象；若无法匹配，则将pos加1后重新尝试匹配；直到pos=endpos时仍无法匹配则返回None。下面看个列子：

```

import re
pattern = re.compile(r'(hello w.*)(hello a.*)')
result1 = pattern.search(r'aahello world hello alice')
print(result1.groups())
结果：
('hello world ', 'hello alice')

```

## re的split, findall, finditer方法

```
import re

#\d+ 匹配数字
p1 = re.compile(r'\d+')

a_str = 'one1two222three3four4'


# 正则对象的split方法，使用正则匹配进行分割
# 最后以列表的形式返回

print(p1.split(a_str))


# 正则对象的findall方法，查找符合对象的字符串
# 最后以列表的形式返回

print(p1.split(a_str))

# finditer
# p.finditer(a_str)是一个迭代器，而返回的每个i都是match对象，group方法会在下一节进行详细介绍。

for i in p1.finditer(a_str):
    print(i.group())

```

## re的matche对象

> Match对象是一次匹配的结果，包含了很多关于此次匹配的信息，可以使用Match提供的可读属性或方法来获取这些信息。上面的过程中多次使用了match对象，调用了他的group()和groups()等方法。

例子:

```
import re
prog = re.compile(r'(?P<tagname>abc)(.*)(?P=tagname)')
result1 = prog.match('abclfjlad234sjldabc')
print(result1)
print(result1.groups())
print result1.group('tagname')
print(result1.group(2))
print(result1.groupdict())

```
结果：

<_sre.SRE_Match object at 0x0000000002176E88>

('abc', 'lfjlad234sjld')

abc

lfjlad234sjld

{'tagname': 'abc'}


解释：

我们可以看到result1已经由字符串转换成了一个正则对象。

resule.groups()可以查看出来所有匹配到的数据，每个()是一个元素，最终返回一个tuple进行访问。

groupdict只能显示有分组名的数据

group([group1, …]): 
获得一个或多个分组截获的字符串；指定多个参数时将以元组形式返回。group1可以使用编号也可以使用别名；编号0代表整个匹配的子串；不填写参数时，返回group(0)；没有截获字符串的组返回None；截获了多次的组返回最后一次截获的子串。

groups([default]): 
以元组形式返回全部分组截获的字符串。相当于调用group(1,2,…last)。default表示没有截获字符串的组以这个值替代，默认为None。

groupdict([default]): 
返回以有别名的组的别名为键、以该组截获的子串为值的字典，没有别名的组不包含在内。default含义同上。



