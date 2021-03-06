﻿# 告知服务器意图的HTTP方法

标签（空格分隔）： HTTP

---

###GET:获取资源
GET方法用来请求访问已被URI识别的资源。指定的资源经过服务器端解析后返回响应内容，也就是说，如果请求的资源是文本，那就保持原样返回;如果是像CGI（Common Gateway interface，通用网关接口）那样的程序，则返回经过执行后的输出结果。
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciwwh7v0dj30p008cgmj)
#####使用GET方法的请求~响应的例子
| 请求        |  GET /index.html HTTP/1.1    Host:www.dafada.jp| 
| :---:   | :-----:  |
|  响应      |  返回index.heml的页面资源    |

| 请求        |  GET /index.html HTTP/1.1    Host:www.dafada.jp If-Modified-Since: Thu ,12 Jul 2016 08:30:00 GMT| 
| :---:   | :-----:  |
|  响应      |  仅返回2012年7月12日七点三十分以后更新过的index.html页面资源。如果未有内容更新，则以状态吗304 Not Modified作为响应返回    |
###POST：传输实体主体
POST方法用来传输实体的主体
虽然用GET方法也可以传输实体的主体，但一般不用GET方法进行传输，而是用POST方法，虽说POST的功能与GET方法很相似，但是POST的主要目的并不是获取响应的主体内容。
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciww744gbj30q809umyb)
#####使用POST方法的请求~响应的例子
| 请求        |  POST /submit.cgi HTTP/1.1    Host:www.dafada.jp  Content-Length:1560(1560字节的数据)| 
| :---:   | :-----:  |
|  响应      |  返回submit.cgi接受数据的处理结果    |
###PUT：传输文件
PUT方法用来传输文件，就像FTP协议的文件上传一样，要求在请求报文的主题中包含文件内容，然后保存到请求的URI指定的位置。
但是鉴于HTTP/1.1的PUT方法自身不带验证机制，任何人都可以上传文件，存在安全性问题，因此一般的Web网址不使用该方法。若配合Web应用程序的验证机制，活架构设计采用REST（Representational State Transfer 表征状态转移）标准的同类的Web网址，就可能会开放使用PUT方法
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciwpaiorjj30q009ogms)
#####使用PUT方法的请求~响应的例子
| 请求        |  PUT /index.html HTTP/1.1    Host:www.dafada.jp Content-Type:text/html Content-Length:1560(1560字节的数据)| 
| :---:   | :-----:  |
|  响应      |  响应返回状态吗204 Not Content(比如：该html已经存在于服务器上)    |
###HEAD：获取报文首部
HEAD方法和GET方法一样，只是不返回报文主体部分，用于确认URI的有效性及资源更新的日期时间等
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciwvpfog3j30og0bmwfw)
#####使用HEAD方法的请求~响应的例子
| 请求        |  HEAD /index.html HTTP/1.1    Host:www.dafada.jp | 
| :---:   | :-----:  |
|  响应      |  返回index.html有关的响应首部    |
###DELETE：删除文件
DELETE方法用来删除文件，是与PUT相反的方法，DELETE方法按请求URI删除指定的资源
但是，HTTP/1.1的DELETE方法本身和PUT方法一样不带有验证机制，所以一般的Web网站也不使用DELETE方法，并配合Web应用缓存层序的验证机制，或者遵守REST标准时还是有可能会开放使用的。
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciwuvw34zj30nq098wfk)
#####使用DELETE方法的请求~响应的例子
| 请求        |  DELETE /index.html HTTP/1.1    Host:www.dafada.jp | 
| :---:   | :-----:  |
|  响应      |  响应返回状态码204 No Content(比如：该html已经从该服务器上删除)    |
###OPTIONS:询问支持的方法
OPTIONS方法用来查询针对请求URI指定的资源支持的方法
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciwvgamhnj30sm0b2tai)
#####使用OPTIONS方法的请求~响应的例子
| 请求        |  OPTIONS * HTTP/1.1    Host:www.dafada.jp | 
| :---:   | :-----:  |
|  响应      |  HTTP/1.1 200OK Allow：GET/POST/HEAD/OPTIONS(返回服务器支持的方法)    |
###TRACE:追中路径
TRACE方法是让Web服务器端将之前的请求通信环回给客户端的方法
发送请求的时候，在Max-Frowards首部字段中填入数值，每经过一个服务器端就讲该数字减一，当数值刚好减为0的时候，就停止继续传输，最后接受到请求的服务器端则返回状态码200 OK的响应
客户端通过TRACE方法可以查询发送出去的请求是怎样被加工修改/篡改的，这是因为，请求想要链接到源目标服务器可能会通过代理中转，TRACE方法就是用来确认连接过程中发生的一系列操作。
但是TRACE方法本来就不怎么使用，而且加上它容易引发XST（Cross-Site Tracing 跨站追中）攻击，通常就更不会用到了
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciwv5yrc5j30oo0a2tai)
#####使用TRACE方法的请求~响应的例子
| 请求        |  TRACE  /HTTP/1.1    Host:www.dafada.jp  Max-Forwards:2| 
| :---:   | :-----:  |
|  响应      |  HTTP/1.1 200OK Content-Type:message/http Content-Length:1024 TRACE /HTTP/1.1  Host:www.dafada.jp Max-Forwards:2(返回响应包含请求内容)   |
###CONNECT:要求用隧道协议连接代理
CONNECT方法要求在于代理服务器通信时候建立隧道，实现用隧道协议进行TCP通信，主要使用SSL（Secure Sockets Layer,安全套接层）和TLS（Transport Layer Security 传输层安全）协议把通信内容加密后经网络隧道传输
![](http://ww1.sinaimg.cn/large/006jcGvzgy1fciwvybktnj30pc09sdh6)
#####使用CONNECT方法的请求~响应的例子
| 请求        |  CONNECT  proxy.hackr.jp:8080 /HTTP/1.1    Host:www.dafada.jp  | 
| :---:   | :-----:  |
|  响应      |  HTTP/1.1 200OK (之后进入网络隧道)   |




