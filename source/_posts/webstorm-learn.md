---
title: webstorm-learn
date: 2017-03-13 13:35:32
tags:
---


## 未经授权报错

访问sass编译后的css，出现警告。
![pic alt](http://p1.bpimg.com/567571/79b8a3afeb43ff83.png "opt title")

设置允许非法请求
![pic alt](http://p1.bpimg.com/567571/b810bf7626dfe381.png "opt title")


## *.vue中scss报错

```
<style lang='scss' rel='stylesheet/scss'></style>
```
添加`rel=stylesheet/scss`

## 手机访问内置server

![pic alt](http://p1.bpimg.com/567571/6eb329c840332e54.png "opt title")

注意端口需要修改，63342是默认端口。


## 配置动态编译scss

添加file watcher [官方文档](https://www.jetbrains.com/help/webstorm/2016.3/transpiling-sass-less-and-scss-to-css.html#d253312e140)
注意sass需要使用 **gem** 安装

```
sudo gem install sass
```

gem env 可以查看包的位置

> 编译中文乱码

 关于使用gulp压缩sass中文乱码问题：Invalid US-ASCII character "\xE5") 

```shell
cd /Library/Ruby/Gems/2.0.0/gems/sass-3.4.23/lib/sass
sudo vim engine.rb
```

添加一行 `Encoding.default_external = Encoding.find('utf-8')`, 在require的后面


## window下watch失效

  ![pic alt](http://i2.muimg.com/567571/9ea5c3c38362f3f5.png "opt title")

  把safe_write选项去掉

