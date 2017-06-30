---
title: github/gitlab web-hook
date: 2017-06-16 09:55:09
tags:
---

日常开发场景中，我们可能需要频繁的去线上机器拉取最新git代码，比较繁琐。这些工作可以交给webhook来做。
github、gitlab都是支持webhook功能的
webhook的意思，就是当我们的仓库发生提交操作时，github、gitlab会往我们预先配置好的地址发送一个请求。从请求中我们可以拿到相应的提交信息，然后判定执行什么操作，比如git pull 或者编译等操作,可以写成shell脚本。
所以，我们需要在线上服务器上部署一个监听server，php，nodejs，python等都可以。

## webhook server

我用的nodejs，所以找了一个nodejs的webhook模块。这里要注意一下，**gitlab和github是不一样的，所以需要找相应的三方模块**，别弄错了。

> (gitlab-webhook-handler)[https://github.com/Flying-Ape/gitlab-webhook-handler]

基础代码如readme说明

```javascript
var http = require('http')
var createHandler = require('gitlab-webhook-handler')
var handler = createHandler({ path: '/webhook' })

http.createServer(function (req, res) {
  handler(req, res, function (err) {
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(7777)

console.log("Gitlab Hook Server running at http://0.0.0.0:7777/webhook");

handler.on('error', function (err) {
  	console.error('Error:', err.message)
})

handler.on('push', function (event) {
  	console.log('Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref)
})

handler.on('issues', function (event) {
  	console.log('Received an issue event for %s action=%s: #%d %s',
    event.payload.repository.name,
    event.payload.action,
    event.payload.issue.number,
    event.payload.issue.title)
})
```

注意path参数，为访问路径，在gitlab上面配置webhook信息，配置发送请求的目标地址 ip + path

可以点击下面的test，进行测试。


## 处理

在拿到的提交信息中，我们可以自己过滤执行不同的操作。比如是哪个分支，提交人是谁等等