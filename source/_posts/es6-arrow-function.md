---
title: 坑之箭头函数
date: 2017-01-18 11:53:47
tags:
---

## 坑点

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误, 因为没有this
（3）不可以使用arguments,caller对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
（4）不可用于prototype的method
（5）不可以使用yield命令，因此箭头函数不能用作Generator函数。

this,arguments,caller都没有，都是继承父function作用域的。

使用场景为优化回调，拉平层次。
