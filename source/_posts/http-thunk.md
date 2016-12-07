---
title: http--thunk 和 content-length
date: 2016-12-07 13:23:16
tags:
categories: "网络"
---


## content-length

content-length是消息传输实体的传输长度,在HTTP/1.0下，content-length可有可无。因为通过服务器端关闭连接可以确定消息的传输长度，谓之短连接，即非keep-live模式

## chunk

在HTTP/1.1下，如果设置了transfer-Encoding，说明消息长度和消息传输长度不等，此时不能设置content-length，有也会被忽视。如果没有设置transfer-Encoding，那么情况就和HTTP1.0一样，可以是非Keep-live模式，也可以设置Content-length。顾名思义,Transfer-Encoding下Chunk模式是在HTTP/1.1才有的模式，即分块传输。


## 为什么会有chunk模式

如果服务器准备发送的内容需要动态创建，那么在创建完成之前，消息大小都是未知的，那么消息头就无法生成，content-length无法计算。而Chunk模式允许消息头在传输完成之发送，这就可以在消息大小生成前发送消息内容。如果没有这种模式，会造成浏览器端迟迟收不到响应，因为服务器处于动态创建内容的缓冲状态。



## gzip + chunk

如果消息通过gzip压缩的话，那么就可以压缩与传输并行，也就是说，压缩一块消息，传输一块一块消息，边压缩，边传输，这样可以提高传输效率。


## chunk 格式

```
Chunked-Body   = *chunk  
                 last-chunk  
                 trailer  
                 CRLF  
  
chunk          = chunk-size [ chunk-extension ] CRLF  
                 chunk-data CRLF  
chunk-size     = 1*HEX  
last-chunk     = 1*("0") [ chunk-extension ] CRLF  
  
chunk-extension= *( ";" chunk-ext-name [ "=" chunk-ext-val ] )  
chunk-ext-name = token  
chunk-ext-val  = token | quoted-string  
chunk-data     = chunk-size(OCTET)  
trailer        = *(entity-header CRLF)  
```

格式如上， 简单说明一下，主要有三个大部分：

首先，Chunk每一个非空的部分有俩个部分组成：

1. chunk-size及其长度单位 
2. chunk-data

**其次，Chunk串的最后一块是一个长度为0的块（last-chunk）**
最后，Chunk最后一块的后面跟着一个可选的尾部trail（包括消息头header）
CRLF是指回车换行


## 参考

[chunk编码](http://blog.csdn.net/b2997215859/article/details/51169784)
