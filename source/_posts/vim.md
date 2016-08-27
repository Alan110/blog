---
title: 极致编程，我最爱的vim
date: 2016-08-16 14:44:50
type: "categories"
categories: "工具"
tags:
---

## 前言

vim是有毒的!

vim是有毒的!

vim是有毒的!

vim的定位,它只是一个编辑器.适合文件修改与小工程的开发.如果希望它变得像ide一样,那需要很多的工具配合.不会做就不会死,对它偏爱的人偏偏要作!,而且作得极致.就像咱们中国是走的中国特色社会主义道路一样，vim也应该有它属于每个人独特的道路.

你可以让它大而全，也可以让他小而精美。vim是一块精石,需要他的主人不停的对它打磨，才能变成宝玉。没有毅力之人，或是对它的爱不够深切，都将无法令之完全绽放光芒。本人不才,我用了半年的时间来适应它的按键布局,它的模式理念.又花了一年学着怎样怎样给他装插件,怎样高效的用它编辑,怎么让它融入到我的指尖.其中无数的爱与恨伴我度过了多少日夜,敲过了多少代码.好几回我都想离她而去,然而用别的编辑器一段时间,我又无法忘怀她的一切,琢磨着怎样让她焕发新生.于是又毅然回归.或许你会说我傻,可是她真的融入到了我的工作生活,我戒不掉了.

前期使用vim有一个要点是，要动脑子。完成一个操作，有很多种方式，要选择最合适方式才最高效。这往往很不容易，因为还要在写作的过程中动脑子想用什么组合快捷键，是很让人烦的，往往最后就演变成不停的jjjj,kkkk，效率奇低，最后内伤而逃。但如果你坚持下来了，把这些best pricite练成了身体记忆，你也就学会了vim这套武功秘籍。

我为什么要学vim？

* 它真的能让我装逼
* 它给我足够的空间去创造。
* 一劳永逸，再也不需要学别的编辑器了。

## 入门

### 安装

>homebrew 安装

```shell
brew install vim  --with-lua --with-python
brew install macvim  --with-lua --with-python   // gui版
```

>源码安装

为什么要源码安装？
这样才能保证是最新的，而且能够保证对各种特性插件的支持，比如一些对lua，python依赖的插件。

下载源码
[https://github.com/vim/vim](https://github.com/vim/vim)
可以看下各个环境的安装说明，src/INSTALL文件

配置编译选项

```
./configure --with-features=huge --enable-rubyinterp --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config-i386-linux-gnu/ --enable-perlinterp  --enable-cscope --enable-luainterp --enable-perlinterp --enable-multibyte --prefix=/usr/local/bin/
```

* --with-features=huge：支持最大特性
* --enable-rubyinterp：启用Vim对ruby编写的插件的支持
* --enable-pythoninterp：启用Vim对python编写的插件的支持
* --enable-luainterp：启用Vim对lua编写的插件的支持
* --enable-perlinterp：启用Vim对perl编写的插件的支持
* --enable-multibyte：多字节支持 可以在Vim中输入中文
* --enable-cscope：Vim对cscope支持
* --enable-gui=gtk2：gtk2支持,也可以使用gnome，表示生成gvim
* --with-python-config-dir=/usr/lib/python2.7/config-i386-linux-gnu/ 指定 python 路径
* --prefix=/usr/local/bin/：编译安装路径


编译

```
sudo make && make install
```

### 基本操作

这部分教程网上多如牛毛，我也就不多说了，同时vim的快捷键多如牛毛，也不好一一说明。
在网上淘了如下几种入门方式。总之学会看文档是关键.

> 自带的vimtutor

> 游戏[vim大冒险](http://vim-adventures.com/)

![](http://feihu.me/img/posts/vim-adventures.jpg)

> vim速记图

![](http://o99eh3ii0.bkt.clouddn.com/vim-cmd.png)

> vim自带文档说明

```
:help       // 完整文档
:h buffer   // 查看某一部分的文档
```

看不惯英文可以去装中文版

### 常用特性

#### text object

>:help text-objects

* <action>i<object> 作用于对象内部，不包括边界
* <action>a<object> 作用于整个对象，括边界

```

ci'   改变'中的内容
ca'   改变'中的内容（包括"'"号）
di[   删除[中的内容
da[   ...
>i{   缩进{中的代码
>a{   ...

```

#### buffer

#### tab

## 进阶

怎样才算是一个好的编辑器？我认为有3个维度:

* 快速移动
* 高效编辑
* 扩展定制

为什么键盘比鼠标效率高？因为在熟练的情况下，它更准确，更少的误操作。身体记忆，无需思考。vim更是将键盘的优势极度放大，设置了3种模式，各司其职，快捷键无数。就像是一个军火库，什么装备都有，不过你非要拿大炮去打一个小毛贼，费时费力，大材小用,我也无话可说。这考验的是你的经验和那一点灵气。

### 快速移动

能精确而快速的定位到你要编辑的地方，这是用好vim的一个诀窍。

就像前面说的，很多人可能只会用j，k，h，l几个最基础的方向键，无疑编辑体验是痛苦的。这些人并没有学到vim的精髓。

#### 多种插入方式

```
    A   // 当前行后插入
    I   // 当前行前插入
    o   // 下一行插入
    O   // 上一行插入
```

#### 横向行内搜索

```
    f{字母} //  横向搜索第一个出现的字母, 此时按; ，搜寻下一个该字母
```

这个是很常用的，快速定位到行内的某个字母.

#### 全文搜索

#### 翻页

```
    ctrl + d    // 光标向后翻半屏
    ctrl + e    // 屏幕向下翻一行
    ctrl + f    // 光标向后翻一屏
    ctrl + b    // 光标向前翻一屏

    H   //  将光标置于最上面
    L   //  将光标置于最下面
    M   //  将光标置于中间面
    zt  //  将光标当前行置为屏幕顶端
    zb  //  将光标当前行置为屏幕底端
    zz  //  将光标当前行置为屏幕中间
```

翻页在我们浏览的时候是很常用的，ctrl + f/b 会使得光标和屏幕都移动，不好预测结果会在哪里，体验糟糕。推荐使用zt系列的方式翻页，我们可以预测翻页后的结果，体验好很多，也不用频繁按键。

#### 标记

#### 鼠标模式

#### 插件easyMotion

### 高效编辑


### 主题美化

* 推荐主题

* 光标美化


### 最佳实践

* 减少normal，insert模式之间的切换

* 善用重复功能键

### 自定义配置

* 插件安装

bundle
