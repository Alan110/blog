---
title: JavaScript的类型转换
date: 2017-01-18 16:19:56
tags:
---

JavaScript，不同数据类型在不同运算符下会做不同的类型转换，这是坑点之一也是面试经常考的一个问题。

## valueOf与toString方法

要理解在类型转换的时候底层做了什么事情，就不得不说valueOf与toString方法了。
**valueOf的意思是返回最适合该对象类型的原始值，而toString则是将在该对象类型的原始值以字符串形式返回。**

> valueOf

| 对象            | 返回值p                                                    |
| : ------------- | :-------------                                             |
| 对象            | 返回值                                                     |
| Array           | 数组实例对象                                               |
| Boolean         | 布尔值                                                     |
| Date            | 以毫秒数存储的时间值，从 UTC 1970 年 1 月 1 日午夜开始计算 |
| Function        | 函数本身                                                   |
| Number          | 数字值                                                     |
| Object          | 对象本身。这是默认设置                                     |
| String          | 字符串值                                                   |

> toString

| 类型            | 行为描述|
| : ------------- | :-------------                                             |
| Array           | **将 Array 的每个元素转换为字符串，并将它们依次连接起来，两个元素之间用英文逗号作为分隔符进行拼接。**|
| Boolean         | 如果布尔值是true，则返回"true"。否则返回"false"。|
| Date            | 返回日期的文本表示。|
| Error           | 返回一个包含相关错误信息的字符串。|
| Function        | **返回如下格式的字符串，其中 functionname 是一个函数的名称，此函数的 toString 方法被调用： "function functionname() { [native code] }" **|
| Number          | 返回数值的字符串表示。还可返回以指定进制表示的字符串，请参考Number.toString()。|
| String          | 返回 String 对象的值。|
| Object(默认)    | 返回"[object ObjectName]"，其中 ObjectName 是对象类型的名称。|


## [隐式类型转换规则](http://www.cnblogs.com/wangmeijian/p/4639112.html)

![pic alt](http://o99eh3ii0.bkt.clouddn.com//17-1-19/91445201-file_1484798327287_1767e.png "opt title")

原始值的转换就是原始值，**主要要关注的是复合对象转原始值**。会先调用valueOf(),如果返回值不是原始值，再调用toString，再转成对应的类型

> `+`

1. 如果其中一个操作数是对象，对象会转换成原始值：日期对象通过toString()方法转换，其他对象通过valueOf()方法转换，如果valueOf()返回值不是原始值再使用toString()方法转换。
2. 在进行了对象到原始值的转换后，如果其中一个操作数是字符串的话，另一个操作数也会转换为字符串，然后进行字符串拼接。
3. 否则，两个操作数都将转换为数字（转换不了的将转换为NaN），然后进行加法操作。

```javascript

1 + true       // true转换为1,然后加法得出结果2
"1" + true     // true转换为"true"，然后字符串拼接得出结果"1true"
1 + "1"        // 数字1转换为"1"，然后字符串拼接得出结果"11"
{} + 1         // {}对象调用toString()方法转换为字符串"[object Object]"，变成了"[object Object]" + 1，匹配第二条规则，1将转换为字符串"1"，然后字符串拼接得出结果"[object Object]1"
{} + {}        // 自己想想过程吧

```

> `- * / ==`

会把左右两边转成数字，转不了的转成NaN, 复合类型会先调用valueOf -> toString -> number


##  [NaN](http://www.cnblogs.com/onepixel/p/5281796.html)

它是number类型的一个特殊状态,不是一个确定的值。 typeof NaN == number

```javascript
NaN == NaN    // false
```

所以它自己不等于自己。

