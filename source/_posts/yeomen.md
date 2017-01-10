---
title: 用Yeomen搭建你的手脚架工具
date: 2016-09-12 13:27:54
tags:
---


## 简介

[Yeoman](http://yeoman.io/) 可以帮助你快速开始一个新项目，你可以预先指定一些最佳实践，和实用工具来帮助你提高效率。它是一个生成器。
同时，它提供了一个良好的插件生态环境，开发者们能共享资源。开箱即用，一个generator能完整的生成一个工程或者是一个功能。

## 基本示例

> 目录结构

```
generator-baseconfig
├── generators
│   └── app
│       ├── index.js
│       └── templatesl
│           ├── dir
│           ├── .ackrc
│           ├── .eslintrc.json
│           ├── .gitignore
│           └── package.json
└── package.json
```

yeoman-generator 需要加入package.json npm依赖

```
{
  "name": "generator-arch-karmaTest",
  "version": "0.0.4",
  "description": "karma单元测试环境",
  "repository": {
    "type": "git",
    "url": "git@github.com:Alan110/generator-arch-karmaTest.git"
  },
  "files": [
    "generators/app"
  ],
  "homepage": "git@github.com:Alan110/generator-arch-karmaTest",
  "keywords": [
    "yeoman-generator",
    "karmaTest"
  ],
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Alan110",
  "license": "MIT",
  "devDependencies": {
    "chalk": "^1.1.3",
    "yeoman-generator": "^0.24.1",
    "yosay": "^1.2.0"
  }
}
```

> 基本代码

app/index.js

```javascript
var yeoman = require('yeoman-generator');

// 继承yeomen生成器基类
var baseconfig = yeoman.Base.extend({
    method1 : function(){
        this.directory('data', 'data');				 //拷贝目录
        this.copy('package.json', 'package.json');   //拷贝文件
        this.log('test');
    }
});

module.exports = baseconfig;

```

注意点 :

* 项目目录名必须是generator-****
* 生成文件要放在app/templates目录下
* 自行拷贝需要的文件/目录，执行yo name 的时候便会生成

> 本地运行

在generator-** 目录下执行npm link 链接到全局模块，然后新建一个文件夹执行yo name

`npm link`
`cd newdir && yo name`

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-12/23544887.jpg)

> 调试

如果生成失败，可以执行yeomen自带的检测工具，根据提示修改环境问题

`yo doctoer`

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-12/93505212.jpg)

## 生命周期

> [run loop](http://yeoman.io/authoring/running-context.html)

可以理解每一个方法就是一个task，按顺序执行。
当执行yo example启动生成器时，它会沿着它的生命周期执行如下特定名称的函数，这些特定名称的函数会放进一个队列里面按顺序执行，如果功能函数不是特定的函数名称，如上面的method1，则放进另一个队列default按顺序执行。这些特定的函数名称有:

* initializing : 初始化阶段
* prompting : 接受用户输入阶段
* configuring : 保存配置信息和文件，如.editorconfig
* default : 非特定的功能函数名称，如上面说到的method1
* writing : 生成项目目录结构阶段
* conflicts : 统一处理冲突，如要生成的文件已经存在是否覆盖等处理
* install : 安装依赖阶段，如通过npm、bower
* end : 生成器即将结束

> 私有方法

使用下划线的方法不会加入run loop

```javascript
  generators.Base.extend({
    method1: function () {
      console.log('hey 1');
    },
    _private_method: function(){
      console.log('private hey');
    }
  });
```

继承父generators的方法不会加入run loop

```javascript
  var MyBase = generators.Base.extend({
    helper: function () {
      console.log('methods on the parent generator won\'t be called automatically');
    }
  });

  module.exports = MyBase.extend({
    exec: function () {
      this.helper();
    }
  });
```

> 异步task

某些情况需要阻塞run loop，拿到结果后继续执行

```javascript
asyncTask: function () {
  var done = this.async();

  getUserEmail(function (err, name) {
    done(err);
  });
}
```

## [修改已经存在的文件内容](http://yeoman.io/authoring/file-system.html)

```JavaScript
this.fs.copy(path, newPath, {
    process: function(content) {

        /* Any modification goes here. Note that contents is a Buffer object */

        var regEx = new RegExp('old string', 'g');
        var newContent = content.toString().replace(regEx, 'new string');
        return newContent;
    }
});
```

## [获取cli参数](http://yeoman.io/authoring/user-interactions.html)

yo baseconfig gulp

```javascript
    initializing : function(){
        this.argument('type', {
            type: String,
            required: false,  // 可选，不强制
            default: 'gulp'
        });
    },
	myStart: function() { 
        var type = this.options.type; // 获取参数
        this.log(type || '');

        if (['gulp', 'node'].indexOf(type) == -1) {
            this.log('没有指定或者非法参数，使用默认值 gulp')
            type = this.options.type = 'gulp';
        }

        this['_gen_' + type]();

    },

```

一定要在初始化的时候指定需要监听的参数，否则可能获取不到

______

如果是这种参数
yo baseconfig --coffee

```javascript
    initializing : function(){
		 this.option('coffee'); //设置参数
    },
	myStart: function() { 
		this.log(this.option('coffee')); //获取参数
    }
```

##  与用户交互

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-12/3780543.jpg)

> [Inquirer.js](https://github.com/SBoudrias/Inquirer.js#question)

yeomen只用了Inquirer.js(在命令行下与用户交互的库).
prompt 接受一个question object 数组,返回promise

```javascript
// useage
prompting: function () {
    return this.prompt([{
      type    : 'input',
      name    : 'name',
      message : 'Your project name',
      default : this.appname // Default to current folder name
      choices :
      validate :
      filter :
      when :
    }, 
    {
      type    : 'confirm',
      name    : 'cool',
      message : 'Would you like to enable the Cool feature?'
    }]).then(function (answers) {
      this.log('app name', answers.name);
      this.log('cool feature', answers.cool);
    }.bind(this));
}

```

## 和别的插件组合

## [加载依赖(npm , brower)](http://yeoman.io/authoring/dependencies.html)

在install生命周期执行 this.npmInstall, 可以加载npm包,这很方便动态添加依赖，多个生成插件之间的配合。

```javascript
    install : function () {
         this.npmInstall(['babel-preset-es2015-rollup'], { 'save-dev': true });
         this.npmInstall(['rollup-plugin-babel'], { 'save-dev': true });
         this.npmInstall(['rollup-plugin-replace'], { 'save-dev': true });
         this.npmInstall(['rollup-plugin-uglify'], { 'save-dev': true });
    },

```

## 插件

有一些美化输出的插件。

* [chalk](https://github.com/chalk/chalk)      //输出带颜色的文本
* [yosay](https://github.com/yeoman/yosay)     //输出一个图案

## 发布到npm

注册npmjs账号 [注册地址](https://www.npmjs.com/signup)

在npmjs上登陆
`npm config set registry http://registry.npmjs.org`
`npm adduser`

很多人用的是淘宝镜像，此时必须是在npm官网上登陆,否则publish的时候会报错
![](http://o99eh3ii0.bkt.clouddn.com//16-9-12/15863496.jpg)

发布
`npm publish .`

