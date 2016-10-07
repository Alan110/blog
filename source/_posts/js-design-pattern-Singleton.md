---
title: js设计模式-单例模式
date: 2016-09-24 22:15:59
type: "categories"
categories: "设计模式"
tags:
---


在一些底层框架中，会很常见单例模式。即一个类只返回同一个实例。避免资源浪费，或者多实例争夺一份资源时的冲突。
js非常灵活，有很多种方式实现单例模式。这里介绍常见的几种。

> 字面量

```javascript
var obj = {
    name : 'fda',
    age : 'fds'
}
```
对象字面量是最简单的单例模式。但是实例是提前确定的。

> 缓存到构造函数上

```JavaScript
function Global() {

   // 缓存到构造函数的静态方法中
   // 弊端是外部可以修改实例属性
   if ( typeof Global.instance === 'object' ) {
       return Global.instance;
   }
   Global.instance = this;

   // 初始化
   this.name = 'Singleton';
   this.age = 12;
}

var g1 = new Global();
var g2 = new Global();

console.log( g1 === g2);

```

缓存实例，判断是否存在，存在即返回。可以在第一次new的时候确定实例。

> 缓存到闭包里面

```javascript
// 推荐
var Person = (function () {
    // 缓存到闭包中
    var instance; 

    function per() {
       if ( typeof instance === 'object' ) {
           return instance;
       }

       instance = this;
       // 初始化
       this.name = 'Singleton';
       this.age = 123;
    }

    return per;
})()

var p1 = new Person();
var p2 = new Person();
console.log(p1 === p2);
```
缓存实例到闭包里面，这样外部就不能修改了。
