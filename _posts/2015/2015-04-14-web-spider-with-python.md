---
layout: post
title: [Python]网络爬虫学习笔记(转自：汪海blog)
categories:
- 爬虫
tags:
- 爬虫
- python
---

## 前期调研

[](http://www.nowamagic.net/academy/detail/1302879)
[Python 之 BNUOJ代码抓取器](http://blog.csdn.net/sssogs/article/details/8574595)
[](http://www.isaced.com/post-197.html)
[](http://bbs.chinaunix.net/thread-3779115-1-1.html)


## 异常处理和HTTP状态码分类

1. URLError
2. HTTPError

**HTTP状态码**

    200：请求成功      处理方式：获得响应的内容，进行处理 
    201：请求完成，结果是创建了新资源。新创建资源的URI可在响应的实体中得到    处理方式：爬虫中不会遇到 
    202：请求被接受，但处理尚未完成    处理方式：阻塞等待 
    204：服务器端已经实现了请求，但是没有返回新的信 息。如果客户是用户代理，则无须为此更新自身的文档视图。    处理方式：丢弃
    300：该状态码不被HTTP/1.0的应用程序直接使用， 只是作为3XX类型回应的默认解释。存在多个可用的被请求资源。    处理方式：若程序中能够处理，则进行进一步处理，如果程序中不能处理，则丢弃
    301：请求到的资源都会分配一个永久的URL，这样就可以在将来通过该URL来访问此资源    处理方式：重定向到分配的URL
    302：请求到的资源在一个不同的URL处临时保存     处理方式：重定向到临时的URL
    304 请求的资源未更新     处理方式：丢弃 
    400 非法请求     处理方式：丢弃 
    401 未授权     处理方式：丢弃 
    403 禁止     处理方式：丢弃 
    404 没有找到     处理方式：丢弃 
    5XX 回应代码以“5”开头的状态码表示服务器端发现自己出现错误，不能继续执行请求    处理方式：丢弃


**异常处理经典案例**

    from urllib2 import Request, urlopen, URLError, HTTPError
    
    req = Request('http://bbs.csdn.net/callmewhy')
    try:
        response = urlopen(req)
    except HTTPError, e:
        print 'The server couldn\'t fulfill the request.'
        print 'Error code: ', e.code
    except URLError, e:
        print 'We failed to reach a server.'
        print 'Reason: ', e.reason
    else:
        print 'No exception was raised.'
        # everything is fine



## Opener与Handler

...


## urllib2的使用细节与抓站技巧

使用说明：

**1 proxy的设置**

urllib2 默认会使用环境变量 **http_proxy** 来设置 HTTP Proxy。如果想在程序中明确控制 Proxy，而不受环境变量的影响，可以使用下面的方式

    import urllib2
    
    enable_proxy = True
    proxy_handler = urllib2.ProxyHandler({"http" : 'http://some-proxy.com:8080'})
    null_proxy_handler = urllib2.ProxyHandler({})
    
    if enable_proxy:
        opener = urllib2.build_opener(proxy_handler)
    else:
        opener = urllib2.build_opener(null_proxy_handler)
    
    urllib2.install_opener(opener)
这里要注意的一个细节，使用 **urllib2.install_opener()** 会设置 urllib2 的全局 opener。这样后面的使用会很方便，但不能做更细粒度的控制，比如想在程序中使用两个不同的 Proxy 设置等。比较好的做法是不使用 install_opener 去更改全局的设置，而只是直接调用 opener 的 open 方法代替全局的 urlopen 方法。

**2 Timeout 设置**

在老版本中，urllib2 的 API 并没有暴露 Timeout 的设置，要设置 Timeout 值，只能更改 Socket 的全局 Timeout 值。

import urllib2
import socket
 
socket.setdefaulttimeout(10) # 10 秒钟后超时
urllib2.socket.setdefaulttimeout(10) # 另一种方式
在新的 Python 2.6 版本中，超时可以通过 urllib2.urlopen() 的 timeout 参数直接设置。

import urllib2
response = urllib2.urlopen('http://www.google.com', timeout=10)


**3 在 HTTP Request 中加入特定的 Header**

要加入 Header，需要使用 Request 对象：

import urllib2
 
request = urllib2.Request(uri)
request.add_header('User-Agent', 'fake-client')
response = urllib2.urlopen(request)
对有些 header 要特别留意，Server 端会针对这些 header 做检查

User-Agent 有些 Server 或 Proxy 会检查该值，用来判断是否是浏览器发起的 Request
Content-Type 在使用 REST 接口时，Server 会检查该值，用来确定 HTTP Body 中的内容该怎样解析。
 

常见的取值有：

application/xml ：在 XML RPC，如 RESTful/SOAP 调用时使用
application/json ：在 JSON RPC 调用时使用
application/x-www-form-urlencoded ：浏览器提交 Web 表单时使用

……
 

在使用 RPC 调用 Server 提供的 RESTful 或 SOAP 服务时， Content-Type 设置错误会导致 Server 拒绝服务。

**4 Redirect**

urllib2 默认情况下会针对 3xx HTTP 返回码自动进行 Redirect 动作，无需人工配置。要检测是否发生了 Redirect 动作，只要检查一下 Response 的 URL 和 Request 的 URL 是否一致就可以了。

import urllib2
response = urllib2.urlopen('http://www.google.cn')
redirected = response.geturl() == 'http://www.google.cn'
如果不想自动 Redirect，除了使用更低层次的 httplib 库之外，还可以使用自定义的 HTTPRedirectHandler 类。

import urllib2
 
class RedirectHandler(urllib2.HTTPRedirectHandler):
    def http_error_301(self, req, fp, code, msg, headers):
        pass
    def http_error_302(self, req, fp, code, msg, headers):
        pass
 
opener = urllib2.build_opener(RedirectHandler)
opener.open('http://www.google.cn')

**5 Cookie**

urllib2 对 Cookie 的处理也是自动的。如果需要得到某个 Cookie 项的值，可以这么做：

import urllib2
import cookielib
 
cookie = cookielib.CookieJar()
opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookie))
response = opener.open('http://www.google.com')
for item in cookie:
    if item.name == 'some_cookie_item_name':
        print item.value

**6 使用 HTTP 的 PUT 和 DELETE 方法**
urllib2 只支持 HTTP 的 GET 和 POST 方法，如果要使用 HTTP PUT 和 DELETE，只能使用比较低层的 httplib 库。虽然如此，我们还是能通过下面的方式，使 urllib2 能够发出 HTTP PUT 或 DELETE 的包：

import urllib2
 
request = urllib2.Request(uri, data=data)
request.get_method = lambda: 'PUT' # or 'DELETE'
response = urllib2.urlopen(request)
这种做法虽然属于 Hack 的方式，但实际使用起来也没什么问题。

**7 得到 HTTP 的返回码**

对于 200 OK 来说，只要使用 urlopen 返回的 response 对象的 getcode() 方法就可以得到 HTTP 的返回码。但对其它返回码来说，urlopen 会抛出异常。这时候，就要检查异常对象的 code 属性了：

import urllib2
try:
    response = urllib2.urlopen('http://restrict.web.com')
except urllib2.HTTPError, e:
    print e.code

**8 Debug Log**

使用 urllib2 时，可以通过下面的方法把 Debug Log 打开，这样收发包的内容就会在屏幕上打印出来，方便我们调试，在一定程度上可以省去抓包的工作。

import urllib2
 
httpHandler = urllib2.HTTPHandler(debuglevel=1)
httpsHandler = urllib2.HTTPSHandler(debuglevel=1)
opener = urllib2.build_opener(httpHandler, httpsHandler)
 
urllib2.install_opener(opener)
response = urllib2.urlopen('http://www.google.com')


**9.表单的处理**

登录必要填表，表单怎么填？
首先利用工具截取所要填表的内容。
比如我一般用firefox+httpfox插件来看看自己到底发送了些什么包。
以verycd为例，先找到自己发的POST请求，以及POST表单项。
可以看到verycd的话需要填username,password,continueURI,fk,login_submit这几项，其中fk是随机生成的（其实不太随机，看上去像是把epoch时间经过简单的编码生成的），需要从网页获取，也就是说得先访问一次网页，用正则表达式等工具截取返回数据中的fk项。continueURI顾名思义可以随便写，login_submit是固定的，这从源码可以看出。还有username，password那就很显然了：
[python] view plaincopy
# -*- coding: utf-8 -*-  
import urllib  
import urllib2  
postdata=urllib.urlencode({  
    'username':'汪小光',  
    'password':'why888',  
    'continueURI':'http://www.verycd.com/',  
    'fk':'',  
    'login_submit':'登录'  
})  
req = urllib2.Request(  
    url = 'http://secure.verycd.com/signin',  
    data = postdata  
)  
result = urllib2.urlopen(req)  
print result.read()   

**10.伪装成浏览器访问**

某些网站反感爬虫的到访，于是对爬虫一律拒绝请求
这时候我们需要伪装成浏览器，这可以通过修改http包中的header来实现
[python] view plaincopy
#…  
  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6'  
}  
req = urllib2.Request(  
    url = 'http://secure.verycd.com/signin/*/http://www.verycd.com/',  
    data = postdata,  
    headers = headers  
)  
#...  

**11.对付"反盗链"**

某些站点有所谓的反盗链设置，其实说穿了很简单，
就是检查你发送请求的header里面，referer站点是不是他自己，
所以我们只需要像把headers的referer改成该网站即可，以cnbeta为例：
#...
headers = {
    'Referer':'http://www.cnbeta.com/articles'
}
#...
headers是一个dict数据结构，你可以放入任何想要的header，来做一些伪装。
例如，有些网站喜欢读取header中的X-Forwarded-For来看看人家的真实IP，可以直接把X-Forwarde-For改了。


## 一个简单的百度贴吧的小爬虫

...


