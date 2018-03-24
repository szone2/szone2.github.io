---
title: Python简介
date: 2017-10-17 22:56:18
tags: Python
copyright: true
---

# 安装Python

## 预备知识
- 大多数Linux系统自带Python解析器（CentOS7自带Python版本为2.7）。
- 目前比较流行的Python版本为2.7。
- 如果要使用Python3，只需学习两者（Python3和Python2）之间的差异部分。
- 学习的时候使用CentOS自带的Python即可。

<!--more-->

## Linux下安装Python

```
wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz
tar zxf Python-2.7.14.tgz
cd Python-2.7.14
./configure
make && make install
```

## Windows下安装Python

### 下载Python

- 在浏览器打开[Python官网](http://www.python.org/)，找到并下载Windows的安装包：[下载链接](https://www.python.org/ftp/python/2.7.14/python-2.7.14.amd64.msi)。
- 双击运行并安装（记下安装目录，其他保持默认即可）

### 配置Python的环境变量

- 开始菜单
- 控制面板\所有控制面板项\用户帐户
- 更改我的环境变量
- 系统变量，找到变量名为Path，在变量值的最前面添加E:\Python27;E:\Python27\Scripts;（Python安装目录和Scripts目录，注意要使用**英语分号**与其他变量分隔)

### 测试Python

- 使用组合键：__windows + r__ 打开运行窗口，然后输入cmd (打开cmd命令提示符)
- 在命令提示符中输入python，回车，如果出现下面的情况就说明安装成功：

```
Python 2.7.14 (v2.7.14:a06454b1afa1, Dec 17 2016, 20:53:40) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

### 退出Python
- 输入exit()，回车即可。

## Mac下安装Python

### homebrew
```
brew install python
```

> 说明：Mac中的brew类似于CentOS中的yum命令

### 从官网下载安装

- 在浏览器打开[Python官网](http://www.python.org/)，找到并下载Mac的安装包：[下载链接](https://www.python.org/ftp/python/2.7.14/python-2.7.14-macosx10.6.pkg)。
- 双击运行并安装
- 查看Python安装路径可使用以下命令：

```
which python
```

### 测试Python

- 在Terminal中输入python验证即可。

## hello world小程序

### 使用Python交互式命令行(Windows)

- 在交互式环境的提示符>>>下，直接输入代码，按回车，就可以立刻得到代码执行结果。


```
>>> print('hello, world')
hello, world
```


### 使用文本编辑器

> 在Python的交互式命令行写程序，好处是一下就能得到结果，坏处是没法保存，下次还想运行的时候，还得再敲一遍。所以，实际开发的时候，我们总是使用一个文本编辑器来写代码，写完了，保存为一个文件，这样，程序就可以反复运行了。

- 使用文本编辑器Sublime Text或Notepad++，输入以下代码：

```
#!/usr/bin/env python

print("hello, world")
```

- 然后，选择一个目录，例如C:\work，把文件保存为hello.py，就可以打开命令行窗口，把当前目录切换到hello.py所在目录，就可以运行这个程序了：


```
C:\work>python hello.py
hello, world
```