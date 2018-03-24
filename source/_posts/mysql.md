---
title: mysql
date: 2017-11-21 00:59:16
tags: python
copyright: true
---

# 使用Python操作Mysql

## Windows下安装MySQL-python 1.2.5

<!--more-->

下载mysql-python:

源码包:https://pypi.python.org/pypi/MySQL-python/1.2.5 

需要安装mysql connector 根据python的版本下载32位或64位版本. 默认安装即可.下载地址：http://dev.mysql.com/downloads/connector/c/6.0.html#downloads 

> 注意：site.cfg 中connector 的值是否是你安装mysql connector的目录

 
还需要安装一个VCforpython2.7 去微软官网下载即可:https://www.microsoft.com/en-us/download/confirmation.aspx?id=44266

python setup.py install

安装检验, import MySQLdb 无返回错误信息表示安装成功

Python 2.7.13 (v2.7.13:a06454b1afa1, Dec 17 2016, 20:53:40) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>import MySQLdb


## 安装mysql数据库

略


```
create database python;
use python;
grant all privileges on python to 'python'@'%' identified by '123456';
flush privileges;
```

# 链接数据库

```
# 比较常用的参数包括:
# host:数据库主机名.默认是用本地主机
# user:数据库登陆名.默认是当前用户
# passwd:数据库登陆的秘密.默认为空
# db:要使用的数据库名.没有默认值
# port:MySQL服务使用的TCP端口.默认是3306，数字类型
# charset:数据库编码
# 更多关于参数的信息可以查这里 http://mysql-python.sourceforge.net/MySQLdb.html

conn=MySQLdb.connect(host="192.168.1.100",user="python",passwd="123456",db="python",charset="utf8")


# 有时候，为了我们代码的规范，我更加推荐把所有数据库的配置写在一个字典中，如下所示：
def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf-8'
    }
    try:
        cnx = MySQLdb.connect(**db_config)
    except Exception as e:
        raise e
    return cnx

# 调用时只需写
connect_mysql()

# 这样写的代码更加规范，即使以后数据库有协议，我们只需要改动db_config字典中的内容就可以了，后面的内容就不用更改了，这样可以增加代码的可移植性，你也可以把mysql的连接包装成一个函数，以后在连接mysql的时候，直接调用函数就可以了！

```

# mysql事务

MySQL 事务主要用于处理操作量大，复杂度高的数据。比如，你操作一个数据库，公司的一个员工离职了，你要在数据库中删除他的资料，也要删除该人员相关的，比如邮箱，个人资产等。这些数据库操作语言就构成了一个事务。

在MySQL中只有使用了Innodb数据库引擎的数据库或表才支持事务，所以很多情况下我们都使用innodb引擎。

事务处理可以用来维护数据库的完整性，保证成批的SQL语句要么全部执行，要么全部不执行。

一般来说，事务是必须满足4个条件（ACID）： Atomicity（原子性）、Consistency（稳定性）、Isolation（隔离性）、Durability（可靠性）

1、事务的原子性：一组事务，要么成功；要么撤回。

2、稳定性 ： 有非法数据（外键约束之类），事务撤回。

3、隔离性：事务独立运行。一个事务处理后的结果，影响了其他事务，那么其他事务会撤回。事务的100%隔离，需要牺牲速度。

4、可靠性：软、硬件崩溃后，InnoDB数据表驱动会利用日志文件重构修改。可靠性和高速度不可兼得， innodb\_flush\_log\_at\_trx_commit选项 决定什么时候吧事务保存到日志里。

而mysql在默认的情况下，他是把每个select，insert，update，delete等做为一个事务的，登录mysql服务器，进入mysql，执行以下命令：


```
mysql> show variables like 'auto%';

+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| auto_increment_increment | 1     |
| auto_increment_offset    | 1     |
| autocommit               | ON    |
| automatic_sp_privileges  | ON    |
+--------------------------+-------+
4 rows in set (0.00 sec)
```
如上所示： 有一个参数autocommit就是自动提交的意思，每执行一个msyql的select，insert，update等操作，就会进行自动提交。

如果把改选项关闭，我们就可以每次执行完一次代码就需要进行手动提交，connect对象给我们提供了两种办法来操作提交数据。

d)	mysql事务的方法

commit():提交当前事务，如果是支持事务的数据库执行增删改后没有commit则数据库默认回滚

rollback():取消当前事务


# mysql操作数据

接下来，我们通过python代码增加一条数据到数据库中，代码如下：

```
import MySQLdb


def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf-8'
    }
    try:
        cnx = MySQLdb.connect(**db_config)
    except Exception as e:
        raise e
    return cnx

if __name__ == '__main__':
    cnx = connect_mysql()
    cus = cnx.cursor()
    sql  = ''' create table test(id int not null);insert into test(id) values (100);'''
    try:
        cus.execute(sql)
        cus.close()
        cnx.commit()
    except Exception as e:
        cnx.rollback()
        print('Error')
        # raise e
    finally:
        cnx.close()

```

# 游标

```
# excute(sql[, args]):执行一个数据库查询或命令
解释：
先通过MySQLdb.connect(**db_config)建立mysql连接对象
在通过 = cnx.cursor()创建游标
fetchone():在最终搜索的数据中去一条数据
fetchmany(1)在接下来的数据中在去1行的数据，这个数字可以自定义，定义多少就是在结果集中取多少条数据。
fetchall()是在所有的结果中搞出来所有的数据。
对于excute()和excutemany()的方法，我们会在以一节详细分析他们的区别。

import MySQLdb


def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    try:
        cnx = MySQLdb.connect(**db_config)
    except Exception as e:
        raise e
    return cnx
    
if __name__=="__main__":
    sql = "select * from tmp."
    cnx = connect_mysql()
    # 创建游标游标对象cursor()
    cus = cnx.cursor()
    
    try:
    # 执行一个数据库查询或命令
        cus.execute(sql)
        
        # 得到结果集的下一行
        result1 = cus.fetchone()
        print(result1)
        
        # 得到结果集的下几行
        result2 = cus.fetchmany(3)
        print(result2)
        
        # 得到结果集中剩下的所有行
        result3 = cus.fetchall()
        print(result3)
        
        cus.close()
    except Exception as e:
        cnx.rollback()
    finally:
        cnx.close()

```

```
# executemany (sql, args):执行多个数据库查询或命令

import MySQLdb

def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    try:
        cnx = MySQLdb.connect(**db_config)
    except Exception as e:
        raise e
    return cnx

if __name__ == "__main__":
    sql = "select * from tmp;"
    sql1 = "insert into tmp(id) value (%s);"
    param = []
    for i in xrange(100, 130):
        param.append([str(i)])
    print(param)
    cnx = connect_mysql()
    cus = cnx.cursor()
    print(dir(cus))
    try:
        cus.execute(sql)
        cus.executemany(sql1, param)
        # help(cus.executemany)
        result1 = cus.fetchone()
        print("result1")
        print(result1)

        result2 = cus.fetchmany(3)
        print("result2")
        print(result2)

        result3 = cus.fetchall()
        print("result3")
        print(result3)
        cus.close()
        cnx.commit()

    except Exception as e:
        cnx.rollback()
        raise e
    finally:
        cnx.close()

```

# 数据库连接池

> python编程中可以使用MySQLdb进行数据库的连接及诸如查询/插入/更新等操作，但是每次连接mysql数据库请求时，都是独立的去请求访问，相当浪费资源，而且访问数量达到一定数量时，对mysql的性能会产生较大的影响。因此，实际使用中，通常会使用数据库的连接池技术，来访问数据库达到资源复用的目的。

python的数据库连接池包 DBUtils：

DBUtils是一套Python数据库连接池包，并允许对非线程安全的数据库接口进行线程安全包装。DBUtils来自Webware for Python。

DBUtils提供两种外部接口：

* PersistentDB ：提供线程专用的数据库连接，并自动管理连接。
* 
* PooledDB ：提供线程间可共享的数据库连接，并自动管理连接。
* 
下载地址：https://pypi.python.org/pypi/DBUtils/  

下载解压后，使用python setup.py install 命令进行安装

或者使用

Pip install DBUtils

例子：

```
import MySQLdb
from DBUtils.PooledDB import PooledDB
db_config = {
        "host": "192.168.1.100",
        "port": 3306,
        "user": "python",
        "passwd": "123456",
        "db": "python",
        "charset": "utf8"
    }


# 5为连接池里的最少连接数
pool = PooledDB(MySQLdb, 5, **db_config)

 # 以后每次需要数据库连接就是用connection（）函数获取连接就好了
conn = pool.connection() 
cur = conn.cursor()
SQL = "select * from tmp;"
r = cur.execute(SQL)
r = cur.fetchall()
print(r)
cur.close()
conn.close()

PooledDB的参数：
1. mincached，最少的空闲连接数，如果空闲连接数小于这个数，pool会创建一个新的连接
2. maxcached，最大的空闲连接数，如果空闲连接数大于这个数，pool会关闭空闲连接
3. maxconnections，最大的连接数，
4. blocking，当连接数达到最大的连接数时，在请求连接的时候，如果这个值是True，请求连接的程序会一直等待，直到当前连接数小于最大连接数，如果这个值是False，会报错，
5. maxshared 当连接数达到这个数，新请求的连接会分享已经分配出去的连接

在uwsgi中，每个http请求都会分发给一个进程，连接池中配置的连接数都是一个进程为单位的（即上面的最大连接数，都是在一个进程中的连接数），而如果业务中，一个http请求中需要的sql连接数不是很多的话（其实大多数都只需要创建一个连接），配置的连接数配置都不需要太大。
连接池对性能的提升表现在：
1.在程序创建连接的时候，可以从一个空闲的连接中获取，不需要重新初始化连接，提升获取连接的速度
2.关闭连接的时候，把连接放回连接池，而不是真正的关闭，所以可以减少频繁地打开和关闭连接

```

# 设计数据库表结构

Student

字段名 | 类型 | 是否为空 | 主键 | 描述
---|---|---|---|---|
STDID | int | 否 | 是 | 学生ID
StdName | varchar(100) | 否 | | 学生姓名
Gender | enum('M', 'F') | 是 | | 性别 
Age | tinyint | 是 | | 年龄

Course

字段名 | 类型 | 是否为空 | 主键 | 描述
---|---|---|---|---|
CouID | int | 否 | 是 | 课程ID
Cname | varchar(50) | 否 | | 课程名称
TID | int | 否 | | 老师ID

Score

字段名 | 类型 | 是否为空 | 主键 | 描述
---|---|---|---|---|
SID | int | 否 | 是 | 分数ID
StdID | int | 否 | | 学生ID
CouID | int | 否 | | 课程ID 
Grade| int | 否 | | 分数

Teacher

字段名 | 类型 | 是否为空 | 主键 | 描述
---|---|---|---|---|
TID | int | 否 | 是 | 老师ID
Tname| varchar(100) | 否 | | 老师名字


# 创建表

```
import MySQLdb

def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    try:
        cnx = MySQLdb.connect(**db_config)
    except Exception as e:
        raise e
    return cnx
    
if __name__ == '__main__':
    cnx = connect_mysql()
    cus = cnx.cursor()
    # sql  = '''insert into student(id, name, age, gender, score) values ('1001', 'tom', 29, 'M', 88), ('1002', 'alice', 29, 'M', 90), ('1003', 'colgate', 33, 'M', 87);'''
    student = '''create table Student(
            StdID int not null,
            StdName varchar(100) not null,
            Gender enum('M', 'F'),
            Age tinyint
    )'''
    course = '''create table Course(
            CouID int not null,
            CName varchar(50) not null,
            TID int not null
    )'''
    score = '''create table Score(
                SID int not null,
                StdID int not null,
                CID int not null,
                Grade int not null
        )'''
    teacher = '''create table Teacher(
                    TID int not null,
                    TName varchar(100) not null
            )'''
	 tmp = '''set @i := 0;
            create table tmp as select (@i := @i + 1) as id from information_schema.tables limit 10;
        '''
    try:
        cus.execute(student)
        cus.execute(course)
        cus.execute(score)
        cus.execute(thearch)
  		cus.execute(tmp)
        cus.close()
        cnx.commit()
    except Exception as e:
        cnx.rollback()
        print('error')
        raise e
    finally:
        cnx.close()
        
结果：

mysql> show tables;
+------------------+
| Tables_in_python |
+------------------+
| Course           |
| Score            |
| Student          |
| Teacher          |
| tmp              |
+------------------+
3	rows in set (0.00 sec)


```

没有任何异常，在数据库中查看表，出现这五个表。说明这五个表已经创建成功。
首先我们先来了解一下information_schema这个库，这个在mysql安装时就有了，提供了访问数据库元数据的方式。那什么是元数据库呢？元数据是关于数据的数据，如数据库名或表名，列的数据类型，或访问权限等。有些时候用于表述该信息的其他术语包括“数据词典”和“系统目录”。
information_schema数据库表说明:

SCHEMATA表：提供了当前mysql实例中所有数据库的信息。是show databases的结果取之此表。

TABLES表：提供了关于数据库中的表的信息（包括视图）。详细表述了某个表属于哪个schema，表类型，表引擎，创建时间等信息。是show tables from schemaname的结果取之此表。

COLUMNS表：提供了表中的列信息。详细表述了某张表的所有列以及每个列的信息。是show columns from schemaname.tablename的结果取之此表。

STATISTICS表：提供了关于表索引的信息。是show index from schemaname.tablename的结果取之此表。

USER_PRIVILEGES（用户权限）表：给出了关于全程权限的信息。该信息源自mysql.user授权表。是非标准表。

SCHEMA_PRIVILEGES（方案权限）表：给出了关于方案（数据库）权限的信息。该信息来自mysql.db授权表。是非标准表。

TABLE\_PRIVILEGES（表权限）表：给出了关于表权限的信息。该信息源自mysql.tables_priv授权表。是非标准表。

COLUMN\_PRIVILEGES（列权限）表：给出了关于列权限的信息。该信息源自mysql.columns_priv授权表。是非标准表。

CHARACTER_SETS（字符集）表：提供了mysql实例可用字符集的信息。是SHOW CHARACTER SET结果集取之此表。
COLLATIONS表：提供了关于各字符集的对照信息。

COLLATION\_CHARACTER\_SET_APPLICABILITY表：指明了可用于校对的字符集。这些列等效于SHOW COLLATION的前两个显示字段。

TABLE_CONSTRAINTS表：描述了存在约束的表。以及表的约束类型。

KEY\_COLUMN_USAGE表：描述了具有约束的键列。

ROUTINES表：提供了关于存储子程序（存储程序和函数）的信息。此时，ROUTINES表不包含自定义函数（UDF）。名为“mysql.proc name”的列指明了对应于INFORMATION_SCHEMA.ROUTINES表的mysql.proc表列。

VIEWS表：给出了关于数据库中的视图的信息。需要有show views权限，否则无法查看视图信息。

TRIGGERS表：提供了关于触发程序的信息。必须有super权限才能查看该表

而TABLES在安装好mysql的时候，一定是有数据的，因为在初始化mysql的时候，就需要创建系统表，该表一定有数据。

set @i := 0;
create table tmp as select (@i := @i + 1) as id from information_schema.tables limit 10;
mysql中变量不用事前申明，在用的时候直接用“@变量名”使用就可以了。set这个是mysql中设置变量的特殊用法，当@i需要在select中使用的时候，必须加:，这样就创建好了一个表tmp，查看tmp的数据：

```
mysql> select * from tmp;
+------+
| id   |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
|    6 |
|    7 |
|    8 |
|    9 |
|   10 |
+------+
10 rows in set (0.00 sec)

```

我们只是从information_schema.tables表中取10条数据，任何表有10条数据也是可以的，然后把变量@i作为id列的值，分10次不断输出，依据最后select的结果，创建表tmp。

# 增加数据


```

import MySQLdb
def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    cnx = MySQLdb.connect(**db_config)
    return cnx

if __name__ == '__main__':
    cnx = connect_mysql()


    students = '''set @i := 10000;
            insert into Student select @i:=@i+1, substr(concat(sha1(rand()), sha1(rand())), 1, 3 + floor(rand() * 75)), case floor(rand()*10) mod 2 when 1 then 'M' else 'F' end, 25-floor(rand() * 5)  from tmp a, tmp b, tmp c, tmp d;
        '''
    course = '''set @i := 10;
            insert into Course select @i:=@i+1, substr(concat(sha1(rand()), sha1(rand())), 1, 5 + floor(rand() * 40)),  1 + floor(rand() * 100) from tmp a;
        '''
    score = '''set @i := 10000;
            insert into Score select @i := @i +1, floor(10001 + rand()*10000), floor(11 + rand()*10), floor(1+rand()*100) from tmp a, tmp b, tmp c, tmp d;
        '''
    theacher = '''set @i := 100;
            insert into Teacher select @i:=@i+1, substr(concat(sha1(rand()), sha1(rand())), 1, 5 + floor(rand() * 80)) from tmp a, tmp b;
        '''
    try:
        cus_students = cnx.cursor()
        cus_students.execute(students)
        cus_students.close()

        cus_course = cnx.cursor()
        cus_course.execute(course)
        cus_course.close()

        cus_score = cnx.cursor()
        cus_score.execute(score)
        cus_score.close()

        cus_teacher = cnx.cursor()
        cus_teacher.execute(theacher)
        cus_teacher.close()

        cnx.commit()
    except Exception as e:
        cnx.rollback()
        print('error')
        raise e
    finally:
        cnx.close()



结果：
mysql> select count(*) from Student;
+----------+
| count(*) |
+----------+
|    10000 |
+----------+
1 row in set (0.01 sec)

mysql> select count(*) from Course; 
+----------+
| count(*) |
+----------+
|       10 |
+----------+
1 row in set (0.00 sec)

mysql> select count(*) from Score;
+----------+
| count(*) |
+----------+
|    10000 |
+----------+
1 row in set (0.00 sec)

mysql> select count(*) from Teacher;
+----------+
| count(*) |
+----------+
|      100 |
+----------+
1 row in set (0.00 sec)

在Student的表中增加了10000条数据，id是从10000开始的。count函数时用来统计个数的。
解释；
我们知道Student有四个字段，StdID，StdName，Gender，Age；我们先来看这个select语句：select @i:=@i+1, substr(concat(sha1(rand()), sha1(rand())), 1, 3+floor(rand() * 75)), case floor(rand()*10) mod 2 when 1 then 'M' else 'F' end, 25-floor(rand() * 5)  from tmp a, tmp b, tmp c, tmp d;
StdID字段：@i就代表的就是，从10000开始，在上一句sql中设置的；
StdName字段：substr(concat(sha1(rand()), sha1(rand())), 1, floor(rand() * 80))就代表的是，
substr是一个字符串函数，从第二个参数1,开始取字符，取到3 + floor(rand() * 75)结束
floor函数代表的是去尾法取整数。
rand()函数代表的是从0到1取一个随机的小数。
rand() * 75就代表的是：0到75任何一个小数，
3+floor(rand() * 75)就代表的是：3到77的任意一个数字
concat()函数是一个对多个字符串拼接函数。
sha1是一个加密函数，sha1(rand())对生成的0到1的一个随机小数进行加密，转换成字符串的形式。
	concat(sha1(rand()), sha1(rand()))就代表的是：两个0-1生成的小数加密然后进行拼接。
substr(concat(sha1(rand()), sha1(rand())), 1, floor(rand() * 80))就代表的是：从一个随机生成的一个字符串的第一位开始取，取到（随机3-77）位结束。
Gender字段：case floor(rand()*10) mod 2 when 1 then 'M' else 'F' end,就代表的是，
	floor(rand()*10)代表0-9随机取一个数
	floor(rand()*10) mod 2 就是对0-9取得的随机数除以2的余数，
case floor(rand()*10) mod 2 when 1 then 'M' else 'F' end,代表：当余数为1是，就取M，其他的为F
Age字段：25-floor(rand() * 5)代表的就是，25减去一个0-4的一个整数
现在有一个问题，为什么会出现10000条数据呢，这10000条数据时怎么生成的呢，虽然字段一一对应上了，但是怎么出来这么多数据呢？
先来看个例子：
select * from tmp a, tmp b, tmp c;
最终是1000条数据，a， b， c都是tmp表的别名，相当于每个表都循环了一遍。所以最终的数据是有多少个表，就是10的多少次幂。

```

# 查询数据


```
10000条数据，可能会有同学的名字是一样的，需求就是在数据库中查出来所有名字有重复的同学的所有信息，然后写入到文件中。

import codecs

import MySQLdb
def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    cnx = MySQLdb.connect(**db_config)
    return cnx

if __name__ == '__main__':
    cnx = connect_mysql()

    sql = '''select * from Student where StdName in (select StdName from Student group by StdName having count(1)>1 ) order by StdName;'''
    try:
        cus = cnx.cursor()
        cus.execute(sql)
        result = cus.fetchall()
        with codecs.open('select.txt', 'w+') as f:
            for line in result:
                f.write(str(line))
                f.write('\n')
        cus.close()
        cnx.commit()
    except Exception as e:
        cnx.rollback()
        print('error')
        raise e
    finally:
        cnx.close()

结果：
本地目录出现一个select.txt文件，内容如下：
(19844L, u'315', u'F', 24)
(17156L, u'315', u'F', 25)
(14349L, u'48f', u'F', 25)
(17007L, u'48f', u'F', 25)
(12629L, u'afd', u'F', 25)
(13329L, u'afd', u'F', 24)
(10857L, u'e31', u'F', 23)
(14476L, u'e31', u'M', 21)
(16465L, u'ee5', u'M', 22)
(18570L, u'ee5', u'M', 21)
(17056L, u'ef0', u'M', 23)
(16946L, u'ef0', u'F', 24)
解释：
1.	先来分析一下select查询这个语句：
select * from Student where StdName in (select StdName from Student group by StdName having count(1)>1 ) order by StdName;'
2.	看括号里面的语句：select StdName from Student group by StdName having count(1)>1；这个是把所有学生名字重复的学生都列出来，
3.	最外面select是套了一个子查询，学生名字是在我们（）里面的查出来的学生名字，把这些学生的所有信息都列出来。
4.	result = cus.fetchall()列出结果以后，我们通过fetchall()函数把所有的内容都取出来，这个result是一个tuple
5.	通过文件写入的方式，把取出来的result写入到select.txt文件中。得到最终的结果。

```

# 删除数据

情景：有些老师不好好上课，导致课程的及格率太低，最差5名老师将会被开除


```
import MySQLdb
def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    cnx = MySQLdb.connect(**db_config)
    return cnx

if __name__ == '__main__':
    cnx = connect_mysql()

    sql = '''delete from Teacher where TID in(
    select TID from (select Course.CouID, Course.TID, Teacher.TName, count(Teacher.TID) as count_teacher from Course
    left join Score on Score.Grade < 60 and Course.CouID = Score.CouID
    left join Teacher on Course.TID = Teacher.TID
    group by Course.TID
    order by count_teacher desc
    limit 5)  as test )
    '''
    try:
        cus = cnx.cursor()
        cus.execute(sql)
        result = cus.fetchall()
        cus.close()
        cnx.commit()
    except Exception as e:
        cnx.rollback()
        print('error')
        raise e
    finally:
        cnx.close()

结果：
程序正常执行，没有报错
解释：
1.	先查询出Course表中的Course.TID和Course.TID
2.	left join 是关联Score表，查出Score.Grade > 59，并且，课程ID和课程表的CouID要对应上
3.	left join Teacher 是关联老师表，课程中的了老师ID和老师表中的老师ID对应上
4.	select中加上老师的名字Teacher.Tname和count(Teacher.TID)
5.	group by Course.TID，在根据老师的的TID进行分组
6.	oder by 最后对count_teacher进行排序，取前5行，
7.	在通过套用一个select子查询，把所有的TID搂出来
8.	然后delete from Teacher 最后删除TID在上表中的子查询中。

```

# 更改数据

```
# 把分数低于5分的成绩所有都加60分

import MySQLdb
def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    cnx = MySQLdb.connect(**db_config)
    return cnx

if __name__ == '__main__':
    cnx = connect_mysql()

    sql = '''select *, (grade+60) as newGrade from Score where Grade <5;'''
    update = '''update Score set grade = grade + 60 where grade < 5;  '''
    try:
        cus_start = cnx.cursor()
        cus_start.execute(sql)
        result1 = cus_start.fetchall()
        print(len(result1))
        cus_start.close()

        cus_update = cnx.cursor()
        cus_update.execute(update)
        cus_update.close()

        cus_end = cnx.cursor()
        cus_end.execute(sql)
        result2 = cus_end.fetchall()
        print(len(result2))
        cus_end.close()

        cnx.commit()
    except Exception as e:
        cnx.rollback()
        print('error')
        raise e
    finally:
        cnx.close()

```

# 索引


```

#创建Course的CouID的字段为主键   Score的SID字段为主键 Student的StdID字段为主键  Teacher的TID字段为主键

import MySQLdb
def connect_mysql():
    db_config = {
        'host': '192.168.1.100',
        'port': 3306,
        'user': 'python',
        'passwd': '123456',
        'db': 'python',
        'charset': 'utf8'
    }
    cnx = MySQLdb.connect(**db_config)
    return cnx


if __name__ == '__main__':
    cnx = connect_mysql()

    sql1 = '''alter table Teacher add primary key(TID);'''
    sql2 = '''alter table Student add primary key(StdID);'''
    sql3 = '''alter table Score add primary key(SID);'''
    sql4 = '''alter table Course add primary key(CouID);'''
    sql5 = '''alter table Score add index idx_StdID_CouID(StdID, CouID);'''
    # sql6 = '''alter table Score drop  index idx_StdID_CouID;'''   删除索引
    sql7 = '''explain select * from Score where StdID = 16213;'''
    try:
        cus = cnx.cursor()
        cus.execute(sql1)
        cus.close()

        cus = cnx.cursor()
        cus.execute(sql2)
        cus.close()

        cus = cnx.cursor()
        cus.execute(sql3)
        cus.close()

        cus = cnx.cursor()
        cus.execute(sql4)
        cus.close()

        cus = cnx.cursor()
        cus.execute(sql5)
        cus.close()

        cus = cnx.cursor()
        cus.execute(sql7)
        result = cus.fetchall()
        print(result)
        cus.close()

        cnx.commit()
    except Exception as e:
        cnx.rollback()
        print('error')
        raise e
    finally:
        cnx.close()

结果：
((1L, u'SIMPLE', u'Score', u'ref', u'idx_StdID_CouID', u'idx_StdID_CouID', u'4', u'const', 4L, None),)
解释：
Sql1， sql2， sql3， sql4是添加主键，sql5是增加一个索引，我们也可以在mysql的客户端上执行sq7，得到如下的结果：
mysql> explain select * from Score where StdID = 16213;               
+----+-------------+-------+------+-----------------+-----------------+---------+-------+------+-------+
| id | select_type | table | type | possible_keys   | key             | key_len | ref   | rows | Extra |
+----+-------------+-------+------+-----------------+-----------------+---------+-------+------+-------+
|  1 | SIMPLE      | Score | ref  | idx_StdID_CouID | idx_StdID_CouID | 4       | const |    4 | NULL  |
+----+-------------+-------+------+-----------------+-----------------+---------+-------+------+-------+
1 row in set (0.00 sec)
这个说明，我们在搜索StdID的时候，是走了idx_StdID_CouID索引的。

```

