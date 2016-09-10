---
title: 美化你的vim
date: 2016-09-11 01:03:12
tags:
---


## airline

![pic alt](https://github.com/vim-airline/vim-airline/wiki/screenshots/dark.png)



#### 使用powerline 符号字体

![](http://o99eh3ii0.bkt.clouddn.com//16-9-11/63571441.jpg)

> [安装powerline](https://powerline.readthedocs.io/en/latest/installation.html#pip-installation)

```
pip3 --user install powerline-status
```

> pip安装必须要用 —user属性，不然会很卡, 这里貌似是有bug

> 安装powerline字体
 [powerline-fonts下载](https://github.com/powerline/fonts)

cd ./fonts && ./install.sh

> 配置vimrc

let g:airline_powerline_fonts = 1

## 主题

#### gruvbox
切换model

其他主题配置使用方式请看[官方文档](https://github.com/morhetz/gruvbox/wiki/Usage)

切换airline主题


### 主题与airline配合
一般主题也会自带airline主题，所以切换一下就行了。

```
:AirlineTheme {theme-name}
```
