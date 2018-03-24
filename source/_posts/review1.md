---
title: 复习1
date: 2017-10-30 23:44:52
tags: review python
copyright: true
---


```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2017/10/30 21:48
# @Author   : keshen
# @File     : review1.py

# 第一题：把一个数字的list从小到大排序，然后写入文件，然后从文件中读取出来文件内容，然后反序，在追加到文件的下一行中
<!--more-->
# 方法1：
#
# import codecs
#
# l = [2, 32, 43, 453, 54, 6, 576, 5, 7, 6, 8, 78, 7, 89]
# new_l = sorted(l)
#
# with codecs.open('1.txt', 'wb') as f:
#     for i in new_l:
#         f.write("{0} ".format(i))
#
# with codecs.open('1.txt', 'rb') as ff:
#     str = ff.readline()
#     l_tmp = str.split()
#
# with codecs.open('1.txt', 'ab') as fff:
#     l_tmp.reverse()
#     fff.write('\n')
#     for i in l_tmp:
#         fff.write("{0} ".format(i))

# 方法2：
import codecs


# 把一个数字的list从小到大排序，然后写入文件（默认文件名为result.txt）:
def l_write(l, filename = "result.txt"):
    new_l = sorted(l)
    if new_l:
        for i in new_l:
            if isinstance(i, int):
                pass
            else:
                print("This is not a list of numbers.")
                return 1
        with codecs.open(filename, 'wb') as f:
            for i in new_l:
                f.write("{0} ".format(i))
    else:
        print("This list is None.")
    return 0


# 从文件中读取出来文件内容，然后反序，在追加到文件的下一行中
def read_add(filename = "result.txt"):
    with codecs.open(filename, 'rb') as f:
        str = f.readline()
        l_tmp = str.split()

    with codecs.open(filename, 'ab') as ff:
        l_tmp.reverse()
        ff.write('\n')
        for i in l_tmp:
            ff.write("{0} ".format(i))

if __name__ == "__main__":
    li = [2, 32, 43, 453, 54, 6, 576, 5, 7, 6, 8, 78, 7, 89]
    l_write(li, '2.txt')
    read_add('2.txt')


# 分别把 string， list， tuple， dict写入到文件中
# import codecs
#
# str = "abcdefghijklmn"
# l = [2, 32, 43, 453, 54, 6, 576, 5, 7, 6, 8, 78, 7, 89]
# t = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
# d = {'name':'alice', 'age':'100', 123:'abc'}
#
# with codecs.open('file1.txt', 'wb') as f:
#     f.write("{0} \n".format(str))
#     f.write("{0} \n".format(l))
#     f.write("{0} \n".format(t))
#     f.write("{0} \n".format(d))


```