---
title: restful api设计
date: 2017-08-10 09:34:40
tags:
---


前后端分离大行其道，往往是多个客户端，一个服务后端，后端变得服务化。接口越来越多，**保证api的设计更符合人性化，简单，容易理解显得非常重要**

网站的本质是软件，互联网软件具有分布式的特点。**对api的设计是属于软件架构的一部分。**

Fielding将他对互联网软件的架构原则，定名为REST，即Representational State Transfer的缩写。我对这个词组的翻译是"表现层状态转化"。

REST是**互联网软件架构设计原则**,它不是实体。具有rest特点的软件，我们称其为restful。

其实它所描述的是---"资源表现层状态转移"。 api其实也是资源的访问，和状态变换。那么对api抽象为标准成为可能。

## 资源

互联网软件，通过http实现资源的交流。我们上网其实就是访问各种资源。以及资源的"状态转移"。

## 表现层

资源的表现形式, 如.png,.text,html, .css 等等。代表了各种各样的资源

## 状态转化

http是无状态的，资源的一切状态都存储在服务器上的。所以客户端需要通过某种手段告诉服务器进行状态转化

客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，四个表示操作方式的动词：**GET、POST、PUT、DELETE**。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。

## 综述

综上所述，restful架构具有以下特点:
1. 每一个URI代表一种资源
2. uri是名词，不能包含动词
3. 客户端通过四个HTTP动词，对服务器端资源进行操作描述,如果某些动作是HTTP动词表示不了的，你就应该把动作做成一种资源

```
GET（SELECT）：从服务器取出资源（一项或多项）。
POST（CREATE）：在服务器新建一个资源。
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
DELETE（DELETE）：从服务器删除资源。
```


## 例子

```
GET /zoos：列出所有动物园
POST /zoos：新建一个动物园
GET /zoos/ID：获取某个指定动物园的信息
PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID：删除某个动物园
GET /zoos/ID/animals：列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物
```

## 参考

http://www.ruanyifeng.com/blog/2014/05/restful_api.html 
http://www.ruanyifeng.com/blog/2011/09/restful.html