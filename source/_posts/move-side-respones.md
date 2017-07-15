---
title: 移动端响应式解决方案
date: 2017-03-13 19:13:21
tags:
---

移动端尺寸不一，所以需要一套自适应的方案。
需要考虑我们的场景，有些时候，我们只需要宽度按设备尺寸伸缩，纵向上可以是固定的值
有时候，我们需要横向，纵向上都能按设备尺寸伸缩，例如铺满屏幕的应用。

为什么移动端会有尺寸适配的问题

## 概念

> 物理像素(Device Pixels)
    
真实物理设备上的像素点, 单位是px，但也是相对的，因为我的点可能比你的点小

> 逻辑像素(设备独立像素)
    
逻辑像素是一个相对的抽象概念,并不是实际的具体长度。比如css像素，它只是一个单位，具体渲染出来有多大得看设备。

> dpr (Device pixel ratio)

* 表示每个逻辑像素（css）有几个物理像素
* dpr = 物理像素 / css像素
* window.devicePixelRatio 可以直接获取

> dpi/ppi (Dots Per Inch / Pixel Per Inch)

* 像素密度，每英寸的像素点个数。
* Math.sqrt(750*750 + 1334*1334) / 4.7 = 326ppi // 屏幕对角线的像素尺寸 / 物理尺寸（inch）

> viewport

* 视口，桌面上视口宽度等于浏览器宽度，也等于css像素，但在手机上有所不同 。 
* 它也是一个虚拟的概念，因为手机的分辨率高，在没有缩放的情况下，手机上的视口要比屏幕宽度大得多，但它实际尺寸就是屏幕那么大，所以就只好缩小网页，将其容纳于视口之中。
* <meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no"/>, 所以有了这个经典的meta标签，设置视口width=device-width 。
* 这也是为什么手机上能双指缩放



## 百分比

百分比布局只能用在横向上，比如宽度, 并且它是参照父元素的宽度。
高度百分比，需要html,body 等一层一层的往下写高度，才能有参照,参照的是父元素的高度

* **需要层层继承父元素的宽，高**
* **但是在高度上使用百分比，在不同机型上会有拉伸效果。比如图片，因为图片的伸缩比例和设备的比例是不同的。**
* **其中有个坑就是margin，padding，它的百分比是参照的父元素的宽度**。 所以用百分比布局，如果要在纵向上自适应，就相当麻烦了。

但这个特性可以用来解决**已知图片比例**的自适应问题。
因为padding是按父元素的宽度来计算百分比的，在我们知道图片比例的情况下，可以通过padding来根据宽度自适应计算出高度。

```css
.wrap{
    height : 0;
    padding-top : 75%;
}

.img{
    position : absolute;
    top : 0;
    left : 0;
    width : 100%;
    height : 100%;
}
```


## 百分比绝对定位

不需要层层继承父元素的宽，高，只需要参照最外层的定位元素就可以了。
缺点就是所有元素都脱离了的文档，采用平面定位的方式来布局。


## 动态计算rem

既然要伸缩，就需要一个统一的参照标准，这是最好的。

在rem是参照html元素的字体大小, 可以做到所有元素有统一的参照。

不过rem的布局，在缩小时不一定能完全按比例缩小。

rem 默认是1rem=16px 

> rem的换算

手机上的分辨率高，一个css像素点往往是几个物理像素点组成, 所以ue给的psd量的尺寸需要经过换算，

iPhone6  375设备宽度   750设计稿宽度 正好是2倍
6s 414设备宽度 1125设计稿宽度 3倍


下面是一个用js动态计算html的rem的例子

```javascript
(function setResponseRem(basePx, width) {
    var doc = document,
        win = window,
        docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        timer = null,

        recalc = function () {
            clearTimeout(timer);
            timer = setTimeout(function () {
                var viewportWidth = docEl.getBoundingClientRect().width || docEl.clientWidth;
                if (!viewportWidth) return;
                docEl.style.fontSize = basePx * (viewportWidth / width) + 'px';
            }, 10);
        };

    recalc();

    win.addEventListener(resizeEvt, recalc, false);
})(100,750);

```

 rem自适应，css=实际图的px / 100
 basePx : 100 ,换算基准, 1rem等于多少px，这个值可以随意定，因为后续px换算rem时会除以这个值，设为100只是为了方便计算
 width : 750 代表设计稿宽度
 viewportWidth : 代表设备宽度，css像素
 viewportWidth / width : 缩放比例, 其实就 1/dpr
 fontSize : 实际的html上的字体大小

 我们的换算基准是固定的，缩放的任务交给了rem


 ## 动态计算meta标签

`<meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no"/>`
从前面的viewport的知识，我们知道，viewpoint是可以进行缩放的，一般情况下，我只是把viewpoint设置成了设备的css宽度，并且没有缩放。通过rem动态计算来实现缩放。其实也可以用它来实现响应式缩放。
通过js计算出当前设备的dpr
scale = 1 / dpr
动态设置meta标签的scale缩放程度。

rem我们也用，只不过它只是作为一个写法标准，缩放的任务是交给了meta 标签的scale属性。

> [lib-flexable](http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)

这是淘宝沉淀的自适应方案，就是采用动态计算meta标签和rem，来做的。

原理 ：

和响应式方案，媒体查询有冲突

只兼容了安卓


## flexbox 

弹性布局对移动端比较友好，功能比inline-block强大，在横向上的布局，建议用flexbox
移动端兼容，建议使用autoprefix

```css
  .f-box{
    display: flex;
    flex-wrap: nowrap;
  }

  .f-box-cld{
    flex : 1;
    width: 40%;
  }
```

