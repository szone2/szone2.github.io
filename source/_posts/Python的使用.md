---
title: Python的使用
date: 2017-10-17 22:50:08
tags: Python
copyright: true
---

# PyCharm的使用

## 编辑器的选择

Vim编辑器（在Linux下使用）

Sublime Text

PyCharm（[下载地址](http://www.jetbrains.com/pycharm/download/download-thanks.html?platform=windows&code=PCC)）

<!--more-->

## PyCharm的设置

### 设置python解析器路径

一般情况下安装好后会自动设置

手工设置：在File->Settings->Projects->Projects Interpretor 输入路径即可

### 设置字体

File->Settings->Editor->Font 设置

### 设置新建文件时自动生成自定义的文件头信息

File->Settings->Editor->Code Style->File and Code Templates-Python Script

输入以下代码：


```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : ${DATE} ${TIME}
# @Author   : yourname
# @File     : ${NAME}.py
```

## 第一个Python程序

新建一个名为**test.py**的文件，写入如下代码：

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2017/10/17 22:04
# @Author   : yourname
# @File     : test.py

name = raw_input("Please input your name: ")
print("hello " + name)
```

按下快捷键：Alt + Shift + F10，运行**test.py**，运行界面如下：


```
E:\Python27\python.exe E:/code/test.py
Please input your name: foo
hello foo

Process finished with exit code 0
```

代码解释：

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2017/10/17 22:04
# @Author   : yourname
# @File     : test.py

# raw_input()将所有输入作为字符串看待，返回字符串类型。
name = raw_input("Please input your name: ")

# 输出字符串hello $name（两个字符串拼可以使用+号)
print("hello " + name)
```

## PyCharm左下角控制台说明

**Python Console**：iPython解析器

**Terminal**: Python解析器，和在Windows cmd窗口下输入python运行的交互界面一样

**Run**：显示程序运行结果的窗口

## 扩展部分

[pycharm 快捷键整理及一些常用设置](https://segmentfault.com/a/1190000005776418)