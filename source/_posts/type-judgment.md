---
title: JavaScript-类型判断
date: 2017-01-19 16:24:07
type: "categories"
categories: "JavaScript"
tags:
---

## 类型判断的方法

* `typeof`
* `instanceof`
* `Object.prototype.toString`
* `constructor`

各方法能力表
![](http://o99eh3ii0.bkt.clouddn.com//17-1-20/75308578-file_1484882147499_9f11.png)

`综合看constructor的表现最好，可以判断基本数据类型，内置对象，自定义对象。`

## typeof

**typeof 是判断数据原始值的类型**。
可能的字符串有："number"、"string"、"boolean"、"object"、"function" 和 "undefined"。
**不支持具体的object类型**

````javascript
typeof null == object  // true
typeof (function(){})  // function
````

## instanceof

判断实例与类的关系，`依赖constructor函数`。

* 能够判别内置对象类型
* **不能判别原始类型**
* 能够判别自定义类型

````javascript

//1. 能够判别内置对象类型
[] instanceof Array;//true
/\d/ instanceof RegExp;//true

//2. 不能判别原始类型
1 instanceof Number;//false
"xiaohong" instanceof String;//false

//3. 能够判别自定义类型
function Point(x, y) {
    this.x = x;
    this.y = y;
}
var c = new Point(2,3);
c instanceof Point;//true

//4. 局限性--依赖prototype
var obj = Object.create(null);
Object.getPrototypeOf(obj) null //obj确凿是一个对象,但它不是任何值的实例:
typeof obj 'object'
obj instanceof Object //false 
````


## Object.prototype.toString.call()

* 可以识别标准类型,及内置对象类型
* **不能识别自定义类型** `坑`

````javascript
//1. 可以识别标准类型,及内置对象类型
Object.prototype.toString.call(21);//"[object Number]"
Object.prototype.toString.call([]);//"[object Array]"
Object.prototype.toString.call(/[A-Z]/);//"[object RegExp]"

//2. 不能识别自定义类型
function Point(x, y) {
    this.x = x;
    this.y = y;
}

var c = new Point(2,3);//c instanceof Point;//true
Object.prototype.toString.call(c);//"[object Object]"

````

## constructor

* 可识别原始类型
* 可识别内置对象类型
* 可识别自定义类型

````javascript
//1. 可识别原始类型
"guo".constructor === String;//true
(1).constructor === Number;//true
true.constructor === Boolean;//true
({}).constructor === Object;//true

//2. 可识别内置对象类型
new Date().constructor === Date;//true
[].constructor === Array;//true

//3. 可识别自定义类型
function People(x, y) {
    this.x = x;
	this.y = y;
}
var c = new People(2,3);
c.constructor===People;//true
````


## 常用封装

````javascript

//判断类型--toString
function typeProto(obj) {
    return Object.prototype.toString.call(obj).slice(8,-1);
}

//判断类型--constructor
function getConstructorName(obj) {
    return obj && obj.constructor && obj.constructor.toString().match(/function\s*([^(]*)/)[1];
}

//变量是否定义(不等于null，也不等于undefined)
function isDefined(x) { 
	return x !== null && x !== undefined; 
}

//检测x是否是一个对象值
function isObject(x) { 
	return (typeof x === "function" || (typeof x === "object" && x !== null)); 
} 
````
