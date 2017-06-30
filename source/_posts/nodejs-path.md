---
title: nodejs中的路径
date: 2017-06-18 09:15:23
tags:
---

## path.join 和 path.resolve

`path.join`    是组合给予的路径参数，并使用正确的系统分隔符
`path.resolve` 是合并给予的路径参数，并返回绝对路径

```javascript`

path.join('/a', '/b', 'c')      // /a/b/c


path.resolve('/a', '/b', 'c')   // /b/c
```

所以什么时候应该用哪个，一个比较直观的区别点在于你期望'/' 的行为，如果你期望'/'表现为路径相加，就用path.join , 如果你期望'/'表现为root路径，则使用path.resolve
