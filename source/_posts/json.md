---
title: json
date: 2017-11-13 21:15:36
tags: python
copyright: true
---

# json

> json,全名：JavaScript Object Notation,是一种轻量级的数据交换格式。

<!--more-->

json类型和python类型相互转换

json的四种方法（s结尾的是用来处理字符串的，非s结尾的就是用来处理文件的：

json.loads、json.dumps、json.load、json.dump

load/loads (加载)，就是把json转换成其他格式，字符串或者文件相关的。

dump/dumps （颠倒），就是把其他对象或格式，转换成json格式。

> json在线解析及格式化网站：json.cn

## json和字符串之间的转换

```
# 例子1，把python的dict格式转换成json字符串格式

import json

a = dict('name' = 'alice', 'age' = '25', 'message' = 'you are so cool.' )
print(a)
print(type(a))

b = json.dumps(a)
print(b)
print(type(b))


# 例子2, 把json转换成dict格式

c = json.loads(b)
print(type(c))
print(c)
print(c['name'])

```

## json和文件之间的转换

```
# import json

#json.dump()可以把json数据直接写入到文件中。

jsonData = '{"a":1,"b":2,"c":3,"d":4,"e":5}'

f = open('a.txt', 'w')
json.dump(jsonData,f)
f.close()

#结果：生成a.txt文件，内容如下："{\"a\":1,\"b\":2,\"c\":3,\"d\":4,\"e\":5}"

```

```
# json.load()把文件内容转换成unicode数据类型返回

jsonData = '{"a":1,"b":2,"c":3,"d":4,"e":5}'

print(type(jsonData))

f = open('a.txt', 'w')
json.dump(jsonData,f)
f.close()

aa = open('a.txt', 'r')
dict1=json.load(aa)
print(dict1)
print(type(dict1))

# 结果：{"a":1,"b":2,"c":3,"d":4,"e":5}
# <type 'unicode'>

```


