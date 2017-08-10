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

> 过滤非法字符串

```javascript
function safeHTML(strings, ...interpolatedValues) { // `...` essentially slices the arguments for us.
  return strings.reduce((total, current, index) => { // use an arrow function for brevity here
    total += current;
    if (interpolatedValues.hasOwnProperty(index)) {
      total += String(interpolatedValues[index]).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
    }
    return total;
  }, '');
}
```

> 过滤掉多行字符串中的空格

fdsfsd

> 简易模板引擎

原理是利用标签函数可以拿到模板字符串中的所有字符串片段和变量值，并且一一对应,所以我们可以**选择性的重新组装模板**,或者对变量进行处理。这里借助了symbol变量，作为block片段的分隔符

```javascript
var startBlockSentinel = Symbol('blockSentinel');
var ignoreBlockSentinel = Symbol('ignoreBlockSentinel');
var endBlockSentinel = Symbol('endBlockSentinel');

var helpers = {
    if: (condition, thenTemplate, elseTemplate = '') => {
        return condition ? startBlockSentinel : ignoreBlockSentinel;
    },
    end: () => {
        return endBlockSentinel;
    },
    unless: (condition, thenTemplate, elseTemplate) => {
        console.log(!condition)
        return !condition ? startBlockSentinel : ignoreBlockSentinel;
    },
    registerHelper(name, fn) {
        helpers[name] = fn;
    }
};

/**
 * 
 * 标签模板，修饰模板字符串的返回值
 * 拼接字符串，变量和字面字符是一一对应的
 * @param {any} strings  字面字符串数组
 * @param {any} interpolatedValues  替换变量数组
 * @returns 
 */
function template(strings, ...interpolatedValues) {
    // 条件块状态记录
    const statusStuck = [];

    return strings.reduce((total, current, index) => {

        // 拼接字符串, if为true, 或者不在if内
        if ((statusStuck.length > 0 && statusStuck[statusStuck.length - 1] == startBlockSentinel) || statusStuck.length == 0) {
            // +占位符之前的内容
            total += current;
        }

        // 更新当前状态
        if (interpolatedValues.hasOwnProperty(index)) {
            let value = interpolatedValues[index];
            if (value === startBlockSentinel || value === ignoreBlockSentinel) {
                statusStuck.push(value);
            } else if (value === endBlockSentinel) {
                statusStuck.pop();
            } else {
                // + 占位符变量，过滤非法字符,转义成了实体，不能有<div> 
                // 只在是true block状态时才添加，否则丢弃
                // total +=  String(interpolatedValues[index]).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
                if(statusStuck[statusStuck.length - 1] == startBlockSentinel){
                    total += String(interpolatedValues[index]);
                }
            }
        }

        return total;

    }, '');

}

export { template, helpers }
```
