---
title: PyCharm的使用（二）
date: 2017-10-19 01:03:12
tags: PyCharm
copyright: ture
---

# PyCharm的使用(二)

## PyCharm设置参数

输入以下代码：

<!--more-->

```
import sys

print(sys.argv[0])
print(sys.argv[1])
```

<!--more-->

情况一：在Terminal中运行代码得到以下结果


```
E:\code>python test.py hello
test.py
hello
```

> sys.argv[0]获取到的是文件名，sys.argv[1]获取到的是hello这个参数

情况二：直接Run代码会报错，报错信息如下：


```
Traceback (most recent call last):
  File "E:/code/test.py", line 12, in <module>
    print(sys.argv[1])
IndexError: list index out of range

```

> 提示list index out of range,因为缺少第二个参数
> 按Run快捷键 Alt + Shift + F10 的时候选择Edit Configurations ，然后在“Script parameters: ” 后面填上参数 hello
> 再重新Run，即可正常运行

## PyCharm常用快捷键

ctrl + c ：快速复制一行

ctrl + d ：快速复制选中内容

ctrl + shift + n ：找查项目中的文件

ctrl + alt + l ：自动调整好代码格式

alt + enter ：查找所需的包，选中后直接import进来

ctrl + / ：注释选中的内容

shift + tab ：往左移动一个tab的距离

shift + enter ：跳到下一行

ctrl + enter  ：跳到上一行

## 如何执行Python代码

### 在linux中

```
# 方法一
python test.py

# 方法二，先给test.py加x权限，然后运行
chmod +x test.py
./test.py
```

### 在windows中

```
# 方法一，先进入python程序的目录下，然后运行
python test.py

# 方法二，在PyCharm中运行，点菜单上面的Run或使用快捷键 Alt + Shift + F10
```

### PyCharm的调试模式

> F8是从打点位置开始往下一句一句执行
> F7是从打点位置开始跳到下一个方法执行

在以下第五行代码打点
```
def hello():
    return 'hello '
if __name__ == '__main__':
    print('###' * 10)
    name = raw_input("Please input your name: ")
    print(hello() + name)
    print('###' * 10)
```

首先按下 **ctrl + enter** ，进入debug模式

接着按下F8，程序会在打点位置（即第五行代码）停止运行

再按一下F8，进入到输入input的内容，回车，停止在第六行代码（此时查看Debugger窗口可以看到name的类型和值，如下所示：

```
name = {str}'bba'
```

如果此时按F8，直接执行下一句代码（第七行）;如果此时按F7 ，则会跳到hello()这个方法中。