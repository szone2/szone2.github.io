---
title: 线程
date: 2017-12-14 21:39:04
tags: python
copyright: True
---

# 多线程实例

python中提供了threading模块来对多线程的操作

# 多线程实例

线程是应用程序中工作的最小单元

多线程的实现由两种方法：

方法一：将要执行的方法作为参数传给Thread的构造方法（和多进程类似）

t = threading.Thread(taget=action, args=(i,))

方法二：从Thread继承，并重写run()

```
#方法一
def worker(n):
    print("start worker{0}".format(n))

#方法二
class MyThread(threading.Tread):
    def __init__(self):
        super(MyThread, self).__init__()
        self.args = args
    def run(self):
        print("start MyTread{0}".format(self.args))

if __name__ == "__main__":
    for i in xrange(1, 6):
        t1 = threading.Thread(target=worker, args=(i,))
        t1.start()
    t1.join()
    
    for x in xrange(6, 11):
        t2 = MyThread(x)
        t2.start()
    t2.join()
```

# 多线程锁

通过threading.Lock()来创建锁，函数在执行的只有先要获得锁，执行完后要释放锁：

with lock:

lock.acquire()

lock.release()

```
import time

def worker(name):
    with lock:
        print("start {0}".format(name))
        time.sleep(5)
        print("end {0}"format(name))

if __name__ == "__main__":
    lock = threading.Lock()
    t1 = threading.Thread(targe=worker, args("woker1", lock))
    t2 = threading.Thread(targe=worker, args("woker2", lock))

    t1.start()
    t2.start()
    print("main end")

```

# 多线程共享变量

多线程和多进程不同之处在于多线程本身就是可以和父进程共享内存的，这也是为什么其中一个线程挂掉后，为什么其他线程也会死掉的原因


```

def worker(l):
    l.append("a")
    l.append("b")
    l.append("c")
    
if __name__ == "__main__":
    l = list()
    l += range(1, 10)
    print(l)
    t = threading.Thread(targe=worker， args=(1,))
    t.start()
    pirnt(l)

```

# 线程池

通过传入一个参数组来实现多线程，并且它的多线程是有序的，顺序与参数组中的参数顺序保持一致

安装包：

pip install threadpool

调用格式：


```

from threadpool import *
pool = ThreadPool(poolsize)
requests = makeRequests(some_callable, list_of_args, callback)
[pool.putRequest(req)for req in requests]
pool.wait()

```


