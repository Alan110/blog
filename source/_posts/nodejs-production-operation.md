---
title: nodejs生产环境运维
date: 2017-10-15 09:39:56
tags:
---

nodejs在生成环境的运维比较复杂，主要是版本多，切换成本较高，奈何node升级很快，node 包版本良莠不齐，稍不注意就使得我们的线上应用挂了。 node更新很快，跟进新的版本有助于程序的迭代，然而过于新也会导致线上的不稳定。

[nodejs官方发布计划](https://github.com/nodejs/Release#release-schedule1)

![](http://o99eh3ii0.bkt.clouddn.com/FhNgHi0m4FOSk2bwg0Y__99lRMKf)

每隔一年会发布一个LTS长期支持版本，生产环境都建议部署LTS版本。
可见今年10.31即将发布8.x的LTS版本，值得期待。这也是写本文的原因，准备升级server的node版本。


## node版本管理


> [n](https://github.com/tj/n)


n是tj大神的项目,它是npm的一个包, 使用顺序是: node > n > node

它依赖一个已经安装好的node， 公用已经安装好的全局node，通过下载不同版本的node，进行替换。这会导致全局安装的node_module在不同版本的node中是**共享的，虽然不用再次下载，但可能会存在兼容性问题**

`npm install -g n`
`n 6.11.3`
`n ls`

> [nvm](https://github.com/creationix/nvm) (推荐)

nvm 是一个bash脚本，独立与node 之外，**安装多个node版本的备份保存在~/.nvm目录下，通过改变路径映射实现多版本切换**。每个node版本都需要安装自己的全局模块。

`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash`  
安装nvm

`command -v nvm` 
检测是否安装成功，在Linux上如果没有任何反应执行`source .zshrc/.bashrc` 或者重新打开一个shell

`nvm install node` 
下载包，并自动切换到该版本，**目前没有发现只下载，不自动切换的办法**

`nvm install 6.11.3`
安装具体版本

`nvm ls` 
查看所有安装的版本
**如果在安装之前，已经有全局的node，在这里会看到一个system的版本，你可以再次使用它**
`nvm use system`
![](http://o99eh3ii0.bkt.clouddn.com/FoXQxqmpBVQAMeV_VSt6Gd3v2AlS)


`nvm run 6.11.2 file.js` 
使用指定版本运行js文件

`nvm install 6 --reinstall-packages-from=5`
每个版本都有自己的全局node_modules,好在它提供了一个参数，我们不用每次都重新手动安装。你可以指定从哪个版本复制包引用




## npm包版本管理

> npm

npm 3 有个问题是当执行 npm install pkg --save-dev 的时候会自动写入package.json文件，但是其版本写的是`"autoprefixer": "^6.7.2"` , **我们知道^代表可以升级第二位开始的版本号，在别的地方install这个项目，可能版本号不一致，许多库第二位版本号升级也会有较大的改动，从而导致项目无法运行成功**。

> yarn (推荐)

yarn **只是代替了npm的客户端，依旧是使用的npm的仓库** 用法基本和npm 一致,yarn会生成yarn.lock 文件，确保依赖的版本 。 yarn add 一个包的某个版本， 会自动安装相关依赖

`yarn config set registry https://registry.npm.taobao.org` 
使用淘宝镜像

`yarn install`
`yarn add package`
`yarn remove package`
常见命令

`yarn upgrade vue` 
升级包，用这个命令，会自动安装相关依赖升级

> npm5

npm5开始也有package.lock.json文件，也会锁定版本



## [线上应用运维-pm2](http://pm2.keymetrics.io/docs/usage/signals-clean-restart/)

1.如果是ssh登录，运行node server，当断开ssh的时候，服务也会被断开
2.应用的输出日志

pm2这个完美解决这2个问题,应用挂了后会自动重启，还可以进行程序管理,可以监听文件变化自动重启，保存应用输出的所有日志。
所以可以不用单独做日志的处理，可以pm2可以捕获console，和程序运行异常的错误到.pm2/log。

常见命令
`Pm2 start app.js  `
`Pm2 restart app`
`Pm2 show app`
`Pm2 list`
`Pm2 logs app`

> 执行环境变化

当执行命令变化时，比如添加的环境变量变化  ， NODE_ENV=production pm2 start app.js  
需要执行 pm2 delete id ，删除应用，不然无论怎么restart，执行环境都没变

> 不自动reload

pm2 reload dpub2_pre --no-autorestart    
所有的pm2 命令都要带上这个参数

> 多线程