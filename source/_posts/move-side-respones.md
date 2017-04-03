---
title: 移动端响应式解决方案
date: 2017-03-13 19:13:21
tags:
---

移动端尺寸不一，所以需要一套自适应的方案。
需要考虑我们的场景，有些时候，我们只需要宽度按设备尺寸伸缩，纵向上可以是固定的值
有时候，我们需要横向，纵向上都能按设备尺寸伸缩，例如铺满屏幕的应用。


## viewport

## 百分比

百分比布局只能用在横向上，比如宽度, 并且它是参照父元素的宽度。
高度百分比，需要html,body 等一层一层的往下写宽度，才能有参照

其中有个坑就是margin，padding，它的百分比是参照的父元素的**宽度**。 所以用百分比布局，如果要在纵向上自适应，就相当麻烦了。

有一种情况例外，那就是纯绝对定位布局，都参照最外层元素的宽高就行了。

建议使用sass进行百分比换算

## 纯绝对定位

## rem

既然要伸缩，就需要一个统一的参照标准，这是最好的。

在rem是参照html元素的字体大小, 可以做到所有元素有统一的参照。

不过rem的布局，在缩小时不一定能完全按比例缩小。

> rem的换算

rem 默认是1rem=16px 
有个坑点，是手机上的分辨率高，一个物理像素点往往是几个像素点组成, 所以ue给的psd量的尺寸需要经过换算，一般参照iPhone6 设计的图，要除以2，参照iPhone6 plus的需要除3

下面是一个用js动态计算html的rem的例子

```javascript
(function (win,doc) {
    var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        timer = null,

        recalc = function () {
            clearTimeout(timer);
            timer = setTimeout(function () {
                var viewportWidth = docEl.getBoundingClientRect().width || docEl.clientWidth;
                if (!viewportWidth) return;

                docEl.style.fontSize = 100 * (viewportWidth / 750) + 'px';
            },10);
        };

    recalc();

    win.addEventListener(resizeEvt, recalc, false);
})(window,document) ;

```

```sass

$baseFontSize = 40px; // iphone6 下

@function rem($px){
  @return $px / $baseFontSize * 1rem;
}
```

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

