---
title: webpack2 经验教程
date: 2017-02-26 09:17:44
tags:
---

webpack2的文档好了很多，常用配置都有，所以熟读官方文档是最好的学习方式。 [传送门](https://webpack.js.org/configuration/)

![](http://o99eh3ii0.bkt.clouddn.com/FvBo_6LhVku9AzB0_UPAOpD4fIdc)

这里介绍一下官方文档内容分布。

* CONCEPTS : 一些常用概念的介绍
* **GUIDES** : 打包能力介绍，以及涉及的配置
* DOCUMENTATION : 从不同维度详细介绍如何配置webpack

所以看文档的顺序是，先确定你要做什么，它是属于什么概念范围的配置，然后去GUIDES中寻找，然后去DOCUMENTATION中寻找详细的配置
**GUIDES 是webpack的配置能力精华，是总纲。应该要最熟悉**


## 配置解析


## code-spilit 

> [异步模块加载](https://webpack.js.org/api/module-methods/#import-)

在webpack2中，可以使用es6的import实现异步加载模块，它可以返回promise

```javascript
import(
  /* webpackChunkName: "my-chunk-name" */
  /* webpackMode: "lazy" */
  'module'
).then((module)=>{
    let a = new module;
});
```

import支持魔法注释，通过注释，可以配置如何加载模块。

* webpackChunkName : 指定要打包进的chunk名
* webpackMode : 打包的模式

webpackMode 
* lazy  // default , 会为**每个异步模块**，单独打包。如果指定了相同的chunkName,会在后面带上数字区分
* **lazy-one** // 只打包1次，所有的模块会被打包进相同的chunk, 引入时只会请求一次
* eager  // 不生成额外的chunk，会被合并到当前所在chunk
* weak // 会重复加载，已经加载过的模块

注意:

* import是支持动态加载路径模块的，`import(foo)`,但如果路径是完全的变量是不支持的，因为webpack不知道这个文件在哪儿。import(`./locale/${language}.json`),**webpack是可以运行时确认要加载的模块**，在打包时会把这个路径下的所有模块都打包。


> commonChunks

这个插件可以提取多个入口的公共模块代码，如果有hash的话，要注意再运行一次提取出公共 模块加载代码，否则每次内容改变，hash改变，重复代码又会提取一次。

```javascript
// split vendor js into its own file
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      minChunks: function (module, count) {
        // any required modules inside node_modules are extracted to vendor
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          (module.resource.indexOf( path.join(__dirname, '../node_modules')) === 0 ||
           module.resource.indexOf( path.join(__dirname, '../src/common/vendor')) === 0)
        )
      }
    }),
    // extract webpack runtime and module manifest to its own file in order to
    // prevent vendor hash from being updated whenever app bundle is updated
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      chunks: ['vendor']
    }),
```


> tree shaking


## 非npm的三方模块引入

> import-loader

如果第三方模块依赖window作为this，则用此模式

> exports-loader

如果第三方模块导出的是全局变量，可以用此模式，比如jQuery

> script-loader

如果第三方模块比较老旧，可以用这中方式，在全局作用域执行
