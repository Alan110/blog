---
title: fis3 入门解析
date: 2016-08-18 23:28:55
tags:
---


fis3虽然入门简单，可是真的要上手，还是有点难度的。源于有些配置含混不清。

理解fis3的一些概念是很有帮助的



## 打包

fis3尽可能不会改变源码，它会把源文件目录，原样输出。这就有个问题，即所有文件都是有输出的。正常情况下，这没有什么问题，而且节省很多的路径配置工作，但是在做SPA，需要合并输出的场景下，这个就显得很恶心了。fis3可以打包合并。但是不是覆盖源文件，而且新增一个副本。

你可能会想讲打包的文件设置为release:false，不让它输出就行了。然而实际情况是，不输出便无法打包。

## 忽略打包过的文件

fis3-skip-packed    这个插件是可以的，可以去看看怎么实现的。

```javascript
fis.media('dev').match('**', {
  deploy: [
    fis.plugin('skip-packed', {
      // 配置项
      //'skipPackedToAIO' : false,
//      'skipPackedToPkg' : false
    }),

    fis.plugin('local-deliver', {
      to: '/Users/alan/.fis3-tmp/www'
    })
  ]
})
```

注意，配置了deploy选项，就会覆盖默认的deploy路径，默认是发布到本地server的路径，所以这里要手动设置一下


## 模块化

fis3默认不提供模块化框架，它所谓的模块化方案是说你以模块化(requirejs ,commonjs)的语法来写模块, fis会根据文件路径重新定义模块名, 然而我觉得这个很鸡肋啊，我们本来就不提倡命名模块，推荐模块名由实际服务器端文件路径决定。fis这么做感觉有点多此一举。

异步模块化方案有一个相对路径的概念,都是baseUrl + path ，模块引用使用相对路径。这样放到服务器上就能访问。

fis的inline功能很好用，但是不推荐用来做模块化,要限制它的使用。我认为它的应用场景应该只在html文件中，嵌入css，js源码。其他场景都应该由语言自身的模块化能力来解决


### moduleId 的注意点

moduleId的名字是没有.js后缀的，千万注意，否则会require不到。release的文件名是可以没有.js后缀的，会自动加上

```

fis.media('dev').match("/src/(*).js",{
    release : "$1",
    isMod : true,
    moduleId : '$1'
});

```

### 自定义配置

可以使用media来设置不同的发布配置，
使用环境变量来做同media下的细分配置
```
var process = require('process');
if(process.env.fisv == 'xx'){
    fis.media('dev').match('*.js',{
        1...
    });
}else{
    fis.media('dev').match('*.js',{
        2...
    });
}


调用

fisv=xx fis3 relase dev

fisv=xxx 只会在当次命令下生效

```

注意，process.env.fisv 只能在外层fis语法里面拿到，在内层函数里面是拿不到的。比如在:
```
fis.match('*.js',{
    preprocessor:function(){
       process.env.fisv  // 拿不到 
    }
})
```

fis默认对文件有编译换成，如果文件没有改变，下一次编译会直接走缓存，使用环境变量的方式，可能不同次编译走的fis配置语句不同，所以要去掉文件的缓存
```
fis.match('*.js',{
    useCatche : false
})
```


## 构件流程
