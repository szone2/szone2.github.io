---
title: nosql
date: 2017-11-24 22:50:32
tags: nosql
copyright: true
---

# redis服务器搭建

redis官网：https://redis.io/

<!--more-->

```
$ wget http://download.redis.io/releases/redis-4.0.2.tar.gz
$ tar xzf redis-4.0.2.tar.gz
$ cd redis-4.0.2
$ make

修改配置文件支持后台启动redis
vi redis.conf
把daemon no 改为daemon yes

src/redis-server

$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"

```

```
配置主从的方法：

只需在从的配置文件最后一行，添加

salvof 主的IP或域名 端口号

```

```

安装redis客户端
pip install redis

```

# redis简单操作


```
import redis

# r = redis.Redis(host="192.168.1.100", prot=6379)

# 或者
redis_config = {
    "hosts": "192.168.1.100"
    "port": 6379
}

r = redis.Redis(**redis_config)

r.set("alice", "abc")
# 获取所有的keys
print(r.keys())
print(r.get("alice")

```

# 连接池

```
import redis

def get_redis_connect():

    redis_config = {
        "hosts": "192.168.1.100"
        "port": 6379
    }
    
    pool = redis.ConnectionPool(**redis_config)
    r = redis.Redis(connection_pool=pool)
    return r

if __name__ == "__main__":
    r = get_redis_connect()
    r.set("name", "alice")
    print(r.get("name"))

```

# 管道


```
redis-py默认在执行每次请求都会创建（连接池申请连接）和断开（归还连接池）一次连接操作，如果想要在一次请求中指定多个命令，则可以使用pipline实现一次请求指定多个命令，并且默认情况下一次pipline 是原子性操作。减少功耗
redis是一个cs模式的tcp server，使用和http类似的请求响应协议。一个client可以通过一个socket连接发起多个请求命令。每个请求命令发出后client通常会阻塞并等待redis服务处理，redis处理完后请求命令后会将结果通过响应报文返回给client。基本的通信过程如下：

Client: INCR X
Server: 1
Client: INCR X
Server: 2
Client: INCR X
Server: 3
Client: INCR X
Server: 4
基本上四个命令需要8个tcp报文才能完成。由于通信会有网络延迟,假如从client和server之间的包传输时间需要0.125秒。那么上面的四个命令8个报文至少会需要1秒才能完成。这样即使redis每秒能处理100个命令，而我们的client也只能一秒钟发出四个命令。这显示没有充分利用 redis的处理能力。除了可以利用mget,mset 之类的单条命令处理多个key的命令外我们还可以利用pipeline的方式从client打包多条命令一起发出，不需要等待单条命令的响应返回，而redis服务端会处理完多条命令后会将多条命令的处理结果打包到一起返回给客户端。通信过程如下：
Client: INCR X
Client: INCR X
Client: INCR X
Client: INCR X
Server: 1
Server: 2
Server: 3
Server: 4
假设不会因为tcp报文过长而被拆分。可能两个tcp报文就能完成四条命令，client可以将四个命令放到一个tcp报文一起发送，server则可以将四条命令的处理结果放到一个tcp报文返回。通过pipeline方式当有大批量的操作时候。我们可以节省很多原来浪费在网络延迟的时间。需要注意到是用 pipeline方式打包命令发送，redis必须在处理完所有命令前先缓存起所有命令的处理结果。打包的命令越多，缓存消耗内存也越多。所以并是不是打包的命令越多越好。具体多少合适需要根据具体情况测试

```


```
# 使用管道和不使用管道之间的差别实验

import datetime
import redis


def withpipe(r):
    # 管道对象pipe
    pipe = r.pipeline(transaction=True)
    # 利用for循环存入1000个k-v数据
    for i in xrange(1, 1000):
        key = "test1" +str(i)
        value = "test1" + str(i)
        pipe.set(key, value)
    # 执行
    pipe.execute()


def withoutpipe(r):
    # pipe = r.pipeline(transaction=True)
    for i in xrange(1, 1000):
        key = "test1" + str(i)
        value = "test1" + str(i)
        r.set(key, value)

if __name__ == "__main__":
    pool = redis.ConnectionPool(host="192.168.1.100", port=6379, db=0)
    r1 = redis.Redis(connection_pool=pool)
    r2 = redis.Redis(connection_pool=pool)
    start = datetime.datetime.now()
    # print(start)
    withpipe(r1)
    end = datetime.datetime.now()
    # print((end-start).microseconds)
    # print(end-start)
    t_time = (end - start).microseconds
    print("withpipe time is : {0}".format(t_time))

    start = datetime.datetime.now()
    withoutpipe(r2)
    end = datetime.datetime.now()
    t_time = (end - start).microseconds
    print("withoutpipe time is : {0}".format(t_time))

结果：
withpipe time is : 28000
withoutpipe time is : 253000


```

# 字符串操作

redis中的string在内存中都是按照一个Key对应一个value来存储的，如：

r.set('name', 'alice')

set 的使用方法：

set(name, value, ex=None, px=None, nx=False, xx=False)

ex，过期时间（秒）

px，过期时间（毫秒）

nx，如果设置为True，则只有name不存在是，当前set操作才执行，同setnx(name, value)

xx，如果设置为True，则只有name存在时，当前set操作才执行

get(name) 获取值

print(r.get("name")

mset() 批量设置值


```
import redis

pool = redis.ConnectionPool(host="192.168.1.100", port=6379, db=0)
r = redis.Redis(connection_pool=pool)

r.set("name", "alice")
print(r.key())
print(r.get("name")

r.mset(name1 = "foo", name2 = "tom")
print(r.mget("name1", "name2")

r.mset({"hello": "world", "nihao": "nice"})
print(r.keys())
print(r.mget("hello", "nihao")

```

# list

```
redis中的List在在内存中按照一个name对应一个List来存储
lpush(name,values)
#在name对应的list中添加元素，每个新的元素都添加到列表的最左边
r.lpush("list_name",2)
r.lpush("list_name",3,4,5)#保存在列表中的顺序为5，4，3，2
rpush(name,values)
#同lpush，但每个新的元素都添加到列表的最右边
lpushx(name,value)
#在name对应的list中添加元素，只有name已经存在时，值添加到列表的最左边
rpushx(name,value)
#在name对应的list中添加元素，只有name已经存在时，值添加到列表的最右边
llen(name)
#name对应的list元素的个数
print(r.llen("list_name"))、
linsert(name,where,refvalue,value)：在name对应的列表的某一个值前后插入一个新值
#参数：
    # name，redis的name
    # where，BEFORE或AFTER
    # refvalue，标杆值，即：在它前后插入数据
#value，要插入的数据
r.lset(name,index,value)：对name对应的list中的某一个索引位置重新赋值。
#参数：
    # name，redis的name
    # index，list的索引位置
#value，要设置的值
r.lrem(name,value,num):在name对应的list中删除指定的值
#参数：
    # name，redis的name
    # value，要删除的值
    # num，  num=0，删除列表中所有的指定值；
           # num=2,从前到后，删除2个；
           # num=-2,从后向前，删除2个
lpop(name)：在name对应的列表的左侧获取第一个元素并在列表中移除，返回值删除那个元素的值
#扩展： rpop(name) 表示从右向左操作
lindex(name,index)：在name对应的列表中根据索引取列表元素
lrange(name,start,end)：在name对应的列表分片获取数据
```

```
import redis

pool = redis.ConnectionPool(host="192.168.48.131", port=6379, db=0)
r = redis.Redis(connection_pool=pool)
# lpush  在list的左边增加一个元素           left
# rpush  在list的右边增加一个元素           right
r.lpush("list1", "test1")
r.rpush("list1", "li")
r.lpush("list1", "test2")
r.lpush("list1", "test3")
r.lpush("list1", "test2")
r.lpush("list1", 2, 3, 4)
r.lpush("list1", "test2")
print(r.lrange("list1", 0, -1))

# 最终的list结果是    [4, 3, 2, "test3", "test2", "test1", "li"]

```

# set操作

> set集合就是不重复的列表

#给name对应的集合中添加元素

sadd(name,values)

r.sadd("set_name","aa")

r.sadd("set_name","aa","bb")

#获取name对应的集合的所有成员

smembers(name)

#获取name对应的集合中的元素个数

scard(name)

r.scard("set_name")

#获取多个name对应集合的并集

sinter(keys, *args)

r.sadd("set_name","aa","bb")

r.sadd("set_name1","bb","cc")

r.sadd("set_name2","bb","cc","dd")

print(r.sinter("set\_name","set\_name1","set_name2"))

#输出:｛bb｝

#检查value是否是name对应的集合内的元素

sismember(name, value)

#从集合的右侧移除一个元素，并将其返回

spop(name)

#获取多个name对应的集合的并集

sunion(keys, *args)

r.sunion("set\_name","set\_name1","set_name2")

srem(name, value)  删除集合中的某个元素

r.srem("set_name", "aa")

# hash类型操作

```
import redis

pool = redis.ConnectionPool(host="192.168.1.100", port=6379, db=0)
r = redis.Redis(connection_pool=pool)

# hash类型的操作就是一个name对应一个字典
# hset(name, key, value)
r.hset("dict1", "hello", "world")
print(r.get("dict1", "hello"))

r.hmset("dict1", {"k1": "v1", "k2": "v2", "k3": "v3"})
print(r.hmget("dict1", "k1", "k2", "hello"))
print(r.hgetall("dict1"))

print(r.hlen("dict1"))
# 打印所有的key值
print(r.hkeys("dict1")
# 打印所有的value
print(r.hvals("dict1")
# 检查name对应的hash是否存在当前传入的key
print(r.hexists("dict1", "hello"))
# 将name对应的hash中指定key的键值对删除
r.hdel("dict1", "hello")
print(r.hgetall("dict1"))

```

# 其他常用操作

```
import redis

pool = redis.ConnectionPool(host="192.168.1.100", port=6379, db=0)
r = redis.Redis(connection_pool=pool)

# 删除一个值
r.delete("name1")

# 打印所有key值
print(r.keys())

# 查看name1的类型
print(r.type("name")

#重命名
r.rename("name1", "name2")

print(r.exists("name")

# 把dict1移动到1库
r.move("dice1", 1)

```

# memecache安装

memcached是一个高性能的分布式内存对象缓存系统

## 安装memcache

cd /usr/local/src

wget http://memcached.org/latest

tar -zxvf memcached-1.x.x.tar.gz

cd memcached-1.x.x

./configure && make && make test && sudo make install

启动参数：

./memcached -d -m 256 -u root -l 127.0.0.1 -p 11211 -c 256 -P /usr/local/memcached/logs/memcache.pid

# memcache集群的操作

memcached天生支持集群：

python-memcached模块原生支持集群操作，其原理是在内存维护一个主机列表，且集群中主机的权重值和主机在列表中重复出现的次数成正比。

主机     权重

1.1.1.1  1

1.1.1.2  2

1.1.1.3  1

那么在内存中主机列表为：

host_list={'1.1.1.1', '1.1.1.2', '1.1.1.2', '1.1.1.3'}


```
# 安装memcache的客户端
# pip install python-memcached

import memcahce

# memcache.Client初始化一个memcache的客户端对象
# 如果用户要在内存中创建一个键值对（如，k1 = "v1"),那么要执行以下步骤：
# 根据算法将k1换成一个数字：
# 将数字和主机列表长度求余数，得到一个值N（0<=N<列表长度）；
# 在主机列表中根据第二步得到的值为索引获取主机，例如：host_list[N];
# 连接将第三步中获取的主机，将k1='v1'放置在该服务器的内存中。
# debug=True表示在运行中出现错误是，显示错误信息，上线后移除该参数。

mc = memcache.Client([("192.168.1.100:11211" ), ("192.168.1.100:11212", 2), ("192.168.1.100:11213", 1)], debug=True)
mc.set("k1", "v1")
print(mc.get("k1"))

mc1 = memcache.Client(["192.168.1.100:11213"], debug=True)
print(mc1.get("k1"))


```

# 13.12 memcache常用方法

存储命令：set/add/replace/append/prepend/cas

获取命令：get/gets

其他命令：delete/stats

add方法：添加一条键值对，如果已经存在的key，重复执行add操作会报异常。

```
import memcache

mc = memcache.Client(["192.168.1.100:11211"], debug=True)

# add

mc.add("k1", "v1")
print(mc.get("k1"))

# replace

mc.replace("k1", "hello world")
print(mc.get("k1"))

# set

mc.set("k2", "v2")
print(mc.get("k2"))

# set 和 add的区别
# 如果这个key值存在，add就会报错，set不会报错，会进行重新赋值并覆盖

# set_multi 一次设置多个key:value
# get_multi 一次获取多个keys，每个key要以list的形式作为参数传入，返回类型为dict

mc.set_multi({"k100": "v100", "k101": "v101", "k102": "v102"})
print(mc.get_multi(["k100", "k101", "k102"]))

# append 在后面追加 和 prepend在前面进行追加
mc.set_multi({"test1":"v1", "test2":"v2", "test3":"v3", "test4":"v4"})
mc.append("test1", "ling")
print(mc.get("test1"))
mc.prepend("test2", "hello")
print(mc.get("test2"))

# iner 默认自增1 第二个参数指定增加数值为n
# decr 默认自增1 第二个参数指定减少的数值为n

mc.set("shop", 1000)
mc.incr("shop")
print(mc.get("shop"))
mc.incr("shop", 100)
print(mc.get("shop"))


```
