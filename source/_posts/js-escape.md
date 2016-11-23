---
title: json字符串中带json字符串
date: 2016-08-23 21:36:41
type: "categories"
categories: "踩过的坑"
tags:
---


## 什么是转义字符

转义字符是一些特殊的字符,可以用反斜杠在文本字符串中插入省略号、换行符、引号和其他特殊字符。
比如这种 :

```javascript
var txt="We are the so-called "Vikings" from the north."
document.write(txt)
```

> 转义字符是有一定意义的，在赋给一个变量后，会立即转为相应的字符。

![](http://o99eh3ii0.bkt.clouddn.com/16-8-23/21328790.jpg)

> 所以看到转义，可以直接把他替换为相应的字符来理解。上图的转义"号，三个"号就报错了。

## JavaScript的转义字符

<img src="http://o99eh3ii0.bkt.clouddn.com/16-8-23/8874483.jpg"  />

## 传递转义

那如何传递转义呢？

也就是我踩的这个坑。json字符串中的一个属性是json字符串。

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-11-21/26026330.jpg ) 

当我最后再parse回去的时候，就报错了。通过前文，可知我们可以将\" 直接看成 " , 就可以发现有明显语法错误了。

这里是需要保留转义的能力，而不是立即转换。 所以需要双转义，将\本身也转义了，作为字符串的一部分传递给JSON.parse。

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//public/16-11-21/48849402.jpg) 



