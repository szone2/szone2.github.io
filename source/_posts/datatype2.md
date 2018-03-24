---
title: Python数据类型（二）
date: 2017-10-23 22:41:39
tags: python
copyright: ture
---

# 列表

列表由一序列特定顺序排列的元素组成的，可以把字符串，数字，字典等任何东西加入到列表中，列表中的元素之间没有任何关系。

列表也是自带下标的，默认也还是从0开始

<!--more-->

```
str1 = 'sdfesfssfsfsf'
print(list(str1))
a = ['a', 'b', 'c', 123]
print(a)
print(type(a))

# 查看list有哪些方法
#print(dir(a))

# append, list末尾追加
a.append('hello')
print(a)

# pop list末尾删除
print('pop' * 30)
a.pop()
print(a)

# remove 删除list一个元素
print(a)
a.remove('c')
print(a)

# index
print(a[0], a[1], a[2])
print(a.index('abc'))

# insert，往list中插入一个元素
a.insert(0, 'sdfse')
print(a)
a.insert(3, 'sdfasdfae')
print(a)

# sort 排序
print(a)
a.sort()
print(a)

# reverse 反序

# 切片，注意：切片的时候，取的是最后一位数字减一
print(a[3:])
print(a[1:5])

#1是从下标为几开始取，5表示下标为几时结束，2表示取的间隔是多少
print(a)
print(a[1:5:2])

```

# 元组

tuple可以说是不可变的list，但是tuple还是和list有些差别

tuple和list的比较明显的区别就是[]变成了()，其他没什么变化，tuple不能增加和减少，它本身自己的方法很少，**只有count和index两个方法**，其他的list方法都**不可用**。

```
t = ('a', 'b', 'c', 'abc', 1, 2, 3)
print(dir(t))
print(type(t))

```

```
str1 = 'sdfasdfaefasfasefasegfasdgsd'
print(tuple(str1))
a = ('a', 'b', 'c', 'abc', 123)

# 单个tuple的时候要注意：单个tuple元素的时候，元素后面要加，否则python解析器不会识别为tuple类型
m = ('abc')
print(type(m))
n = ('abc',)
print(type(n))
```


```
# tuple的方法： count index
print(dir(x))

# count ，统计某个元素的个数
tul = ('a', 'b', 'c', 'd', 'a')
print(tul.count('a')

# index，返回某个元素的下标
print(tul.index('b'))

```

# 字典

字典是另一种可变容器模型，且可存储任意类型对象。

字典的每个键值(key => value)对用冒号(:)分割，每个对之间用逗号(,)分割，整个字典包括在花括号{}中，字典赋值有三种方式：

```
k = {'name':'alice', 'age':'100', 123:'abc'}
d = dict(a = 1, b = 2, c = 3)
d = dict([('name', 'list'), ('age',20)])
```


```
# 字典常用的方法
print(dir(d))

# get方法
print(k,get('name'))
print(k,get('age'))

# setdefault 设置默认value

print(k.setdefault('name')
print(k.setdefault('age')
print(k.setdefault('address', 'beijing')

# keys
print(k.keys())

# values
print(k.values())

# iteritems 遍历
print(k.iteritems())
for x, y in k.iteritems():
    print(x, y)

# pop 删除指定给定键所对应的值，返回这个值并从字典中把它移除。
k.pop('address')
k.pop('name')
print(k)

```


```
# fromkeys 从一个列表中获得keys
l = ['a', 'b', 'c', 'd']
n = dict.fromkeys(l, 123)
print(n)

# zip 把两个列表合成一个字典
l1 = ['a', 'b', 'c', 'd']
l2 = [1, 2, 3, 4]
dict_test = dict(zip(l1, l2))
print(dict_test)

k = {'name':'alice', 'age':'100', 123:'abc'}
print(k)
dict_test.update(k)
print(dict_test)
```

```
# 对字典进行排序
mm = dict(a=1, b=2, c=3, d=9)
print(mm)
print sorted(mm.iteritems(), key = lambda d: d[1], reverse = False）

```