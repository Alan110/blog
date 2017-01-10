---
title: 前端行为测试
date: 2016-08-25 20:50:04
type: "categories"
categories: "前端自动化测试"
tags:
---

> [phantomjs](https://www.npmjs.com/package/karma)

PhantomJS 是一个基于 WebKit的无头浏览器。原生支持各种web标准:  DOM 处理, CSS 选择器, JSON, Canvas, 和 SVG 。 PhantomJS 可以用于 页面自动化 ， 网络监测 ， 网页截屏 ，以及 无界面测试 等。

所以，可以把phantomjs就想象成一个浏览器，它提供了很多浏览器级别的api，供开发者使用。它可以注入本地，或者远程js代码到目标页面，对于自动化模拟用户操作很有帮助。

可以在目标页面沙箱执行js代码。

可以与断言库结合，利用console报表与onMessage事件通信，在真实环境下跑单侧

> 总结：灵活性高，可以做爬虫，可以做测试，可以做监控，但对代码质量要求也高，同步，异步代码组织困难。

____

> [casperjs](http://docs.casperjs.org/en/latest/installation.html)

![](http://o99eh3ii0.bkt.clouddn.com//16-9-9/35599761.jpg)
casperjs是基于phantomjs封装的一个web应用测试框架。

它封装了phantomjs的api，并提供了一套高级的测试api,以及断言库。开发者不用自己去维护phantomjs的异步，同步代码，执行顺序等问题。更专注于浏览器测试本身。

可以支持单元测试和浏览器测试,但是不支持其他的断言库

> 总结：web测试的最佳选择，简单，完整。

____

> slimerjs
