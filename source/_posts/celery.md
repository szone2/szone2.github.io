---
title: celery
date: 2017-12-19 21:40:44
tags: python
copyright: True
---

# 什么是celery

celery是一个python开发的异步分布式任务调度模块。

<!--more-->

安装celery

pip install celery

安装redis测试用

pip install redis

在服务器上安装redis服务器，并启动

# 例子

创建一个文件abc.python

```
from celery import Celery

broker = "redis://192.168.1.100:6379/5"
backend = "redis://192.168.1.100:6379/6"

app = Celery("abc", broker=broker, backend=backend)

# 注册到任务中
@app.task
def add(x, y):
    return x + y

```

进入abc.python的目录下，输入以下命令启动worker：

celery -A abc worker -l info

新建一个文件

```
from abc import add

a = add.delay(10, 20)
print(a.result)
print(a.status)
print(a.get(timeout=3))
print(a.ready)

```

# 多实例

demon.py

```
from celery import Celery

app = Celery()
# 另外新建一个配置文件名为celeryconfig.py
app.config_from_object("celeryconfig")

@app.task
def taskA(x, y):
    return x * y
    
@app.task
def taskB(x, y ,z):
    return x + y + z

@app.task
def add(x, y):
    return x + y

```

celeryconfig.py文件

```
BROKER_URL = "redis://192.168.48.131:6379/1"
CELERY_RESULT_BACKEND = "redis://192.168.48.131:6379/2"

# 配置消息队列
CELERY_QUEUES = {
    Queue("default", Exchange("default"), routing_key = "default"),
    Queue(for_task_A, Exchange("for_task_A"), routing_key = "for_task_A"),
    Queue("for_task_B", Exchange("for_task_B"), routing_key = "for_task_B")

# 路由
CELERY_ROUTES = {
    "demon3.taskA":{"queue":"for_task_A", "routing_key":"for_task_A"},
    "demon3.taskA":{"queue":"for_task_A", "routing_key":"for_task_B"}
}

```

打开两个session窗口，分别运行以下两个命令

celery -A demon worker -l info -n workerA.%h -Q for\_task\_A

celery -A demon worker -l info -n workerA.%h -Q for\_task\_B

再创建一个文件（模仿客户端）agent.py

```
from demon import *
import time

r1 = taskA.delay(10, 20)
print(r1.result)
print(r1.status)

r2 = taskB.delay(1, 2, 3)
time.sleep(1)
print(r2.result)
print(r2.status)

r3 = add.delay(100, 200)
print(r3.result)
print(r3.status)

```

运行agent.py，我们可以看到add的状态是pending,表示没有执行，这是因为在celeryconfig.py文件中指定route到哪一个Queue中，所以会被发动到默认的名字celery的queue中，但是我们还没有启动worker执行celery中的任务，下面我们启动一个worker来执行celery队列中的任务

celery -A tasks worker -l info -n worker%h -Q celery

print(r3.status) #SUCCESS


# celery与定时任务

在celery中执行定时任务只需要设置celery对象中的CELERYBEAT_SCHEDULE属性即可。


celeryconfig.py文件

```
BROKER_URL = "redis://192.168.48.131:6379/1"
CELERY_RESULT_BACKEND = "redis://192.168.48.131:6379/2"

# 配置消息队列
CELERY_QUEUES = {
    Queue("default", Exchange("default"), routing_key = "default"),
    Queue(for_task_A, Exchange("for_task_A"), routing_key = "for_task_A"),
    Queue("for_task_B", Exchange("for_task_B"), routing_key = "for_task_B")

# 路由
CELERY_ROUTES = {
    "demon3.taskA":{"queue":"for_task_A", "routing_key":"for_task_A"},
    "demon3.taskA":{"queue":"for_task_B", "routing_key":"for_task_B"}
}

CELERY_TIMEZONE = 'UTC'
CELERYBEAT_SCHEDULE = {
    'taskA_schedule':{
        'task':'demon.taskA',
        'schedule':20,
        'args':(5, 6)
    },
    'taskB_schedule':{
        'task':'demon.taskB',
        'schedule':50,
        'args':(100, 200 ,300)
    },
    'add_schedule':{
        'task':'demon.add',
        'schedule':10,
        'args':(1, 2)
    },
}

```
