---
title: class
date: 2017-11-03 23:55:28
tags: python	
copyright: true
---

# 类一般形式

创建类我们一般使用class关键字,class后面跟类名字

使用debug模式运行下面的例子

<!--more-->

```
# object是一个超级类，所有类都默认继承它，即在它基础上改写
# This class is abort ren class
class ren(object):
    
    name = "alice"
    sex = "F"
    
    def hello(self):
        print('hello world.')
    

a = ren()
print(type(a))
print(a.name)
print(a.sex)
a.hello()
a.name = 'tom'
print(a.name)

```


# 类的构造器

\_\_init\_\_构造函数，在生成对象时调用。由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去，通过定义一个特殊的__init__方法，在创建实例的时候，就把name,sex等属性绑上去

```
class ren():
    def __init__(self, name, sex):
        self.name = name
        self.sex = sex
    
    def hello(self):
        print('hello {0}'.format(self.name))


test = ren('alice'，'M')

test.hello()

# 在传参数的时候，必须是传递两个参数，name和sex，不然报错
# self参数不用传递，python自动会把ren实例化的test传递给第一个参数，即self

```

注意到\_\_init\_\_方法的第一个参数永远是self，表示创建的实例本身，因此，在\_\_init\_\_方法内部，就可以把各种属性绑定到self，因为self就指向创建的实例本身。

有了\_\_init\_\_方法，在创建实例的时候，就不能传入空的参数了，必须传入与\_\_init\_\_方法匹配的参数，但self不需要传入，python解析器自己会把实例变量传进去

# 类的继承

> python的类支持多继承，而java没有多继承，但是可以有多接口的实现。

```
# python的多继承例子：

class A(object):
    pass

class B(object):
    pass
    
# C类继承A和B类
class C(A, B):
    pass

```

```
class parent():
    name = 'parent'
    age = '100'
    
    def __init__(self):
        print('My name is {0}'.format(self.name))
    
    def get_name(self):
        return self.name
    
    def get_age(self):
        return self.age


class child(parent):
    name = 'child'
#   age = '20'

    def __init__(self):
        print('My name is {0}'.format(self.name))
    
    def hello(self):
        print('hello, world')


a = child()
a.hello()
print(a.get_name())
# 先在子类中找，找不到再去父类找
print(a.get_age())

```

# 类的重写

super关键字

```
class parent(object):
    parent_name = 'parent'
    age = '100'
    sex = 'M'
    address = 'abcdefg'
    
    def __init__(self, address, age):
        self.address = address
        self.age = age
    
    def get_name(self):
        print('** parent name **')
    
    def get_age(self):
        return self.age


class child(parent):
    child_name = 'child'
    # age = '20'

    def __init__(self, address, age):
    # 子类在执行时执行父类的\_\_init\_\_
    #   parent.__init__(self, address, age)
    super(child, self).__init__.(self, address, age)
    
    def hello(self):
        print('hello, world')
    
# 继承父类get_name的方法，再增加父类中的方法（新增）,使用super关键字
    
    def get_name(self):
        super(child, self).get_name()
        print('today is nice day')

print(a.get_name())
print(a.address)
print(a.sex)

```

# 类的私有变量

```
class A(object):
    _name = 'alice'
    __sex = 'F'
    
    def hello(self):
        print(self._name)
        print(self.__sex)
    def get_sex(self):
        return self.__sex

a = A()
print(a._name)
a.hello()
print(a.get_sex())
print(dir(a))

print(a.__doc__)

```


在Python中可以通过在属性变量名前加上双下划线定义属性为私有属性

特殊变量命名

\_xx 以单下划线开头的表示的是protected类型的变量。即保护类型只能允许其本身与子类进行访问。若内部变量标示，如： 当使用“from M import”时，不会将以一个下划线开头的对象引入 。

\_\_xx 双下划线的表示的是私有类型的变量。只能允许这个类本身进行访问了，连子类也不可以用于命名一个类属性（类变量），调用时名字被改变（在类FooBar内部，\_\_boo变成\_FooBar\_\_boo,如self.\_FooBar\_\_boo）

\_\_xx\_\_定义的是特列方法。用户控制的命名空间内的变量或是属性，如init , \_\_import\_\_或是file 。只有当文档有说明时使用，不要自己定义这类变量。 （就是说这些是python内部定义的变量名）

在这里强调说一下私有变量,python默认的成员函数和成员变量都是公开的,没有像其他类似语言的public,private等关键字修饰.但是可以在变量前面加上两个下划线"\_",这样的话函数或变量就变成私有的.这是python的私有变量轧压(这个翻译好拗口),英文是(private name mangling.) \*\*情况就是当变量被标记为私有后,在变量的前端插入类名,再类名前添加一个下划线"\_",即形成了\_ClassName\_\_变量名.\*\*

Python内置类属性
\_\_dict\_\_ : 类的属性（包含一个字典，由类的数据属性组成）
\_\_doc\_\_ :类的文档字符串
\_\_module\_\_: 类定义所在的模块（类的全名是'\_\_main\_\_.className'，如果类位于一个导入模块mymod中，那么className.\_\_module\_\_ 等于 mymod）
\_\_bases\_\_ : 类的所有父类构成元素（包含了一个由所有父类组成的元组）


