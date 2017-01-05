---
title: nodejs--deepCopy
date: 2016-12-02 17:26:42
type: "categories"
categories: "踩过的坑"
---

原以为 nodejs对es6的支持很好了，会有这个方法。结果发现nodejs不支持深度拷贝

Object.assign(target,source,source), 只是浅拷贝。


## 自实现深拷贝

```javascript

/**
* 继承对象方法
*
* @param parent 继承者
* @param child  被继承者
* @param isDeep 是否深度拷贝
* @isMerge 数组合并,注意值没有去重
* @returns number 失败返回-1
*/ 
function  extend(parent, child, isDeep, isMerge) {
	if ( typeof parent !== 'object' || typeof child !== 'object') {
	    return  parent;
	}

	if (isDeep) {
	    for (var i in child) {
		if (child.hasOwnProperty(i)) {
		    if ( typeof child[i] === 'object') {
			if (isMerge && Array.isArray(child[i]) && Array.isArray(parent[i]) ) {
			    var p1 = this.extend([], parent[i], isDeep);
			    var c1 = this.extend([], child[i], isDeep);
			    parent[i] = p1.concat(c1);
			}else{
			    parent[i] = arguments.callee({}, child[i], isDeep);
			}
		    }else{
			parent[i] = child[i];
		    }
		}

	    }
	}
	else {
	    for (var j in child) {
		if (child.hasOwnProperty(j)) {
		    parent[j] = child[j];
		}

	    }
	}
	return parent;
}
```

可以支持深浅拷贝，可以支持数组merge
