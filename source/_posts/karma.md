---
title: 使用karma管理测试框架
date: 2016-08-30 21:19:44
tags:
---

## 简介

karma不是一个测试框架，也不是断言库，它只是开了一个http server，然后生成了一个包含测试代码，和测试框架的html页面。你可以使用任何你喜欢的测试框架。
karma是一个集成测试环境，管理我们的测试代码，测试依赖，让自动化测试变得简单。它能:
* 在真实browser下运行测试
* 同时在多个浏览器下运行测试
* watch测试文件变化，自动测试
* 终端执行测试
* 生成代码覆盖率报表
* 支持requirejs模块化测试

![](http://o99eh3ii0.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-08-31%20%E4%B8%8B%E5%8D%884.43.54.png)

## 安装

进入工程目录
```
sudo npm install karma --save-dev
sudo npm install -g karma-cli    
```

需要安装全局cli命令工具。否则只能通过`./node_modules/karma/bin/karma start` 运行


## 配置

```
karma init 
or karma init myconf.js // 命名配置文件
```

进入交互配置界面。选择需要的测试框架，测试文件路径等等。完成后默认会在根目录生成karma.conf.js。如果没有配好可以手动修改该配置文件

配置文件里面的一些属性还可以通过命令行的方式设置,比如:`--browsers Chrome`。

这里只说明**plugins**和**files**2个字段 详细配置选项可以看看[官方文档](https://karma-runner.github.io/1.0/config/configuration-file.html)

```javascript
files: [
    'lib/esl.js',
    'lib/jquery.js',
    {
        pattern: 'src/**/*.js',
        included: false
    },
    {
        pattern: 'test/**/*.js',
        included: false,
        watched : false,
        served : false
    },
],

```
files引入的文件，会以script标签的方式直接插入页面中。每一个元素是字符串，也可以是一个对象，进行详细配置。

* pattern  寻找所有满足条件的文件
* included 以`<script>`标签的形式插入文件(AMD下，需要设置为false) default: true
* watched  当autoWatch=true 时，修改文件会自动重新载入  default: true
* served   是否能被karma的webserver访问  default: true

```javascript
plugins:[
    'karma-jasmine',
    'karma-chrome-launcher'
]
```

karma默认会加载所有**devDependencies**字段里面，以karma-*开头的插件。也可以手动填写需要加载的插件。
如果写了此字段，就不会默认加载了。

### 添加测试plugin

karma 的插件都是npm包，需要加入依赖。此处列出常用的几种配置方案

> jasmine + chrome

```javascript
npm install karma-jasmine karma-chrome-launcher --save-dev


// karma.conf.js配置
frameworks: ['jasmine']
```

> mocha + chai  + chrome

```javascript
npm install karma mocha chai  karma-mocha karma-chai  karma-chrome-launcher --save-dev

// karma.conf.js配置
frameworks: ['mocha','chai'],
```

> requirejs

```javascript
npm install  requirejs karma-requirejs  --save-dev

// karma.conf.js配置
frameworks: ['requirejs'],   // add this to list

files: [
    'test/mocha/test-main.js',
    {
        pattern: 'output/*.js',
        included: false
    },
    {
        pattern: 'test/mocha/spec.js',
        included: false
    }
],
```

需要引入test-main.js。
注意由于要使用requirejs，那么所有的源文件和测试文件都要写成**AMD模块**,(以define包裹的模块)且file字段里面included设置为false，因为不需要注入了。通过requirejs载入。

test.main.js配置
```javascript
var allTestFiles = []
var TEST_REGEXP = /(spec|test)\.js$/i

// Get a list of all the test files to include
Object.keys(window.__karma__.files).forEach(function (file) {
  if (TEST_REGEXP.test(file)) {
    allTestFiles.push(file) //匹配到的文件加入到依赖中。
  }
})

require.config({
    
    baseUrl: '/base/output',

    //测试文件会根据路径直接加载
    deps: allTestFiles,

    callback: window.__karma__.start
    

})

```
test.main.js是用来配置requirejs的，其中baseUrl字段是设置查找模块的根路径. /base 相对于karma.conf.js的路径。


> 代码覆盖率

```javascript
npm install  karma-html-reporter karma-mocha-reporter karma-coverage  --save-dev

// karma.conf.js配置
preprocessors: {
    'src/**/*.js': ['coverage'] // specific files to generate coverage reporter
},
reporters: ['coverage'],   // add this to list
coverageReporter: {        // set dest of reporter
    dir: 'coverage/'
},
```

生成的html报表，可以在浏览器中打开，查看分支覆盖情况，函数覆盖情况

![](http://o99eh3ii0.bkt.clouddn.com//16-9-6/18392130.jpg)

点击每一个文件还能看到具体的是哪些语句没有执行到。我们要尽可能的减少红色的，黄色的尽可能覆盖,完善我们的测试用例

![](http://o99eh3ii0.bkt.clouddn.com//16-9-6/54203232.jpg)


## 运行

```
karma start
karma start my.conf.js
```
默认会读取karma.conf.js文件，你也可以指定配置文件名。


## debug

在命令行下执行

```
karma start karma.conf.js --browsers=Chrome --single-run=false --debug
```

打开新弹出的chrome进程，点击debug进入调试页面。与浏览器的调试工具无异

![](http://o99eh3ii0.bkt.clouddn.com//16-9-6/20303741.jpg)

需要注意的，debug模式下，需要去掉coverage覆盖率，不然调试时会有很多覆盖率的代码，造成干扰。

```javascript
preprocessors: {
    'src/**/*.js': ['coverage'] // 去掉coverage
}
```

或者我们检测命令行是否有`--debug`，有则去掉coverage
```javascript
// debug时，去掉覆盖率报表
var preprocess = {};
if (!(process.argv.join(',').indexOf('--debug') > -1)) {
    preprocess['output/*.js'] = 'coverage';
}

preprocessors: preprocess
```

