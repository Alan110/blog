---
title: 模板文件数据Api可视化抽象
date: 2017-03-02 18:34:55
tags:
---


## 场景

通过可视化生成模板文件

当这个模板文件涉及到后端数据交互的时候，这个就很难抽象了，因为API无法抽象确定，每次都可能是唯一的。

## 方案

我们根据后端返回的结果，确定占位符，然后交给前端可视化工具，在数据结果层面做占位符映射，而不在输入层面做占位符映射，因为无法穷举。

![pic alt](http://p1.bqimg.com/567571/4ca04cd67e304b66.png "opt title")

