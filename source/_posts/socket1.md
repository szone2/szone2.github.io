---
title: socket网络编程(1)
date: 2018-01-15 23:53:15
tags: socket
copyright: True
---

# Socket网络编程

<!--more-->

```
TCP可靠性的实现：
（1）校验码
（2）接收方反馈
（3）信息包附带序号

UDP：
（1）快 不需要花费时间建立和关闭连接
（2）偶尔丢失一两个消息包无所谓，但是TCP会严格检查
（3）UDP的限制是一个信息包不超过64KB的数据

TCP和UDP区别就是UDP不建立连接，只保证数据的完整性，数据传输快，但是不保证数据是否真的被收到，也不保证数据是够只接收一次，也不保证次序。

服务端是用来给一个或者多个客户端提供服务的，当客户端发起请求，开始等待服务端的返回结果，服务端接受完请求以后，根据自己的逻辑进行处理请求，并返回给客户端，客户端接收到返回结果以后，关闭和服务端的连接。

最常用个客户端和服务端有两种模式C/S（mysql）模式和B/S模式（网站）

```

假如客户端有N个socket，则服务端有N + 1 个socket

客户端每发起一个socket连接，服务端就会创建一个socket

# socket简介

```
绑定地址：   bind(address)
# 0.0.0.0表示绑定服务器上任何一个地址
address = (‘0.0.0.0’, 8009)
s.bind(address) 或者s.bind((‘0.0.0.0’, 8009))
address 必须是一个元组，容易错误，address = （host，port）
host：服务端ip，字符串类型， 如果为0.0.0.0  代表本机的任意一个ip
port：服务端提供的端口， 整型， 0-1024为系统保留

监听消息：  
s.listen(badklog)
backlog代表可以同时接受多少个socket连接

接受连接：
conn, addr = s.accept()
接受连接并返回元组(conn, addr), 其中conn是新的套接字对象，每一个新的连接就创建一个新的对象。可以用来接受和发送数据，addr是客户端的地址。包含host和port

发送数据:
s.send(string)	         发送字符串到连接的套接字，可能未将指定内容全部发送
s.sendall(string)      内部递归调用send，将所有内容发送出去，建议使用。

接收数据：
data = s.recv(bufsize)
接收套接字数据，数据以字符串形式返回，bufsize指定最多接收的数据量，可以使用1024， 2048
如果不知道接收的数量有多少，可能几个字节，可能几M，一般通过循环接收

UDP:
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.sendto(string)
data, address = s.recvfrom(bufsize)

客户端：
客户端首先也要创建socket套接字
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
客户端连接服务端函数：
s.connect(address)        		连接到address的套接字
result = connect_ex(address)		成功返回0，失败返回错误码（推荐使用）

通用：
s.close()    			关闭socket套接字
s.getsocketname()                   获取套接字的名字
s.settimeout(timeout)		设置套接字超时时间，timeout为float类型，单位为秒。
s.gettimeout() 			获得套接字超时时间

通用：
s.setblocking(flag)
flage为bool值
setblocking(True) is equivalent to settimeout(None); 相当于不设置超时时间，一直阻塞在那里
setblocking(False) is equivalent to settimeout(0.0). 相当于设置超时时间为0, 如果设置False，那么accept和recv时一旦无数据，则报错。

s.fileno()
返回套接字的文件描述符（一个小整数）。这对于select.select()是有用的。

```

# socket常用函数讲解

```
创建套接字
s = socket.socket(address_family, socket_type)
address family： 
socket.AF_INET       	默认ipv4
socket.AF_INET6	     	ipv6
socket.AF_UNIX   	只能用于单一的unix系统进行间通信
socket type：
socket.SOCK_STREAM	流式socket，TCP
socket.SOCK_DGRAM	数据报是socket   UDP

TCP:
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

```

# 练习

模拟httpclient， 访问百度

写一个简单的类似ssh的程序

每一个accept，接收到套接字链接以后，分别用多线程来处理不同的客户端


```
import socket

import time


class InitSocketTest(object):
    def __init__(self, host, port, type):
        self.host = host
        self.port = port
        self.address = (host, port)
        self.type = type
        self.s = None
        self.creatsocket()

    def creatsocket(self):
        if self.type.upper() == "TCP":
            self.s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        elif self.type.upper() == "UDP":
            self.s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        else:
            print("you must input the InitSocket(type) is 'UDP|TCP' ")

class SocketServerTest(InitSocketTest):
    def __init__(self, host, port, type,  backlog):
        self.backlog = backlog
        super(SocketServerTest, self).__init__(host, port, type)
        self.clientAddress = None

    def run(self):
        self.s.bind(self.address)
        self.s.listen(self.backlog)
        print("server starting…………")
        conn, self.clientAddress = self.s.accept()
        print("accept connect from {0}".format(self.clientAddress))
        for i in range(1, 10):
            conn.sendall("i = {0}".format(str(i)).encode("utf-8"))
        self.s.close()

class ClientSocketTest(InitSocketTest):
    def run(self):
        self.s.connect(self.address)
        stat = 1
        while 1:
            data = self.s.recv(2048)
            if len(data)>0:
                stat = 1
                print(data.decode("utf-8"))
            else:
                stat += 1
                time.sleep(1)
                if stat == 5:
                    break

```

新建一个testserver.py（服务端）调用上面封装好的类

```
from code.util import SocketServerTest

if __name__ == '__main__':
    socketServer = SocketServerTest(host="0.0.0.0", port=9999, type="tcp", backlog=5)
socketServer.run()
    
```

新建一个testclient.py（客户端）


```

from code.util import ClientSocketTest

if __name__ == '__main__':
    socketClient = ClientSocketTest(host="127.0.0.1", port=9999, type="tcp")
socketClient.run()

```

  



