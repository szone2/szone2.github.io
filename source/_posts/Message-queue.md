---
title: 消息队列
date: 2018-01-15 23:55:39
tags: python
copyright: True
---

# 消息队列

消息队列的作用：为了防止消息丢失，或者是调用方，不需要一直等待相应方的结果

<!--more-->

```
import codecs
from queue import Queue
from threading import Thread

import time


# 生产者,通过多线程来传送，继承Thread

class Produce(Thread):
    # 构造器，写构造器后第一件事是继承
    def __init__(self, queue):
        super(Produce, self).__init__()
        self.fileName = "../passwd"
        self.filList = list()
        self.queue = queue
    # 重新run方法    
    def run(self):
        with codecs.open("self.fileName") as f:
            self.fileList += f.readlines()
        for line in self.fileList:
            self.queue.put(line)
            
class Consumer(Thread):
    def __init__(self, queue):
        self.queue = queue
        super(Consumer, self).__init__()
        self.newPasswd = "newpasswd.txt"
        self.fileList = list()
    def run(self):
        while 1:
            if self.queue.empty:
                time.sleep(2)
                self.stat += 1
                if self.stat == 5
                    break
            else:
                self.stat = 1
                data = self.queue.get()
                self.fileList.append(data)
        
        with codecs.open(self.newPasswd, 'w') as f:
            f.writelines(self.fileList)
    
```

新建一个py文件，调用上面封装好的类

```
from queue import Queue

from code.threadtest import Produce, Consumer


def main():
    q = Queue()
    produce = Produce(q)
    consumer = Consumer(q)
    produce.start()
    consumer.start()

if __name__ == '__main__':
main()

```

