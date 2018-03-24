---
title: queue
date: 2017-12-15 21:52:41
tags: python
copyright: True
---

# 多进程消息队列

<!--more-->

```

from multiprocessing import Queue

def write(q):
    for i in ["a", "b", "c", "d"]:
        q.put(i)
        print("put {0} to queue".format(i))

def read(q):
    while 1:
        result = q.get()
        print("get {0} from queue".format(result))

def main():
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    pw.start()
    pr.start()
    #让写进程执行完
    pw.join()
    pr.terminate()
    
if __name__ == "__main__":
    main()
    
```

# 消息队列pipe

通过Mutiprocess里面的pipe来实现消息队列：

1.pipe方法返回(conn1,conn2)代表一个管道的两个端，pipe方法有duplex参数，如果duplex参数为True（默认值），那么这个管道是全双工模式，也就是说conn1和conn2均可收发，duplex为False，conn1只负责接收消息，conn2只负责发送消息。

2.send和recv方法分别是发送和接受消息的方法，close方法表示关闭管道，当消息接受结束以后，关闭管道

```

import time
from multiprocessing import Queue

def proc1(pipe):
    for i in xrange(1, 10):
        pipe.send(i)
        print("send {0} to pipe".format(i))

def proc2(pipe):
    n = 9
    while n>0:
        result = pipe.recv()
        print("recv {0} from pipe".format(result))

def main():
    pipe = Pipe(duplex=False)
    print(type(pipe))
    p1 = Process(target=proc1,args=(pipe[1],))
    p2 = Process(targe=proc2, )
    
if __name__ == "__main__":
    main()
    
```

# queue模块

python提供了queue模块来专门实现消息队列

Queue.qsize():返回消息队列的当前空间。返回的值不一定可靠

Queue.empty():判断消息队列是否为空，返回True或False。同样不可靠。

Queue.full()类似上面，判断消息队列是否满

Queue.pull(item, block=True, timeout=None):往消息队列中存放消息。block可以控制是否阻塞，timeout指定阻塞时候的等待时间。如果不阻塞或者超时，会引起一个full expeption

Queue.put_nowait(item):相当于put(item, False)

Queue.get(block=True, timeout=None):获取一个消息，其他同put

Queue.task_done(): 接受消息的线程通过调用这个函数来说明消息对应的任务已完成

Queue.join():实际上意味着等到队列为空，再执行别的操作

```

from threading import Thread

# 写一个消费者和生产者，为了练习多线程，我们用多线程的方式来实现，并通过类的重新的方法来实现

# 类首字母大写,继承Thread类
class Proceduer(Thread):
    def __init__(self):
        super(Proceduer, self).__init__()
        self.queue = queue
    
    def run(self):
        try:
            for i in xrange(1, 10):
                print("put data is : {0} to queue".format(i))
                self.queue.put(i)
        except Exception as e:
            print("put data error!")
    
class Consumer_odd(Thread):
    def __init__(self, queue):
        super(Consumer_odd, self).__init__()
        self.queue = queue
    def run(self):
        try:
            如果值不为空
            while not self.queue.empty()：
                number = self.queue.get(block=True, timeout=3)
                if number%2 != 0:
                    print("get {0} from queue ODD".format(number))
                else:
                    # 不是基数给放回去
                    self.queue.put(number)
                time.sleep(1)
        except Exception as e:
            raise e

class Consumer_evenThread):
    def __init__(self, queue):
        super(Consumer_even, self).__init__()
        self.queue = queue
    def run(self):
        try:
            如果值不为空
            while not self.queue.empty()：
                number = self.queue.get(block=True, timeout=3)
                if number%2 == 0:
                    print("get {0} from queue even, thread name is：{1}".format(number，self.getname()))
                else:
                    # 不是基数给放回去
                    self.queue.put(number)
                time.sleep(1)
        except Exception as e:
            raise e

def main():
    queue = Queue()
    p = Proceduer(queue=queue)
    p.start()
    p.join()
    time.sleep(1)
    c1 = Consumer_odd(queue=queue)
    c2 = Consumer_even(queue=queue)
    c1.start()
    c2.start()
    c1.join()
    c2.join()
    print("All threads terminate ! ")

if __name__ == "__main__":
    main()

```

