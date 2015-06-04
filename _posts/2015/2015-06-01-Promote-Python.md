---
layout: post
title: Python进阶
categories:
- 编程语言
- python
tags:
- python
- 读书笔记
---

http://www.imooc.com/video/6045

## 辅助类函数

> 数字变为字符串 str(4)
> 字符串变为数字 string.atoi(s,[，base]) //base为进制基数
> 浮点数转换 string.atof(s) -> float(s)
> 字符转数字 int(str) or long(str)


## 函数式编程

1.map()函数
2.reduce()函数
3.filter()函数

> filter()函数接收一个函数 f 和一个list，这个函数 f 的作用是对每个元素进行判断，返回 True或 False，filter()根据判断结果自动过滤掉不符合条件的元素，返回由符合条件元素组成的新list。

4.sorted()函数

> sorted()也是一个高阶函数，它可以接收一个比较函数来实现自定义排序，比较函数的定义是，传入两个待比较的元素 x, y，如果 x 应该排在 y 的前面，返回 -1，如果 x 应该排在 y 的后面，返回 1。如果 x 和 y 相等，返回 0。

    def cmp_ignore_case(s1, s2):
    s1_l = s1.lower()
    s2_l = s2.lower()
    if s1_l < s2_l:
        return -1
    elif s1_l == s2_l:
        return 0
    else:
        return 1
    print sorted(['bob', 'about', 'Zoo', 'Credit'], cmp_ignore_case)

### 闭包

1.返回函数
2.匿名函数

    # 通过对比可以看出，匿名函数 lambda x: x * x 实际上就是：
    def f(x):
        return x * x
    关键字lambda 表示匿名函数，冒号前面的 x 表示函数参数。
    匿名函数有个限制，就是只能有一个表达式，不写return，返回值就是该表达式的结果。

    print filter(lambda s: s and len(s.strip())>0, ['test', None, '', 'str', '  ', 'END'])

3.带参数decorator

    def log(prefix):
        def log_decorator(f):
            def wrapper(*args, **kw):
                print '[%s] %s()...' % (prefix, f.__name__)
                return f(*args, **kw)
            return wrapper
        return log_decorator
    @log('DEBUG')
    def test():
        pass
    print test()

**完善decorator**
有decorator的情况下，再打印函数名：

    def log(f):
        def wrapper(*args, **kw):
            print 'call...'
            return f(*args, **kw)
        return wrapper
    @log
    def f2(x):
        pass
    print f2.__name__
    输出： wrapper

可见，由于decorator返回的新函数函数名已经不是'f2'，而是@log内部定义的'wrapper'。这对于那些依赖函数名的代码就会失效。decorator还改变了函数的__doc__等其它属性。如果要让调用者看不出一个函数经过了@decorator的“改造”，就需要把原函数的一些属性复制到新函数中：

    def log(f):
        def wrapper(*args, **kw):
            print 'call...'
            return f(*args, **kw)
        wrapper.__name__ = f.__name__
        wrapper.__doc__ = f.__doc__
        return wrapper

这样写decorator很不方便，因为我们也很难把原函数的所有必要属性都一个一个复制到新数上，所以Python内置的***functools***可以用来自动化完成这个“复制”的任务：

    import functools
    def log(f):
        @functools.wraps(f)
        def wrapper(*args, **kw):
            print 'call...'
            return f(*args, **kw)
        return wrapper

5.偏函数

    # http://www.imooc.com/code/6068
    import functools
    sorted_ignore_case = functools.partial(sorted, cmp=lambda s1,s2:cmp(s1.lower(), s2.lower()))
    print sorted_ignore_case(['bob', 'about', 'Zoo', 'Credit'])


## 模块

### 动态导入模块

### 使用__future__

Python的新版本会引入新的功能，但是，实际上这些功能在上一个老版本中就已经存在了。要“试用”某一新的特性，就可以通过导入__future__模块的某些功能来实现。

    from __future__ import unicode_literals
    s = 'am I an unicode?'
    print isinstance(s, unicode)


## 面向对象编程

### 初始化实例属性

    class Person(object):
    def __init__(self, name, gender, birth, **kw):
        self.name = name
        self.gender = gender
        self.birth = birth
        for key, value in kw.items():
            setattr(self, key, value)
    xiaoming = Person('Xiao Ming', 'Male', '1990-1-1', job='Student')
    print xiaoming.name
    print xiaoming.job
    #note:
    #初始化实例属性#（1）__init__可以在类创建实例时被自动调用，所以初始化属性操作可以写在__init__方法里。格式：def __init__(self,name,birth):  self.name = name ; self.birth = birth;
    （2）要定义关键字参数，使用**kw.除了使用self.name='XXX'来设置属性以外，还可以用setattr(self,name,'XXX')

### 访问限制

    __xxx: 私有属性或方法

### 类属性和类方法

    class Person(object):
        address = "Earth"
        __count = 0 # 不能被外部访问
        def __init__(self):
            pass
        @classmethod
        def get_address(cls): # 第一个参数名定义为cls，表示当前类
            return cls.address


## 类的继承

如果已经定义了Person类，需要定义新的Student和Teacher类时，可以直接从Person类继承：

    class Person(object):
        def __init__(self, name, gender):
            self.name = name
            self.gender = gender

定义Student类时，只需要把额外的属性加上，例如score：

    class Student(Person):
        def __init__(self, name, gender, score):
            super(Student, self).__init__(name, gender)
            self.score = score

一定要用 super(Student, self).__init__(name, gender) 去初始化父类，否则，继承自 Person 的 Student 将没有 name 和 gender。函数super(Student, self)将返回当前类继承的父类，即 Person，然后调用__init__()方法，注意self参数已在super()中传入，在__init__()中将隐式传递，不需要写出（也不能写）。

### 多态

Python提供了open()函数来打开一个磁盘文件，并返回 File 对象。File对象有一个read()方法可以读取文件内容：
例如，从文件读取内容并解析为JSON结果：

    import json
    f = open('/path/to/file.json', 'r')
    print json.load(f)

由于Python的动态特性，json.load()并不一定要从一个File对象读取内容。任何对象，只要有read()方法，就称为File-like Object，都可以传给json.load()。
请尝试编写一个File-like Object，把一个字符串 r'["Tim", "Bob", "Alice"]'包装成 File-like Object 并由 json.load() 解析。

### 多重继承

多重继承的目的是从两种继承树中分别选择并继承出子类，以便组合功能使用。
举个例子，Python的网络服务器有TCPServer、UDPServer、UnixStreamServer、UnixDatagramServer，而服务器运行模式有 多进程ForkingMixin 和 多线程ThreadingMixin两种。
要创建多进程模式的 TCPServer：
    class MyTCPServer(TCPServer, ForkingMixin)
        pass
要创建多线程模式的 UDPServer：
    class MyUDPServer(UDPServer, ThreadingMixin):
        pass
如果没有多重继承，要实现上述所有可能的组合需要 4x2=8 个子类。

    +-Person
      +- Student
      +- Teacher
    是一类继承树；
    +- SkillMixin
       +- BasketballMixin
       +- FootballMixin
    是一类继承树。
    通过多重继承，请定义“会打篮球的学生”和“会踢足球的老师”。

    class Person(object):
    pass
    class Student(Person):
        pass
    class Teacher(Person):
        pass
    class SkillMixin(object):
        pass
    class BasketballMixin(SkillMixin):
        def skill(self):
            return 'basketball'
    class FootballMixin(SkillMixin):
        def skill(self):
            return 'football'
    class BStudent(Student, BasketballMixin):
        pass
    class FTeacher(Teacher, FootballMixin):
        pass
    s = BStudent()
    print s.skill()
    t = FTeacher()
    print t.skill()

### 获取对象信息

拿到一个变量，除了用 isinstance() 判断它是否是某种类型的实例外，还有没有别的方法获取到更多的信息呢？
    例如，已有定义：

    class Person(object):
        def __init__(self, name, gender):
            self.name = name
            self.gender = gender
    class Student(Person):
        def __init__(self, name, gender, score):
            super(Student, self).__init__(name, gender)
            self.score = score
        def whoAmI(self):
            return 'I am a Student, my name is %s' % self.name

首先可以用 type() 函数获取变量的类型，它返回一个 Type 对象：

    >>> type(123)
    <type 'int'>
    >>> s = Student('Bob', 'Male', 88)
    >>> type(s)
    <class '__main__.Student'>
    其次，可以用 dir() 函数获取变量的所有属性：
    >>> dir(123)   # 整数也有很多属性...
    ['__abs__', '__add__', '__and__', '__class__', '__cmp__', ...]
    >>> dir(s)
    ['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'gender', 'name', 'score', 'whoAmI']

对于实例变量，dir()返回所有实例属性，包括`__class__`这类有特殊意义的属性。注意到方法`whoAmI`也是 s 的一个属性。
如何去掉`__xxx__`这类的特殊属性，只保留我们自己定义的属性？回顾一下filter()函数的用法。
dir()返回的属性是字符串列表，如果已知一个属性名称，要获取或者设置对象的属性，就需要用 getattr() 和 setattr( )函数了：

    >>> getattr(s, 'name')  # 获取name属性
    'Bob'
    >>> setattr(s, 'name', 'Adam')  # 设置新的name属性
    >>> s.name
    'Adam'
    >>> getattr(s, 'age')  # 获取age属性，但是属性不存在，报错：
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'Student' object has no attribute 'age'
    >>> getattr(s, 'age', 20)  # 获取age属性，如果属性不存在，就返回默认值20：
    20


## 定制类

### 特殊方法

1.__str__(self):
2.__repr__(self):
    
    因为 Python 定义了__str__()和__repr__()两种方法，__str__()用于显示给用户，而__repr__()用于显示给开发人员。
    class Person(object):
        def __init__(self, name, gender):
            self.name = name
            self.gender = gender
    class Student(Person):
        def __init__(self, name, gender, score):
            super(Student, self).__init__(name, gender)
            self.score = score
        def __str__(self):
            return '(student: %s, %s, %s)' %  (self.name, self.gender, self.score)
        __repr__ = __str__
    s = Student('Bob', 'male', 88)
    print s

3.__len__(self):
4.__cmp__(self):
5.__add__:    如果要让Rational进行+运算，需要正确实现__add__

    减法运算：__sub__
    乘法运算：__mul__
    除法运算：__div__

6.__int__:

    要让int()函数正常工作，只需要实现特殊方法__int__():
    同理，要让float()函数正常工作，只需要实现特殊方法__float__()。

### @property

考察 Student 类：

    class Student(object):
        def __init__(self, name, score):
            self.name = name
            self.score = score

当我们想要修改一个 Student 的 scroe 属性时，可以这么写：
s = Student('Bob', 59)
s.score = 60
但是也可以这么写：
s.score = 1000
显然，直接给属性赋值无法检查分数的有效性。
如果利用两个方法：

    class Student(object):
        def __init__(self, name, score):
            self.name = name
            self.__score = score
        def get_score(self):
            return self.__score
        def set_score(self, score):
            if score < 0 or score > 100:
                raise ValueError('invalid score')
            self.__score = score

这样一来，s.set_score(1000) 就会报错。
这种使用 get/set 方法来封装对一个属性的访问在许多面向对象编程的语言中都很常见。
但是写 s.get_score() 和 s.set_score() 没有直接写 s.score 来得直接。
有没有两全其美的方法？----有。
因为Python支持高阶函数，在函数式编程中我们介绍了装饰器函数，可以用装饰器函数把 get/set 方法“装饰”成属性调用：

    class Student(object):
        def __init__(self, name, score):
            self.name = name
            self.__score = score
        @property
        def score(self):
            return self.__score
        @score.setter
        def score(self, score):
            if score < 0 or score > 100:
                raise ValueError('invalid score')
            self.__score = score

注意: 第一个score(self)是get方法，用@property装饰，第二个score(self, score)是set方法，用@score.setter装饰，@score.setter是前一个@property装饰后的副产品。
现在，就可以像使用属性一样设置score了：

>>> s = Student('Bob', 59)
>>> s.score = 60
>>> print s.score
60
>>> s.score = 1000
Traceback (most recent call last):
  ...
ValueError: invalid score
说明对 score 赋值实际调用的是 set方法。


### __slots__

由于Python是动态语言，任何实例在运行期都可以动态地添加属性。
如果要限制添加的属性，例如，Student类只允许添加 name、gender和score 这3个属性，就可以利用Python的一个特殊的__slots__来实现。
顾名思义，__slots__是指一个类允许的属性列表：

    class Student(object):
        __slots__ = ('name', 'gender', 'score')
        def __init__(self, name, gender, score):
            self.name = name
            self.gender = gender
            self.score = score

现在，对实例进行操作：
>>> s = Student('Bob', 'male', 59)
>>> s.name = 'Tim' # OK
>>> s.score = 99 # OK
>>> s.grade = 'A'
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'grade'

__slots__的目的是限制当前类所能拥有的属性，如果不需要添加任意动态的属性，使用__slots__也能节省内存。


### __call__

在Python中，函数其实是一个对象：

    >>> f = abs
    >>> f.__name__
    'abs'
    >>> f(-123)
    123

由于 f 可以被调用，所以，f 被称为可调用对象。
所有的函数都是可调用对象。
一个类实例也可以变成一个可调用对象，只需要实现一个特殊方法__call__()。
我们把 Person 类变成一个可调用对象：

    class Person(object):
        def __init__(self, name, gender):
            self.name = name
            self.gender = gender

        def __call__(self, friend):
            print 'My name is %s...' % self.name
            print 'My friend is %s...' % friend

现在可以对 Person 实例直接调用：

    >>> p = Person('Bob', 'male')
    >>> p('Tim')
    My name is Bob...
    My friend is Tim...

单看 p('Tim') 你无法确定 p 是一个函数还是一个类实例，所以，在Python中，函数也是对象，对象和函数的区别并不显著。


## 下一步学习计划

1. IO：文件和Socket
2. 多线程：进程和线程
3. 数据库
4. Web开发
