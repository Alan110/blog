---
title: vue2高级技巧-基础能力
date: 2017-05-07 18:36:58
---


## 递归组件

使用name属性, 便可在组件的template中直接使用组件本身, **比如在某些递归的场景就需要如此使用**

```javascript
<template lang="html">
    <li>
        <item></item>
    </li>
</template>
<script>
    import utils from 'utils'
    export default{
        name : 'item',
        props: {
            model: Object
        }
    }
```

## 动态切换component

当我们需要动态切换组件的时候，可以使用内置的component标签，不必写冗余的if else，但是要注意的是，动态切换默认会重新渲染组件，在某些比较费时的组件又需要频繁切换的时候，可以用keep-alive标签使其保持在内存。

**动态component其实可以和异步组件，以及vuex数据结合**，实现按需注册并加载使用组件。

```javascript
<component v-for="(layer,index) in curScene.layers" :key="layer.id" :is="layer.type" source="scenebox" :layer="layer" :index="index" :layers="curScene.layers"></component>

// 保持组件在内存，不会重新初始化
<keep-alive>
    <component v-for="(layer,index) in curScene.layers" :key="layer.id" :is="layer.type" source="scenebox" :layer="layer" :index="index" :layers="curScene.layers"></component>
</keep-alive>

```

## v-model 的秘密

vue的双向数据绑定其实语法糖，通过内部触发input事件，配合指令实现。
http://www.cnblogs.com/wwlhome/p/6551165.html

## v-for key

用于强制替换元素/组件而不是重复使用它
key 是唯一id，不能重复，否则会报致命错误
如果不使用key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试修复/再利用相同类型元素的算法。使用key，它会基于key的变化重新排列元素顺序，并且会移除key不存在的元素。

有子组件的时候，需要加上key

## 调整数组顺序

可以使用数组的变异方法splice
```javascript
layers.splice.apply(layers, [toIndex, 0].concat(moved = layers.splice(fromIndex, howMany)) )`
```

## v-for 和 v-if 需要同时使用时

使用<template></template>标签替换, **template是一个空标签，没有实际渲染内容，但是可以作为逻辑的载体**

```javascript
<template  v-for="page in pages" >
    <tr class="item" v-if="!page.global" :key="page.id">
        <td><img :src="page.global.sceneImgs[0] + '?key=' + randomKey" class="page-thumb"
                    v-if="page.global.sceneImgs && page.global.sceneImgs[0]" style="height:100%;" alt=""></td>
        <td>{{ page.title }}</td><Paste>
</templte>
```

## 动态组件

利用webpack的异步加载模块的能力，和vue的异步注册实现动态组件加载。

异步import返回的是一个promise

```javascript
Vue.component('widget',()=> import('./widget/text'));
```

## [指令](https://cn.vuejs.org/v2/guide/custom-directive.html#main)

在 Vue 中可以把一系列 复杂的操作 包装为一个指令
什么是复杂的操作?
复杂逻辑功能的包装、违背数据驱动的 DOM 操作以及对一些 Hack 手段的掩盖等。我们总是期望以操作数据的形式来实现功能逻辑。

在vue的指令中是不可以双向数据绑定的，如官方所说：除了 el 之外，其它参数都应该是只读的，尽量不要修改他们。如果需要在钩子之间共享数据，建议通过元素的 dataset 来进行

指令可以在钩子函数中去封装dom, 应尽可能的操作当前dom，比如在inserted的时候，当前dom插入文档时
如果要操作别的dom就需要等待他们插入文档，此时可以使用nextTick,vnode.context为当前的vue对象

### 拖拽示例
```javascript
Vue.directive('drag', {
    /**
     * value.target = 实际需要移动的dom元素的ref名
     */
    inserted: function (el, binding,vnode) {
        // 下一帧再获取dom元素
        vnode.context.$nextTick(function(){
            // 实际移动的元素
            let moveEl = this.$refs[binding.value.target];
            // 覆盖下面有监听mousemove的元素，避免干扰
            let $mask = $('<div class="project-mask" style="position: absolute; top : 30px; left: 0; right:0;bottom:0; z-index: 200px; display: none;"></div>').appendTo(moveEl);
            el.onmousedown = function (ev) {
                $mask.show();
                var disX = ev.clientX - moveEl.offsetLeft;
                var disY = ev.clientY - moveEl.offsetTop;
                document.body.focus();
                // prevent text selection in IE
                document.onselectstart = function () { return false; };
                // prevent IE from trying to drag an image
                moveEl.ondragstart = function () { return false; };

                document.onmousemove = function (ev) {
                    var l = ev.clientX - disX;
                    var t = ev.clientY - disY;
                    moveEl.style.left = l + 'px';
                    moveEl.style.top = t + 'px';
                };
                document.onmouseup = function () {
                    $mask.hide();
                    document.onmousemove = null;
                    document.onselectstart = null;
                    moveEl.ondragstart = null;
                    document.onmouseup = null;
                };
            };

        });
    }
})
```

## vuex

> vuex于eventBus的区别

vuex = getters + actions + mutations
状态管理器，vuex = 全局事件中心 + 全局变量
eventBus = 全局事件中心
所以vuex更加强大, 

> 组件通信心得

* 只是简单的父子组件通信，使用$emit，$on
* 稍微复杂的跨组件通信使用eventBus
* 非常复杂的跨组件通信，以及状态切换使用vuex


> vuex心得

**在大型复杂的软件架构中，推崇提取组件状态到state成为公共状态**更利于复用，和通信。
大型软件架构中**vuex可以拆分成模块，异步注册**

* 推荐所有的状态变化都通过mutations来改变。如果1个操作很复杂，或者是公用的，则可提取到action中，action中可以再访问action，或者提交多个commit，以及异步操作。
* **mutations中的方法，应该尽可能的抽象, 满足尽可能多的情况。**
* payload应该使用对象，拥有更好的扩展性。

