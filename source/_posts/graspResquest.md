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

直接安装, 注意将证书权限设置为始终信任。
![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-7/15128752.jpg ) 

> ios

ios 需要下载证书。[hree](http://www.charlesproxy.com/getssl)

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-7/27964601.jpg ) 

> 设置

设置需要代理的域名，端口为443

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-12-7/38781483.jpg ) 


## 手机请求代理

手机和电脑需要在同一一个wifi下。
打开手机的网络，选择代理，自动。填写电脑的ip，和Charles的监听端口
<img src="http://o99eh3ii0.bkt.clouddn.com//16-10-10/44283958.jpg" style="width:300px" />
