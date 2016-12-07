---
title: http 入门
date: 2016-11-17 13:17:32
tags:
categories: "网络"
---

HTTP是Hyper Text Transfer Protocol（超文本传输协议）的缩写。是从WWW服务器传输超文本到本地浏览器的应用层传送协议。特点是无状态。其中最著名的就是RFC 2616。RFC 2616定义了今天普遍使用的一个版本——HTTP 1.1。

HTTP/1.0 每次请求都需要建立新的TCP连接，连接不能复用。HTTP/1.1 新的请求可以在上次请求建立的TCP连接之上发送，连接可以复用。优点是减少重复进行TCP三次握手的开销，提高效率。 注意：在同一个TCP连接中，新的请求需要等上次请求收到响应后，才能发送。

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-5/18925668.jpg ) 


## 整体结构

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-4/96070069.jpg ) 


## http request

> 请求方法

请求方法（所有方法全为大写）有多种，各个方法的解释如下：


| name            | describe                                          |
| : ------------- | :-------------                                    |
|GET    | 请求获取Request-URI所标识的资源                    |
|POST   | 在Request-URI所标识的资源后附加新的数据|
|HEAD   | 请求获取由Request-URI所标识的资源的响应消息报头|
|PUT    | 请求服务器存储一个资源，并用Request-URI作为其标识|
|DELETE | 请求服务器删除Request-URI所标识的资源|
|TRACE  | 请求服务器回送收到的请求信息，主要用于测试或诊断|
|CONNECT| 保留将来使用|
|OPTIONS| 请求查询服务器的性能，或者查询与资源相关的选项和需求   |

> [请求报头](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)

基本的就不多说了，可以看维基百科。这里列一些特殊的

| name            | describe                                          |
| : ------------- | :-------------                                    |
|Accept   |请求报头域用于指定客户端接受哪些类型的信息。eg：Accept：image/gif，表明客户端希望接受GIF图象格式的资源；Accept：text/html，表明客户端希望接受html文本。|
|Accept-Charset |请求报头域用于指定客户端接受的字符集。eg：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。|
|Accept-Encoding |请求报头域类似于Accept，但是它是用于指定可接受的内容编码。eg：Accept-Encoding:gzip.deflate.如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。|
|Accept-Language |请求报头域类似于Accept，但是它是用于指定一种自然语言。eg：Accept-Language:zh-cn.如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。|
|Authorization |请求报头域主要用于证明客户端有权查看某个资源。当浏览器访问一个页面时，如果收到服务器的响应代码为401（未授权），可以发送一个包含Authorization请求报头域的请求，要求服务器对其进行验证。|
|Host |请求报头域主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的，eg： 我们在浏览器中输入：http://www.guet.edu.cn/index.html 浏览器发送的请求消息中，就会包含Host请求报头域，如下： Host：www.guet.edu.cn 此处使用缺省端口号80，若指定了端口号，则变成：Host：www.guet.edu.cn:指定端口号|
|If-Modified-Since |用于告诉服务器，资源的缓存时间|
|Referer | 用于告诉服务器，客户机访问服务器通过资源（防盗链）|
|User-Agent | 客户机的软件环境|
|Cookie | 客户机向服务器带数据|
|Connection:close/keep-alive  |一个请求完毕后是关闭连接还是保持连接|



## http response

HTTP响应也是由三个部分组成，分别是：状态行、消息报头、响应正文

> 状态行

状态行格式 :  HTTP-Version Status-Code Reason-Phrase CRLF
其中，HTTP-Version表示服务器HTTP协议的版本；Status-Code表示服务器发回的响应状态代码；Reason-Phrase表示状态代码的文本描述。

> 状态码

状态代码有三位数字组成，第一个数字定义了响应的类别，且有五种可能取值：

| name            | describe                                          |
| : ------------- | :-------------                                    |
|1xx：|指示信息--表示请求已接收，继续处理|
|2xx：|成功--表示请求已被成功接收、理解、接受|
|3xx：|重定向--要完成请求必须进行更进一步的操作|
|4xx：|客户端错误--请求有语法错误或请求无法实现|
|5xx：|服务器端错误--服务器未能实现合法的请求|

常见状态代码

| name            | describe                                          |
| : ------------- | :-------------                                    |
|200 OK      |客户端请求成功
|400 Bad Request|  客户端请求有语法错误，不能被服务器所理解|
|401 Unauthorized |请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 |
|403 Forbidden|  服务器收到请求，但是拒绝提供服务|
|404 Not Found | 请求资源不存在，eg：输入了错误的URL|
|500 Internal Server Error |服务器发生不可预期的错误|
|503 Server Unavailable | 服务器当前不能处理客户端的请求，一段时间后可能恢复正常|



> [响应报头](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)

| name            | describe                                          |
| : ------------- | :-------------                                    |
|Access-Control-Allow-Origin|指定跨域共享的站点域名|
|Cache-Control: max-age=3600|  缓存时间|
|Location| 重定向uri|
|Set-Cookie| 服务端设置cookie|
