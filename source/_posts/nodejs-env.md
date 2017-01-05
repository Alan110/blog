---
title: nodejs 入门
date: 2016-12-03 15:55:33
type: "categories"
categories: "nodejs"
tags:
---

js 可以写前端，服务端，游戏，脚本，比较全面了。学了还是比较保值的。
nodejs很火，很牛叉，迭代非常迅速，现在都8.0了(我的天...) 。 为了不让自己累死，所以学着积累。能力比知识重要，掌握能力才是关键。

## 文档教程

看[官方文档](https://nodejs.org/api/debugger.html#debugger_information)是最好的。授人以鱼不如授人以渔，借用张孝祥的一句话，要学会看一手资料, 别看二手资料。

有mac的同学推荐使用Dash，包含各种软件，语言的文档。绝对神器。

新手教程就看看这篇吧[教程](http://www.runoob.com/nodejs/nodejs-tutorial.html) (哎。。又给人家导流了..)

## 编写环境与调试

文本编辑器 : 推荐(ST , vim , emacs, vscode)
IDE        : 推荐webstorm

### hot-node

`npm install -g hotnode`
`hotnode index.js`
当你修改程序后，会自动重新运行, 程序报错时，不会挂掉(写web程序时非常有用)

### repl-交互式命令debug

在代码中需要打断点的地方写上debugger, 也可以在debug模式运行起来后再设置断点。

#### 演示调试流程:


`node debug index.js` // run

`help` // to see the action you can do

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//16-12-31/9802197-file_1483181608362_9ad3.png ) 

`list(100) `// 查看前后100行代码

`sb(112)` // 浏览代码，在需要设置断点的地方设置断点

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//16-12-31/30114647-file_1483181750934_702.png ) 


`c  `  // 跳跃到最近的断点

`s  `  // step run

`n  `  // line run

`o  `  // out the function

`repl`  // 进入交互计算模式, 类似chrome的console，直接输入变量即可查看值

`ctrl + c`  // 退出repl

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//16-12-31/12879852-file_1483181933556_141dd.png ) 


#### 操作解释

| name            | describe                                          |
| : ------------- | :-------------                                    |
|run	|执行脚本,在第一行暂停|
|restart|	重新执行脚本|
|cont, c|	继续执行,直到遇到下一个断点|
|next, n|	单步执行|
|step, s|	单步执行并进入函数|
|out, o	|从函数中步出|
|setBreakpoint(), sb()|	当前行设置断点|
|setBreakpoint(‘f()’), sb(...)|	在函数f的第一行设置断点|
|setBreakpoint(‘script.js’, 20), sb(...)|	在 script.js 的第20行设置断点|
|clearBreakpoint, cb(...)|	清除所有断点|
|backtrace, bt|	显示当前的调用栈|
|list(5)|	显示当前执行到的前后5行代码|
|watch(expr)|	把表达式 expr 加入监视列表|
|unwatch(expr)|	把表达式 expr 从监视列表移除|
|watchers|	显示监视列表中所有的表达式和值|
|repl|	在当前上下文打开即时求值环境|
|kill|	终止当前执行的脚本|
|scripts|	显示当前已加载的所有脚本|
|version|	显示v8版本|

### v8 and chrome DevTool

v8可以支持远程调试, 有诸如node-inspector之类的工具，可以在chrome DevTool中调试。不过我将nodejs升级到6以后，使用node-inspector 报错了。后来翻了下文档，发现node自带工具可以直接chrome 调试打通

`node --debug-brk  --inspect build/build.js`

在chrome中打开生成的链接就可以了，与chrome调试无异。


## 参考

[nodejs documentiton](https://nodejs.org/api/debugger.html#debugger_information)
