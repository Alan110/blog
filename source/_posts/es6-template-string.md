---
title: es6-模板字符串
date: 2017-07-21 07:58:34
tags:
---

es6新增了模板字符串，增强了字符串变量的能力。


## 语法

模板字符串支持多行字符串，变量插值，以及函数标签。在模版字符串内使用反引号（`）时，需要在它前面加转义符（\）

```javascript
let str = `string text`

let str = `string text line 1
           string text line 2 `

let str = `string text ${expression} string text`

let str = tag `string text ${expression} string text`

 ```

 在${ } 内，支持任意的js表达式  

 ## 标签函数

标签函数可以理解为一个过滤器，filter 对模板字符串的输出做**进一步过滤**。
**返回值也不一定是一个字符串**，可以是个函数或者别的类型

参数：
 * strings [] 字符串的字面值数组，按照 ${} 符号分割，在下面的例子中，分别为 Hello 、world
 * $1 ""  对应每一个处理好的插值计算结果
 * $2  ""

```javascript
var a = 5;
var b = 10;

function tag(strings, ...values) {
  console.log(strings[0]); // "Hello "
  console.log(strings[1]); // " world "
  console.log(strings[2]); // ""
  console.log(values[0]);  // 15
  console.log(values[1]);  // 50

  return "Bazinga!";
}

let str = tag`Hello ${ a + b } world ${ a * b}`;
// "Bazinga!"

```

> 原始字符串

```javascript
function tag(strings, ...values) {
  console.log(strings.raw[0]); 
  // "string text line 1 \\n string text line 2"
}

tag`string text line 1 \n string text line 2`;
```
在标签函数的第一个参数中，存在一个特殊的属性raw ，我们可以通过它来访问模板字符串的原始字符串。

## 标签函数的应用

> 利用标签函数来实现模板引擎

> 过滤掉多行字符串中的空格