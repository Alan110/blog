---
title: 坑之原生对象继承
date: 2017-01-20 14:53:32
type: "categories"
categories: "踩过的坑"
tags:
---


## es5 继承原生对象

Boolean()
Number()
String()
Array()
Date()
Function()
RegExp()
Error()
Object()

在es5中，是没有办法继承原生构造函数的, 比如我们自己写一个Array


```javascript

var ArrayProto = Array.prototype;

function ResponseArray (){
    Array.apply(this, ArrayProto.slice.call(arguments));
}

ResponseArray.prototype = Object.create(ArrayProto);
ResponseArray.prototype.constructor = ResponseArray;

```

ResponseArray 的表现与原生Array并不一致，比如length的更新

```javascript
var colors = new MyArray();
colors[0] = "red";
colors.length  // 0

colors.length = 0; // 删除元素
colors[0]  // "red"
```

原因是ES5是先新建子类的实例对象this，再将父类的属性添加到子类上,**子类无法获得原生构造函数的内部属性**，通过Array.apply()或者分配给原型对象都不行。**原生构造函数会忽略apply方法传入的this**，也就是说，原生构造函数的this无法绑定，导致拿不到内部属性。

## ES6 继承
ES6允许继承原生构造函数定义子类，因为ES6是先新建父类的实例对象this，然后再用子类的构造函数修饰this，使得父类的所有行为都可以继承

```javascript
class MyArray extends Array {
  constructor(...args) {
    super(...args);
  }
}

var arr = new MyArray();
arr[0] = 12;
arr.length // 1

arr.length = 0;
arr[0] // undefined
```


