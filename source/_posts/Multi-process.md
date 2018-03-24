---
title: 多进程
date: 2017-12-08 23:26:44
tags: python
copyright: true
---

# 多进程多线程概念

进程是程序在计算机上的一次执行活动。当你运行一个程序，你就启动了一个进程。显然，程序是死的（静态的），进程是活的（动态的）。进程可以分为系统进程和用户进程。凡是用于完成操作系统的各种功能的进程就是系统进程，它们就是处于运行状态下的操作系统本身。所有由你启动的进程都是用户进程。进程是操作系统进行资源分配的单位。

<!--more-->

它的思想简单介绍如下：在操作系统的管理下，所有正在运行的进程轮流使用CPU，每个进程允许占用CPU的时间非常短（比如10毫秒），这样用户根本感觉不出来CPU是在轮流为多个进程服务，就好像所有的进程都在不间断地运行一样，但实际上在任何一个时间内有且仅有一个进程占有CPU。

多进程和多线程的区别：

多线程使用的是CPU的一个核，适合io密集型
多进程使用的是CPU的多个核，适合运算密集型

组件：

Python提供了非常好用的多进程包，multiprocessing,使用时，只需导入模块就可以了

Multiprocessing 支持子进程，通信，共享数据，执行不同形式的同步，提供了Process，pipe，lock等组件

# 多进程

```
import multiprocessing

# multiprocessing.active_children() 列出存在的子进程
# cpu_count() 统计cpu的个数

p = multiprocessing.cpu_count()
m = multiprocessiong.active_children()

print(p)
print(m)

```

process的常用方法

is_alive() 判断进程是否存货

run() 启动进程

start() 启动进程，会自动调用run方法，这个常用

join(timeout) 等待进程结束或者直到超时

process 的常用属性

name 进程名字

pid 进程的pid


```
import multiprocessing
import time

def worker(interval):
    time.sleep(interval)
    print("hello world")

if __name__ == "__main__"
    p = multiprocessing.Process(target=worker, args(5, ))
    #启动进程
    p.start()
    print(p.is_alive())
    # 等待子进程执行完毕或者超时退出
    p.join(timeout=3)
    print("end main")
    
    print(p.name)
    print(p.pid)
    
```

# 多进程实例

```
import multiprocessing
import time

def worker(name, interval):
    print("worke {0} start".fomat(name))
    time.sleep(interval)
    print("worke {0} end".format(name))

if __name__ == "__main__":
    print("main start")
    print("this computer has {0}".format(multiprocessing.cpu_count()))
    
    p1 = multiprocessing.Process(target=worker, args("worker1", 2))
    p2 = multiprocessing.Process(target=worker, args("worker2", 3))
    p3 = multiprocessing.Process(target=worker, args("worker3", 4))
    
    p1.start()
    p2.start()
    p3.start()
    
    for p in multiprocessing.active_children():
        print("the pid of {0} is {1}".format(p.name, p.pid))
    
    print("main end")

```

# 多进程锁

Lock组件

当我们用多进程来读写文件的时候，如果一个进程是写文件，一个进程是读文件，如果两个文件同时进行，肯定是不行的，必须是文件写结束后，才可以进行读操作。或者是多个进程在共享一些资源的时候，同时只能有一个进程进行访问，那就要有一个锁机制进行控制


```
import multiprocessing
import time

lock = multiprocessing.Lock()
# lock.acquire 获得锁
# lock.release 释放锁
# with lock
# 不加锁程序

def add(nummer, value, lock):
    with lock:
        print("init add{0} number = {1}".format(number))
        for i in xrange(1, 6):
            number += value
            print("add{0} number = {1}".format(number))

if __name__ == "__main__":
    lock = multiprocessing.Lock()
    number = 0
    # 创建一个多线程
    p1 = multiprocessing.Process(target=add, args(number, 1, lock))
    p2 = multiprocessing.Process(target=add, args(number, 3, lock))
    p1.start()
    p2.start()
    print("main end")
    
```

# 多进程共享内存

共享内存

python的multiprocessing模块也给我们提供了共享内存的操作

一般的变量在进程之间是没法进行通讯的，multiprocessing给我们提供了Value和Array模块，他们可以在不通的进程中共同使用

# 多进程Manager

python中提供了强大的Manage专门用来做数据共享，其支持的类型非常多，包括，Value，Array，list, dict queue,lock等


```
import multiprocessing

def worker(d, l):
    l += range(11, 16)
    for i in xrange(1, 6):
        key = "key{0}".format(i)
        val = "val{0}".format(i)
        d[key] = val

if __name__ == "__main__":
    manager = multiprocessing.Manager()
    d = manager.dict()
    l = manager.list()
    p = multiprocessing.Process(target=worker, args(d, 1)
    p.start()
    # 阻塞
    p.join()
    print(d)
    print(i)
    print("main end")
```

# 进程池

pool 可以提供指定数量的进程，供用户调用，当有新的请求提交到pool中时，如果池还没有满，那么就会创建一个新的进程用来执行请求；但如果池中的进程数已经达到规定最大值，那么该请求就会等待，直到池中有进程结束，才会创建新的进程。

```

import time

def worker(msg):
    print("start {0}".format(msg))
    time.sleep(1)
    print("end {0}".format(msg))

if __name__=="__main__":
    print("main start")
    pool = multiprocessing.Pool(processes=3)
    for i in xrange(1, 10):
        msg = "hello {0}".format(i)
        pool.apply_async(func=worker, args=(msg,))
    pool.close()
    pool.join() #在join之前一定要调用close，否则报错
    print("main end")

```

