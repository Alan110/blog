---
title: cookie跨域共享
date: 2016-11-26 14:56:18
tags:
categories: "网络"
---


## 基本介绍

cookie能够保存少量的信息在浏览器端。我们知道http协议是无状态的。每次访问，都是一次新的访问，如果我们希望记录一些浏览信息。就需要依靠cookie来存储，比如登陆信息，比如浏览信息。

cookie可以在服务端设置，也可以在客户端设置。其大小和个数有限制。一个cookie大小限制在2k。一个站点最多有20个cookie。

cookie有同源策略，不同域下，是不能读写的。

cookie如果不设置过期时间，则只会保存在内存里面。当浏览器关闭时就会销毁。

如果设置了过期时间，就会以文件的形式保存在硬盘里，到了过期时间就会销毁。

当访问一个域时，这个域下所有的cookie都会被带上在http的请求头里，不管是前端设置的，还是在后端设置的。

## 基本操作

```javascript
document.cookie = 'name=alan;age=11;expires=121321';
```
cookie 是以；分割的键值对字符串。网上有很多教程，这里不多说。[基础教程](http://www.jb51.net/article/64330.htm)

Cookie是可以覆盖的，如果重复写入同名的Cookie，那么将会覆盖之前的Cookie。

## 同主域跨域

默认情况下，cookie只在相同服务端路径下的文件可以访问，设置path='/', 几可以在整个站点共享，跨目录。
设置domain=".baidu.com", 则相同主域下cookie都可以共享。例如a.baidu.com, 和b.baidu.com 

## 跨主域写

在SSO单点登录系统中，会有多个主域名，只要登录了其中一个域，那么访问别的域就不需要登录了。实现这个的关键就是跨域的cookie共享。

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-11-26/97630655.jpg) 

我们可以利用script标签的src属性发送跨域请求。这种方式优点在于我们可以向任意多的域发送请求。并且共享cookie。

例如用户登录了A.com, 获得了服务端写的cookie，其中包含登录的token。这时前端发送跨域请求，并带上获得的token给其他域，其他域再写cookie，这样就共享了一个登录token。用户访问b.com的时候就不要输入登录密码了。

*也还有别的方案，比如iframe等。我们的目的是传递cookie值就行了。不过iframe的体验不太好，比较trike，这里不多说。*

## 跨主域读

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-11-26/29022526.jpg) 

有时候我们需要读取别的域下的cookie，也可以利用script的跨域能力。
在访问a域时，向b域发起跨域请求，b域服务端获取到所有的cookie并以jsonp的形式返回。前端就能拿到cookie信息了。

> **跨域读，跨域写都是利用jsonp，或者image的跨域访问的能力，将当前域的信息带到别的域处理，需要后端配合**


## cors

这里不得不提一下cors的几个属性, 当指定携带用户凭证的时候，域名不能使用通配符，必须指定详细的域名。
且，`cookie仍然遵循同源策略，所以只有当前服务器的域名设置的cookie才能携带`，不能携带客户端cookie，所以`cors不能实现跨域cookie共享`

```
Access-Control-Allow-Credentials : true  // 携带用户凭证，cookie
Access-Control-Allow-Origin : http://www.baidu.com  // 指定可跨域的域名
```

## UV统计

在统计中很常见，统计用户行为轨迹。这个实现一般是在cookie中记录用户的唯一ID。所有的请求都会带上这个唯一ID，在日志分析的时候，按照这个ID merge日志就能得出这个用户的行为了。
