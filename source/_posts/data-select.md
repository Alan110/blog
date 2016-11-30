---
title: 日志统计--数据采集
date: 2016-11-27 14:18:49
tags:
---


大数据时代，数据有着巨大的价值，其中蕴含着用户的大量信息，合理的使用数据，找到数据中有价值的信息可以为产品设计提供依据。用数据说话更令人信服。统计至关重要。

## 数据挖掘概况

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-11-27/23115520.jpg) 

数据采集是最核心的问题。数据采集是否丰富，采集的数据是否准确，采集是否及时，都直接影响整个数据平台的应用的效果。这也是本文集中讨论的重点。

准确性，时效性

时效性更多是体现在日志的传输建库上。比如在1个小时内我们就能够使用采集的日志进行分析。


## 数据采集

数据采集时一个需要考虑的问题是用前端统计，还是用后端统计。

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-11-27/41697522.jpg) 

前端统计适合看**趋势，看总量, 和用户行为**日志(点击，浏览，交互，时长等) 。 
后端统计适合对可靠性，准确性，安全性要求较高的日志统计(订单，金额，帖子, 用户等);

> 前端统计最大的问题就是因环境复杂，日志容易丢失

在做前端统计时，我们时长会遇到前后端日志match不上，前端日志丢失是一个大问题。比如这些场景：

* 统计某个链接的点击量，但是这个链接点击后直接跳走了。
* 统计页面时长，unload的时候发送的统计丢失了。
* img发送请求，跳走或者关闭页面时对象被垃圾回收了。
* 如果一个页面用户打开后没加载执行完（因为前置js错误、性能延时、主动关闭等）。应该算一个 pv/uv 吗？这种场景下，一般是认为不应该算的, 但是后端无法识别。

> 业内也有一些解决方案

针对问题一，我们的目的是希望保证请求能够发送出去，然后再跳转。 方案有很多

一、阻塞式ajax

阻塞页面关闭，当然可以在 readState 为 2 的时候就 abort 请求，因为我们不关心响应的内容，只要请求发出去就行了。

```javascript
window.addEventListener('unload', function(event) {
  var xhr = new XMLHttpRequest(),
  xhr.setRequestHeader("Content-Type", "text/plain;charset=UTF-8");
  xhr.open('post', '/log', false); // 同步请求
  xhr.send(data);
});
```

二、暴力死循环

```javascript
window.addEventListener('unload', function(event) {
  send(data);
  var now = +new Date;
  while(new Date - now >= 10) {} // 阻塞 10ms
});
```

三、发送一个图片请求

大部分浏览器都会等待图片的加载，趁这个机会把统计数据发送出去

```javascript
window.addEventListener('unload', function(event) {
  send(data);
  (new Image).src = 'http://example.com/s.gif';
});
```

四、通过window.name传递给下一页再发送
五、ios下pagehide兼容

	unload的兼容性相较于onbeforeonload，onpagehide是最好的。

六、localstorage缓存

七、beacon API

## 业内方案

### [TimeMe](https://github.com/jasonzissman/TimeMe.js)

核心是利用同步ajax请求，在onbeforeonload的时候确保请求发出去。但onbeforeunload有兼容性问题。

```javascript
<script>
var startTime = (new Date()).getTime();

window.onbeforeunload = function (event) {
    var timeSpent = (new Date()).getTime() - startTime,
        xmlhttp= new XMLHttpRequest();
    xmlhttp.open("POST", "your_url");
    xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    var timeSpentOnPage = TimeMe.getTimeOnCurrentPageInSeconds();
    xmlhttp.send(timeSpent);
};
</script>
```

## 产品指标


## 引用

[同一窗口内跨页面数据传递--window.name](https://github.com/aralejs/name-storage)
