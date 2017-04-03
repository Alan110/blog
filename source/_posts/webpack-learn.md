---
title: webpack1.x 入门教学
date: 2017-02-22 19:21:55
tags:
---

![pic alt](http://webpackdoc.com/images/what-is-webpack.png "opt title")

webpack 是一个包含预编译/动态加载的**模块化方案**.  会将各种依赖的资源最后打包成1个或者几个文件, 适合单页应用

它与gulp，fis3是有本质区别的，它本质上是模块化方案对应requirejs之类的，主要能力是打包，只不过包含了部分构建的能力。它可以作为gulp中的task,负责打包工作。

webpack 可以支持AMD，CMD，UMD的打包, **请注意如果代码复杂度没有涉及到模块化，那么完全不需要用webpack，比如简单的运营页面，自己做好简单的模块化就行了。因为webpack的优势在这里根本用不上** , 这种情况推荐用gulp。

**单入口的情况下，不管重复require多少次一个模块，都只会打包一次。**
**多入口实际就是分别执行多个单入口，彼此之间不影响, 如果都引入同一个模块，会被引入多次**

CommonsChunkPlugin解决了这个问题, 就是把多个入口共同的依赖都给定义成 一个新入口

## 核心概念解析

> loader

**webpack本身只能处理commonjs规范的js文件**，如果要处理其他文件，就要使用相应的loader进行转换。
是针对于资源文件转换成commonjs文件，可以理解为预处理转换器
loader 可以通过!传递流，通过?传递参数，或者单独配置query参数

> plugn

plugn是扩展webpack本身的能力 , 是在整个构建过程中都生效的，不要和loader搞混淆了。

> chunkId/mouleId

webpack的id 有两种 一种为 chunkid 一种为moduleId
每个chunkid 对应的是一个js文件
每个moduleid对应的是一个内容模块，一个js文件可以包含多个内容模块
为何要出来一个chunkid呢？ 这个chunkid的作用就是，标记这个js文件是否已经加载过了

> HMR

热插拔, 其本质上是一个插件, 作用是修改后自动刷新浏览器。 它有2种启动方式

CLI (推荐)

`webpack-dev-server --hot --inline`

cfg

![](http://p1.bqimg.com/567571/f4129450f02c59ce.png)

## [配置文件接口](http://webpack.github.io/docs/configuration.html#output-chunkfilename) / [官方插件接口](http://webpack.github.io/docs/list-of-plugins.html)

配置写在webpack.conf.js, 可以在官网查询到所有的配置项

![pic alt](http://p1.bpimg.com/567571/af938cd7c72d71f7.png "opt title")

> 异步加载模块

```javascript
require.ensure([],function(){

})
```

这是编译后的代码
![pic alt](http://i1.piimg.com/567571/d62cf7c9f9a246d1.png "opt title")

## 常见配置解析

```javascript

var webpack = require("webpack");  // 包含一些自带的插件，使用时需要引入
var HtmlwebpackPlugin = require('html-webpack-plugin');  // 第三方插件需要安装，并引入
var OpenBrowserPlugin = require('open-browser-webpack-plugin');

var TEM_PATH = path.resolve(ROOT_PATH, 'templates'); // 模板文件路径


module.exports = {

  entry: { // 可以有多个入口
	'main' : 'main.js',
	'main2' : 'main2.js',
	'vender' : ['jqeury', 'vue']
  }, 

  output: {
    path : path.resolve(__dirname,'dist'), // 打包后资源的输出路径
    filename: 'bundle.js', // 输出单文件，如果是多入口按文件名分文件打包则，顺带添加hash，[name].[hash].js , 如果不这么写，由于输出名一样，文件会被覆盖
    publicPath :  '' // 替换资源url，src的时候的路径，线上访问的时候，比如访问cdn文件的时候
    pathinfo : true // 添加模块的注释信息,配合eval使用
  },

  module: {
    loaders:[ // 针对资源文件
      // npm install --save-dev babel-loader babel-core babel-preset-es2015  babel-preset-react  编译jsx
      { test: /\.js[x]?$/, exclude: /node_modules/, loader: 'babel-loader?presets[]=es2015&presets[]=react' }, // 简写  使用es6
      { test: /\.css$/, loader: 'style-loader!css-loader?modules|post-loader' }, // 将css文件插入dom
      { // 可读性好的写法
        test: /\.png$/,
        loader: "url-loader",  // 将图片转换成base64
        query: { mimetype: "image/png" }
      }
    ]
  },

  resolve : {   

    extensions: ['.js', '.vue', '.json'], // 设置需要识别的扩展文件名 ，设置了此项后会覆盖默认的配置

    modules: [   // 类似nodejs的引用方式，当require一个模块，可以直接写相对路径
	    resolve('src'),
	    resolve('node_modules')
    ],


	alias : {  // 设置路径别名
		lib : '/util/head/lib'
	}
  },

  externals: { // 遇到require这些时, 不需要再编译. 适合那些常用的库, 已经在页面通过<script>引入了, 就无需都打包到一起了 , 全局变量申明
      jquery: 'jQuery',
  },

  postcss: [
      require('autoprefixer')
  ],

  devtool : 'eval' ,  // 有7中模式，eval速度最快, 建议开发的时候使用

  plugins: [ // 扩展webpack能力

    new HtmlwebpackPlugin({   // 生产html文件，自动插入构建完成的bundle文件
        title: 'Hello World app',
        template: path.resolve(TEM_PATH, 'index.html'),
        filename: 'index.html',
        chunks: ['app', 'vendors'],  //chunks这个参数告诉插件要引用entry里面的哪几个入口
        inject: 'body' //要把script插入到标签里
    }),

    new OpenBrowserPlugin({
      url: 'http://localhost:8080'
    }),

    new webpack.optimize.UglifyJsPlugin({       // 混淆代码
      compress: {
        warnings: false
      }
    }),

    new webpack.ProvidePlugin({ // 让模块变成全局模块,不用每次都require, 只要发现有引用就会当成依赖传入
      $: "jquery",
      jQuery: "jquery",
      "window.jQuery": "jquery"
    })

    new webpack.optimize.CommonsChunkPlugin({  // 提取公共部分为trunk文件,  所以在浏览器中加载的话，必须先加载 common.js names: ['vendor'], // 成为一个新的入口 filename: "commons.js", // the filename of the commons chunk
        trunks : ['main','main2'] // 指定对那些入口进行提取，默认是对所有入口
    }),

    new webpack.HotModuleReplacementPlugin(),   // 热插拔, 修改后自动刷新浏览器，有配置文件，和cli2中方式
    new webpack.NoErrorsPlugin(), //在打包时不会因为错误而中断

    new webpack.DefinePlugin({ // 可以定义编译时的全局变量，有很多库（React, Vue等）会根据 NODE_ENV 这个变量来判断当前环境。为了尽可能减少包大小，在生产环境中要定义当前环境变量
        __DEV__: JSON.stringify(JSON.parse(process.env.DEBUG || 'false'))
    });

    /*optimize.OccurrenceOrderPlugin: 可以减少文件大小

    optimize.DedupePlugin: 可以减少重复文件数

    ExtractTextPlugin: 可以将所有css文件打包到一个css文件中*/
  ]

};

```

## 开发/调试技巧

`进度和颜色`
当项目逐渐变大，webpack 的编译时间会变长，可以通过参数让编译的输出内容带有进度和颜色。
webpack --progress --colors

`监听增量编译`
webpack --progress --colors --watch

> 调试服务器[webpack-dev-server](http://webpack.github.io/docs/webpack-dev-server.html)

启动一个 express 静态资源 web 服务器，并且会以监听模式自动运行 webpack
webpack-dev-server --progress --colors

* 用webpack-dev-server生成bundle.js文件是在内存中的，并没有实际生成；

* 如果引用的文件夹中已经有bundle.js就不会自动刷新了，你需要先把bundle.js文件手动删除（后期有插件可以完成）

* webpack-dev-server --content-base build/   指定server的根目录，不指定则为当前目录

* --devtool eval：为你的代码创建source map。当有任何报错的时候可以让你更加精确地定位到文件和行号

* 如果webpack使用的1.x的版本，那么webpack-dev-server也要使用1.x的版本，否则会报如下错误：Connot find module 'webpack/bin/config-yargs'。

* 如果已经有一个工程中使用了webpack-dev-server，并且在运行中，没有关掉的话，那么8080端口就被占用了，此时如果在另一个工程中使用webpack-dev-server就会报错：Error: listen EADDRINUSE 127.0.0.1:8080。*

> [sourceMap](http://sanwen.net/a/ltjldpo.html)

webpack 支持7中sourceMap方式

模式    解释
eval    每个module会封装到 eval 里包裹起来执行，并且会在末尾追加注释 //@ sourceURL.
source-map  生成一个SourceMap文件.
hidden-source-map   和 source-map 一样，但不会在 bundle 末尾追加注释.
inline-source-map   生成一个 DataUrl 形式的 SourceMap 文件.
eval-source-map 每个module会通过eval()来执行，并且生成一个DataUrl形式的SourceMap.
cheap-source-map    生成一个没有列信息（column-mappings）的SourceMaps文件，不包含loader的 sourcemap（譬如 babel 的 sourcemap）
cheap-module-source-map 生成一个没有列信息（column-mappings）的SourceMaps文件，同时 loader 的 sourcemap 也被简化为只包含对应行的。

`排查错误`

webpack --display-error-details

`HMR` 热替换
webpack-dev-server --hot --inline

这个在跨平台开发或者较复杂的项目中特别有用，比如我有个层级很深的操作，操作了10多次才进入这个界面，这个时候更改了一个小功能，如果没有热替换，只能刷新整个页面，再重复操作10多次才能看到效果，热替换改变了这一切

`使用绝对路径`
Webpack 中涉及路径配置最好使用绝对路径，建议通过 
`path.resolve(__dirname, "app/folder")` 或 
`path.join(__dirname, "app", "folder")` 的方式来配置，以兼容 Windows 环境。



## 引用

[阮一峰webpack入门项目](https://github.com/ruanyf/webpack-demos#demo01-entry-file-source )
[CommonsChunkPlugin解析](http://cnodejs.org/topic/5867bb575eac96bb04d3e301)
