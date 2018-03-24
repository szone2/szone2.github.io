---
title: 爬虫
date: 2018-01-20 11:17:05
tags: web_crawler
copyright: True
---

# 爬虫

网络爬虫（又被称为网页蜘蛛，网络机器人），是一种安装一定的规则，自动地抓取万维网信息的程序或脚本。另外一些不常使用的名字还有蚂蚁、自动索引，模拟程序或者蠕虫。

通过代码模拟浏览器访问网站的过程，实现自动化获取所需的数据。

<!--more-->

# 爬虫基础知识

在学习爬虫之前，我们先来了解http定义的与服务器交互的几种方法：

两种 HTTP 请求方法：GET 和 POST。在客户机和服务器之间进行请求-响应时，两种最常被用到的方法是：GET 和 POST。

GET - 从指定的资源请求数据。

POST - 向指定的资源提交要被处理的数据

打开谷歌浏览器，按F12，选择Network，勾上Preserve log（作用是让控制台在刷新页面后仍然保留已输出的控制台信息。）

response headers 是网站响应的头信息

request headers 是我们请求的头信息

User-Agent：中文名为用户代理，简称 UA，它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等。

# requests模块

安装requests模块：pip install requests

requests官方教程：http://docs.python-requests.org/zh_CN/latest/user/quickstart.html

http://httpbin.org是requests相关教程地址，通过json方式返回。可以看到我们返回的数据。

requests需要传送的很多参数都是通过字典的形式进行传输。

```
import requests

# requests的各种方法

param = {"key1": "hello", "key2":"world"}
url = "http://www.baidu.com"
r = requests.get(url=url, params=param)
# 请求的url是什么
print(r.url)

# 头部信息
print(r.headers)

# content和text的区别：text是获取页面的所有前端源码（字符串形式），content是二进制形式的源码
print(r.content)
print(r.text)

# post不会在url添加任何内容，直接把数据打包在requests里发送出去
# data是字典类型
r1 = requests.post(url=url, data=param)
print(r1.url)

# cookie（什么等于什么的形式）
print(r.cookie)

# headers 头部信息
print(r.headers)

# 状态码
print(r.status_code)
print(r.request)

# 查看编码
print(r.encoding)

```

# requests 更改请求头信息：

```
import requests

# 有些网站没有user-agent是不能访问的（例如糗事百科，csdn)，所以爬的时候最好加上user-agent

header = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'}

# get的时候通过headers参数传进去即可
r = requests.get('https://www.qiushibaike.com/', headers=header)

print(r.text)
print(r.headers)

# 在网上收集各种浏览器的user-agent信息，把这些信息存储在一个list中，然后通过random模块随机取一个header信息进行访问，防止反爬虫操作。

```

# requests的会话对象

s = requests.session()

python2还可以这样写：s = requests.Session()

在一次会话中，所有的会话信息都保存在s中，只需要对s进行操作即可，例如：

s.get(url)

s.post(url)

# cookie

cookie的常用属性：

Domain 域

Path 路径

Expires 过期时间

name 对应的key值

value key对应的value值

cookie中的domain代表的是cookie所在的域，默认情况下就是请求的域名，例如请求https://baidu.com，那么响应中的set-Cookie默认会用baidu.com作为cookie的domain，在浏览器中也是按照domain来组织cookie的。

我们可以在响应中设置cookie的domain为其他域，但是浏览器并不会去保存这些domain为其他域的cookie

cookie中的path能进一步的控制cookie的访问，当path=/; 当前域的所有请求都可以访问到这个cookie。如果path设为其他值，比如path=/test，那么只有/test下面的请求可以访问到这个cookie。

```

import requests

# cookie是字典的形式

url = "https://www.hao123.com"
s = requests.session()
r = s.get(url)
print(r.cookies)
for cook in r.cookies:
    print(cook.name)
    print(cook.value)
    print(cook.domain)
    print(cook.path)
    print(cook.expires)
    print("#" * 32)
    
```

# 如何带着cookie信息访问网站

cookie信息携带在request headers中

```
import requests

url = 'http://httpbin.org/cookies'
r = requests.get(url, cookies={'key1':'value1', 'key2':'value2'})
print(r.text)

```

结果：

{
  "cookies": {
    "key1": "value1", 
    "key2": "value2"
  }
}

# 小练习

爬www.xicidaili.com中的IP 端口 类型 是否可用

# 代理

```
import requests

# url = "http://2017.ip138.com/ic.asp"
# s = requests.session()
# result = s.get(url=url)
# # 查看所爬网页的编码形式
# print(result.encoding)
# # 修改所爬内容编码形式，防止出现乱码
# result.encoding = "gb2312"
# print(result.text)

# 代理
url = "http://2017.ip138.com/ic.asp"
proxies = {"http":"http://122.225.17.123:8080"}
r1 = requests.get(url=url, proxies=proxies)
print(r1.encoding)
r1.encoding = "gbk"
print(r1.text)


```

# urllib模块

在python2和python3中的差异

在python2中，urllib和urllib2各有各的功能，虽然urllib2是urllib的升级版，但是urllib2还是不能完全带urllib，但是在python3中，全部封装成一个类，urllib

urllib2可以接受一个Requests对象，并以此来设置一个URL的headers，但是urllib只接受一个URL。这就意味着你不能通过urllib伪装自己的请求头。

Urllib模板可以提供运行urlencode的方法，该方法用于GET查询字符串的生成，urllib2不具备这样的功能，而且urllib.quote等一系列quote和unquote功能没有被加入urllib2中。因此有时也需要urllib的辅助。这就是urllib和urllib2一起使用的原因。

quote用来做url转码

```
# python2
# import urllib.urlopen(url)
# import urllib2
# urllib2.Request
# data = urllib.encode(data)
# data = {"key1": "hello", "key2": "world"}

# python3
import urllib.request
url = "https://www.qiushibaike.com/"
headers{""}
ndata = parse.urlencode(data)

urllib.request.Request(url=url, data=ndata)

```

```
import urllib2
import urllib

data = {'key1':'value1', 'key2':'value2'}
ndata = urllib.urlencode(data)
header = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'}

url = "https://www.qiushibaike.com/"
req = urllib2.Requests(url, headers=header)
res = urllib2.urlopen(req)
print(res.read())

```
