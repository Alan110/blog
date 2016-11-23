---
title: clearInterval 无法清除定时器
date: 2016-11-21 16:24:11
type: "categories"
categories: "踩过的坑"
tags:
---


今天遇到一个奇怪的问题，clearTimeout失效了。大概这么一个场景

```javascript

var timer = null;

for(var i=0; i<2;i++){
	var timer = setInterval(function(){
		console.log('xxxx');
	},3000);
}

clearInterval(timer);

```

结果就是timer依然在运行。浓缩到这么几行代码，可能比较容易看出问题来，但是分散到比较长的代码里面就很难看出来了。

问题就是setInterval重复注册了。每执行setinterval一次，会返回一个注册id，重复执行时，以前的id被覆盖了，就永远也找不到那个id了，当然这个时候调用clearInterval也没用了。

**所以避免的方法就是在每次setInterval之前，先clearInterval, 如果有多个定时器，请一定要保存所有的句柄，一旦出现不可控句柄，问题就严重了**

```javascript

var timer = null;

for(var i=0; i<2;i++){
	clearInterval(timer);
	var timer = setInterval(function(){
		console.log('xxxx');
	},3000);
}

clearInterval(timer);

```

> 总结

也算是一个坑吧，需要小心注意。setTimeout也是类似的，要注意。
