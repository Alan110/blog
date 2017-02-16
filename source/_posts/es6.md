---
title: es6调研报告
date: 2016-12-29 15:53:49
type: "categories"
categories: "JavaScript"
tags:
---

es6(ECMAScript 6) 下一代JavaScript标准语法,它是JavaScript的血统重构，也是前端混乱丛生的语法糖规范的大一统。或许某一天我们就不再需要各种`弥补`js缺陷的框架，js已经足够强大去处理各种问题。这一天应该不远了。我们需要重新认识JavaScript，就从es6开始。

目前主流的项目都用es6来做了，这是趋势，个人认为其中一个比较重要的内因是，js能做的事情越来越强，需求越来越多，语言能力必须得跟上。构建大型js软件需要言语层面的支持。随着es6的普及，很多辅助的类库都会被弃用了，js会更加成熟。

我是看了vue2.0的项目后，决定好好学习下es6，受限于公司技术栈，之前并没有使用过，有种es6还很遥远的错觉, 其实它已经泛滥了！我更被敲响了一个警钟，多看看当前最火的技术项目，你才不会掉队。

## es6目前的整体情况

| 平台            | 支持情况
| : ------------- | :-------------                                    |
|Androia  |差得远|
|ios 10| 100%支持|
|ios 9| 50%支持|
|chrome，Safari，Edge，Firefox，Opera| 差不多97%|
|nodejs，6.5  以上|差不多支持了97%的新特性。|

**总体看Safari是对es6支持最好的浏览器,支持率100%。**
**其中支持module特性的只有Safari Technology Preview 21 及其以上和Edge最新版，nodejs 6也不支持import，只能用require。**

![pic alt](http://o99eh3ii0.bkt.clouddn.com//17-2-5/33253591-file_1486288762048_15985.png "opt title")

详细的支持情况可以参考这个表 [es6各平台兼容表](http://kangax.github.io/compat-table/es6/)

## es6书写环境

写es6的方法有很多，最终我选择 rollup + babel + eslint。最核心的考虑点是基于es6的module系统和class, 这2个特性是构建复杂，大型js应用的关键所在。 我们能做的是尽可能的贴近标准规范。没用webpack除了个原因外，还因为rollup可以只打包用到的函数，方法。

浏览器端大多数特性都不支持，所以还是需要babel这样的转换器转换成es5的代码。

构建一个项目的底层并不容易，要考虑项目的实际情况，目录结构规范，代码规范，构建流程。rollup比较适合写js库，如果是写应用页面，那么就不合适，还需要考虑到项目的seo优化,rollup没有这方面的能力，需要结合其他的构建工具。


## rollup cli

需要重新学习一套rollup的配置流，上手快(当然得看个人), 坑少，官方构建工具。当需要输出多个版本,或者其他自定义构建的时候，显得不那么灵活。

## gulp-better-rollup

基于rollup js API的封装，使之与gulp的工作流结合，与上一个的区别是，gulp的社区广泛，大多数人最熟的，有人写插件。上手快，但是有很多隐藏的坑，这不排第一的gulp-rollup就挺坑的，有很多重叠的配置。因为已经是二次封装了。熟悉npm的API的同时，为了避免坑还要了解rollup 自己的底层API。好在社区广泛，大多数工具可以重复利用，有积累价值。

babel插件要使用rollup自己的，需要在单文件的时候就进行转换。其他的比如sourcemap, replace等可以用gulp的工具流, 此babel插件的配置可以集成到rollup的配置里面，不需要另外写一个配置文件。

多版本输出，可以输出不同format的bundle，如果是不同工作流，就再写一个task吧，最开始我还想抽象一下成为配置文件，后来发现其实没必要，gulp的api已经是可以配置+简易编程的了，一个task其实就是一个配置文件，也不在乎多写那几行代码，拷贝就行，就是对强迫症有点难受(卧槽。。我真的是强迫症。。)。

## rollup js API

最底层的API，最灵活，坑少。同时成本也很高。如果不用gulp，就需要自己实现流程控制.


总的来看 js API是效果最好的，当然要求也高, 必须要对这个东西有谱，查问题的时候还是得看1手资料。其次还是建议使用gulp-rollup, 这肯定是大多数人使用的版本，对行情的了解是有帮助的, 初看可能受益没那么高，但是是有积累价值的。


## yeomen 工程包

写了一个npm包，集成了es6 + rollup + gulp + eslint. 基本构成es6书写环境。 欢迎下载试用

```javascript
## 依赖就全局安装吧，反正后续也会用
npm install -g yeoman-generator
npm install -g babel
npm install-g babel-cli
npm install -g rollup

npm install -g generator-baseconfig

## 生成
yo baseconfig
```

## es6教程

我并没有刻意的去学es6，不过是找以前的写的代码，然后看一个es6特性，就套一个特性。帮助我加深印象，也更加实际的体会新语法的特点是什么。
语法可以参考 阮一峰大神的。 http://es6.ruanyifeng.com/

## 结语

年底了，拖了好久才开始写这篇文章，去年定得目标是赶上前端技术潮流，稀里糊涂的折腾了一年，前端技术进步得也不多啊。。。到是一天发神经折腾编辑器(你确定不是本末倒置？)。哈哈，随便吧，想到哪儿就写到哪儿吧。想2017许个愿，玛德我还真没什么愿望。就身体健康，万事如意吧。
