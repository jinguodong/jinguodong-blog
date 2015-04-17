---
layout: post
title: Python基础教程
categories:
- 编程语言
- python
tags:
- python
- 读书笔记
---

## 循环（条件、循环和其他辅助语句）

**数据结构**：序列(列表、元组)和字典

**循环方法**：while、for

### 序列:循环分片操作

    numbers[begin, end, step]
    numbers[0:10:1]

基本列表操作：

    list('Hello')
    del numbers[2]
    numbers.append(9) #add element 9 to the end of numbers
    numbers.extend(9) # 通过分片赋值实现
    numbers.index(5) #找到列表中第一个匹配项，若未找到抛出异常
    numbers.insert(3, 'who') #在numbers[3]的位置上添加'who'元素
    numbers.pop(空或number) #默认是最后一个元素，否则是指定位置元素
    numbers.remove(4) # 删除第一个匹配项，不存在抛出异常    
    numbers.reverse()
    numbers.sort(空|自定义比较函数cmp|key=somefunc|reverse=TrueorFalse)
        x = [4,2,1,3]
        y = x[:] # 将x的所有元素的分片赋给y
        y.sort()
    numbers.sorted()

元组：

    tuple([1,2,3])


### 字典

    dict([('name','miky'), ('age',42)])
    d = dict(name='Mike', age=42)

字典基本操作：

    len(d)
    del d[k] # 删除d中键为k的项
    k in d # 判断d中是否含键为k的项

字典的格式化字符串

    phonebook = {"Cecil" : '9120'}
    "Cecil's phone is %(Cecil)s" % phonebook

字典方法

    clear()
    copy() # 浅复制
    deepcopy() # 深复制函数
        from copy import deepcopy()
        dc = deepcopy(d)
    fromkeys()
        dict.fromkeys(['name', 'age'], 空or自定义默认值)
    get(key, 空or自定义默认值)
    has_key()
    items(), iteritems()
    keys(), iterkeys()
    values(), itervalues()
    pop(key) # 返回可以指定的值，并移除key
    popitem()

### 条件执行语句

    if num>0:
        statement
    elif num<0:
        statement
    else:
        statement

### 断言

    assert 0<age<10 #断言必须为真

### while循环 and for循环

    words = ['this', 'is', 'an', 'ex']
    for word in words:
        statement

遍历字典

    d = {'x':1, 'y':2, 'z':3}
    for key in d:
        print key, 'correspondsto', d[key]
    for key, value in d.items():
        statement

列表推算式

    [x*x for x in range(10) if x % 3 == 0]

三人行

    pass
    del
    exec and eval

    scope={}
    scope['x'] = 2
    scope['y'] = 3
    eval("x+y", scope)

    scope={}
    exec "x=1" in scope # 在scope域内执行x=1
    eval("x*x", scope) # 在scope域内运算x*x


## 异常

    raise Exception
    class SomeCustomException(Exception): pass

**捕捉异常**

    try:
        x/y
    except ZeroDivisionError:
        print ...
    except TypeError, e: # 得到异常对象e
        print ...
    except Exception, e: # 全捕捉
        print ...

    try:
        x/y
    except (ZeroDivisionError, TypeError[, Others]):
        print ...
    else: # 没有异常发生时被执行
        print ...
    finally: # 绝对会被执行
        print ...

**异常的经典应用**

    def describePerson(person):
        print "Description of", person['name']
        print 'Age', person['age']
        try:
            print 'Occupation', person['occupation']
        except KeyError: pass

    try:
        obj.write
    except AttributeError:
        print ...
    else:
        print ...


## 抽象

    class Filter:
        def init(self):
            self.blocked = []
        def filter(self, sequence):
            return [x for x in sequence if x not in self.blocked]
    class SPAMFilter(Filter):
        def init(self):
            self.blocked = ['SPAM']

    issubclass(SPAMFilter, Filter)


## 文件和素材

    open(name[, mode[, buffering]])
    mode: r, w, a, b, +
    buffering: 0, +num, -num
    buffering控制着文件的缓冲，+num表示使用num字节大小的内存区作为缓冲，只有当使用flush和close时才会写入硬盘。


## Python和万维网

1 屏幕抓取

    Tidy+HTMLParser
    from subprocess import Popen, PIPE
    text = open('messy.html').read()
    tidy = Popen('tidy', stdin=PIPE, stdout=PIPE, stderr=PIPE)
    tidy.stdin.write(text)
    tidy.stdin.close()
    print tidy.stdout.read() # 得到标准的XHTML格式的文档
    
    HTMLParser:
    handle_starttag(tag, attrs) # 找到开始标签时调用。attrs是（名称，值）对的序列
    handle_data(data) # 使用文本数据时调用
    handle_endtag(tag) # 找到标签结束时调用
    一个经典的案例
    from urllib import urlopen
    from HTMLParser import HTMLParser

    class Scraper(HTMLParser):
        in_h3 = False
        in_link = False

        def handle_starttag(self, tag, attrs):
            attrs = dict(attrs)
            if tag = 'h3':
                self.in_h3 = True
            if tag = 'a' and 'href' in attrs:
                self.in_link = True
                self.chunks = []
                self.url = attrs['href']
        def handle_data(self, data):
            if self.in_link:
                self.chunks.append(data)
        def handle_endtag(self, tag):
            if tag = 'h3':
                self.in_h3 = False
            if tag = 'a':
                if self.in_h3 and self.in_link:
                    print '%s (%s)' % (''.join(self.chunks), self.url)
                self.in_link = False
    text = urlopen('http://python.org/community/jobs').read()
    parser = Scraper()
    parser = feed(text)
    parser = close()


    "Beautiful Soup" FOR HTML
    "Scrape 'N' Feed" FOR RSS feed

**Beautiful Soup:**

    Docs: http://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html
    四种对象:
        1. Tag
        2. NavigableString
        3. BeautifulSoup
        4. Comment

    Tag:
    tag.name
    tag.attrs
    最常见的多值的属性是 class (一个tag可以有多个CSS的class). 还有一些属性 rel , rev , accept-charset , headers , accesskey . 在Beautiful Soup中多值属性的返回类型是list
    
    tag.string
    字符串常被包含在tag内

遍历文档树

    获取<body>标签中的第一个<b>标签:
    soup.body.b
    得到所有的<a>标签：
    soup.find_all('a')









    



