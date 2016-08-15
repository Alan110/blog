---
title: hexo个性化配置(进阶篇)
date: 2016-08-14 11:22:58
tags:
---


## 原理介绍

## 常用配置

> 需要注意的是，Hexo 3.2 版本不允许配置文件中存在重复的选项设置。 因此，最好检查下 站点配置文件 中是否有存在上述同名的配置。 如果存在，请将两者配置在一起。



### 设置菜单对应的中文

http://theme-next.iissnan.com/getting-started.html#menu-settings

### 如何设置页面文章的篇数？

> hexo可以设置首页,归档,tag的文章篇数,不过需要安装hexo的插件

```shell
npm install --save hexo-generator-index
npm install --save hexo-generator-archive
npm install --save hexo-generator-tag
```

在根目录的_config.yml中配置

```yml
index_generator:
  per_page: 5

archive_generator:
  per_page: 20
  yearly: true
  monthly: true

tag_generator:
  per_page: 10
```


## next主题自定义

* 看下官方文档

包括了

代码高亮
页面3种样式布局

* 优化动画效果

> 默认的动画太慢了,先是logo,然后是menu,postlist,sliderbar,依次动画执行.对于有强迫症的人来说,这个真心受不了.曾一度关闭了动画.设置`use_motion:false`. 后来想想还是觉得给页面增加一点灵气比较好.就有了修改主题代码的打算.

next与页面动画相关的代码是在/source/js/src/motion.js文件里. 乍一看,真是云里雾里,不知道如何下手.个人认为代码结构有些混乱,吐槽一下.平静一下心情,仔细找了找.发现了这2段代码

```javascript
      hasElement($subtitle) && sequence.push({
        e: $subtitle,
        p: {opacity: 1, top: 0},
        o: {duration: 200}
      });

      ...

      $.Velocity.RunSequence(sequence);


```

顿时发现与jquery的动画调用有些类似,无非是设置起点,终点属性,然后调用.只是不知作者是用的什么库.baidu了一下,原来是用的`Velocityjs`动画库.翻了一下API,明白了,这2句话是说,创建一个array,每一个元素是一个动画帧,其中有具体的动画描述属性.运行时会按顺序一个接着一个运行,说白了就是一个动画队列.

明白了核心代码,情况就明朗多了. 不过又发现一个让我迷糊的点.看看这段代码摘要,部分代码我已删除

```javascript
  NexT.motion.middleWares =  {
    logo: function (integrator) {
      var sequence = [];
      var $brand = $('.brand');
      ....
      if (sequence.length > 0) {
        sequence[sequence.length - 1].o.complete = function () {
          integrator.next();
        };
        $.Velocity.RunSequence(sequence);
      } else {
        integrator.next();
      }
    },

    menu: function (integrator) {
      $('.menu-item').velocity('transition.slideDownIn', {
        display: null,
        duration: 200,
        complete: function () {
          integrator.next();
        }
      });
    },

    postList: function (integrator) {
      function postMotion () {
        var postMotionOptions = window.postMotionOptions || {
            stagger: 100,
            drag: true
          };
        postMotionOptions.complete = function () {
          `integrator.next();`
        };

        $post.velocity('transition.slideDownIn', postMotionOptions);
      }
    },

    sidebar: function (integrator) {
      if (CONFIG.sidebar.display === 'always') {
        NexT.utils.displaySidebar();
      }
      integrator.next();
    }
  };

```

每个方法都调用了integrator.next()方法,而方法顺序是logo->menu->postlist->sidebar. 正好是动画的执行顺序. 作者的意图就不言而喻了, 是控制各部分动画依次进行.我不禁有个疑问,既然velocityjs是支持动画队列的,那干嘛还这么写..... 怀着疑问,突然眼前一亮.

```javascript
  NexT.motion.integrator = {
    queue: [],
    cursor: -1,
    add: function (fn) {
      this.queue.push(fn);
      return this;
    },
    next: function () {
      this.cursor++;
      var fn = this.queue[this.cursor];
      $.isFunction(fn) && fn(NexT.motion.integrator);
    },
    bootstrap: function () {
      this.next();
    }
  };
```

这便是一个简单的队列function依次执行的实现,估计作者是想自己练练手实现一个动画队列吧....

好了,一切都真相大白,拨开云雾见明月.要实现咱们的需求也很简单了. 把作者的动画队列代码删掉就行. 各个部分就能并行执行.

## 技巧分享

## 引用
