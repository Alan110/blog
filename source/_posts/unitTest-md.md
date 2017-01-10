---
title: 前端单元测试
date: 2016-08-25 20:49:05
type: "categories"
categories: "前端自动化测试"
tags:
---

## 单元测试风格

BDD TDD

## 断言库

> [jasmine](http://jasmine.github.io/edge/introduction.html)

![](http://o99eh3ii0.bkt.clouddn.com//16-8-28/59843772.jpg)
jasmine 是一个bdd驱动的测试框架,自带**断言库**
支持部分函数spy，stub，mock的能力
支持异步测试

> 总结: 在浏览器上做单元测试是比较好的，但是与别的工具整合有些麻烦，需要自行实现reporter

___

> [mocha](http://mochajs.org/)

![](http://o99eh3ii0.bkt.clouddn.com//16-8-28/96121519.jpg)
mocha 是一个功能丰富的js测试框架，可以有BDD和TDD的测试风格。
支持nodejs,浏览器端测试
支持异步测试
支持任意断言库

> 总结: **功能完善，可配置性强,与其他工具整合方便，也需要自实现报表**

## sinon.js 增强测试手段

为了更好的测试，减少依赖，sinon.js提供一些增强测试手段或者说概念。

> [spy](http://sinonjs.org/docs/)

A test spy is a function that records arguments, return value, the value of this and exception thrown (if any) for all its calls

spy是一个间谍函数，它用于测试函数callback, 或者对象的某个方法。就像间谍，他还是他，只不过他会向我们汇报"敏感信息"。
spy可以告诉我们函数调用的参数，返回值，this对象，和它被调用的次数，抛出的异常等。

```javascript
{
    setUp: function () {
        sinon.spy(jQuery, "ajax");
    },

    tearDown: function () {
        jQuery.ajax.restore(); // Unwraps the spy
    },

    "test should inspect jQuery.getJSON's usage of jQuery.ajax": function () {
        jQuery.getJSON("/some/resource");

        assert(jQuery.ajax.calledOnce);
        assertEquals("/some/resource", jQuery.ajax.getCall(0).args[0].url);
        assertEquals("json", jQuery.ajax.getCall(0).args[0].dataType);
    }
}
```

> [stub](http://sinonjs.org/docs/#stubs) 

stub是一个依赖的替代函数，当我们测试的模块依赖别的API接口时，就可以使用stub来代替, 预先定义好行为。
stub本质上也是一个spy，拥有spy完整的能力。

```javascript
it("returns the return value from the original function", function () {
    var callback = sinon.stub().returns(42);
    var proxy = once(callback);

    assert.equals(proxy(), 42);
});
```

> mock


> fake

```
var xhr, requests;

before(function () {
    xhr = sinon.useFakeXMLHttpRequest();
    requests = [];
    xhr.onCreate = function (req) { requests.push(req); };
});

after(function () {
    // Like before we must clean up when tampering with globals.
    xhr.restore();
});

it("makes a GET request for todo items", function () {
    getTodos(42, sinon.spy());

    assert.equals(requests.length, 1);
    assert.match(requests[0].url, "/todo/42/items");
});
```
