---
title: charles抓包工具教程
date: 2016-10-09 16:47:25
type: "categories"
categories: "网络"
tags:
---


Charles是mac上的一款比较好用的抓包工具。

**在使用Charles代理的时候，需要关闭其他代理。**

选择MAC OS X Proxy，自动代理系统的网络请求。
![](http://o99eh3ii0.bkt.clouddn.com//16-10-9/42022369.jpg)

开始代理，在浏览器访问地址，就可以在监控面板看到了。
![](http://o99eh3ii0.bkt.clouddn.com//16-10-9/22250920.jpg)

## [https代理](https://www.charlesproxy.com/documentation/using-charles/ssl-certificates/)

在访问https的资源的时候，我们会看到请求都是unknown的，这是因为https的代理需要证书。证书下载http://yun.baidu.com/s/1o6J2Crg
注意将证书权限设置为始终信任。

> mac 

直接安装, 注意将证书权限设置为始终信任。在help面板里面，有pc端和移动端的证书快捷安装方式。
![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-7/15128752.jpg ) 

> ios

ios 需要下载证书。[hree](http://www.charlesproxy.com/getssl)。也可以直接安装

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-7/27964601.jpg ) 

> 设置

设置需要代理的域名，端口为443

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-7/38781483.jpg ) 


## 手机请求代理

手机和电脑需要在同一一个wifi下。
打开手机的网络，选择代理，自动。填写电脑的ip，和Charles的监听端口
<img src="http://o99eh3ii0.bkt.clouddn.com//16-10-10/44283958.jpg" style="width:300px" />


## 证书信任问题

在高级的ios版本中，代理https可能会出现证书信任问题。 `Failure SSLHandshake: Received fatal alert: unknown_ca`
原因是ios高版本中需要手动确认，信任的证书。`通用->关于本机->证书信任设置 打开才有效果 `


## 远程代理

Charles可以将请求代理到远程链接(mapping remote)，也可以代理到本地文件(mapping local)。

![](http://o99eh3ii0.bkt.clouddn.com/FoLoz_oUlmofKFRliBLjPJRaZ8Ks)

填写要代理的path，代理到的path。

这里有个技巧，**把完整的path拷贝填入host，然后光标移动到下面的输入框**，就会自动拆解填入