---
title: vue是什么
date: 2017-02-13 19:06:29
tags:
---

![pic alt](http://o99eh3ii0.bkt.clouddn.com/17-4-22/43478794-file_1492820528872_bccd.png "opt title")

vue是前端渲染库的新宠，它只关注于view层，与传统的展现于数据分离，展现于操作分离不同。 vue推荐将操作直接绑定到视图，显得更加直观。以数据驱动的思想进行开发，将dom同步工作交给vue，开发者只需要关注数据和状态的变化，以及需要的操作。

## 面向数据驱动

这一点，虽然看着很好理解，但在实际开发过程中，不一定能把握住精髓。我们被jQuery等dom操作的库洗脑太深了，不自觉的会往dom操作上面去想，其实vue已经做了dom操作的封装，我们真正关心的是**数据的变化**，一切状态的改变，ui的改变都应该是数据的变化，我们focus在数据这个点上，dom操作交给vue去处理就好, 当以这种思维去开发时，会发现以前很复杂的问题抽象到数据上来看，变得清爽，简洁了很多，一目了然。对于维护大型前端软件来说，正好切中了痛点。

## 陌生中的熟悉
我们在写vue的时候是不是觉得有些写法很眼熟，对了，就是我们用了很久的模板引擎。当初的模板引擎概念局限在了模板渲染上，如今大行其道的MVVM，正是吸收了数据驱动的思想精华后的产物。它不光拥有模板引擎所有的能力，还升级了它在模块化，响应式watcher等能力。所以我们对如今纷繁的前端渲染库也不必感到那么惊慌，它们其实也是从我们熟悉的技术中演变进化而来的，MVVM就可以理解为结合了诸多前端前沿技术概念的升级版模板引擎。它就是一个增强版的view层，隐藏了dom操作，浓缩了业务逻辑到视图， 响应式通信。一切都围绕着数据驱动，让前端开发更抽象，更工程，更合理。解放了开发者的生产力。

## 响应式watcher

脏检查

es5 geter/serter

## 组件通信

refs方式：parent.$refs.list.<Method>
customer event方式: 先dispatch给父组件，再由父组件broadcast给子组件(vue1.x)
event bus 使用一个空的vue对象，作为全局的事件中心
props方式

## 编译版本

runtime

## vue实战

### vue-cli

vue-cli 是vue project 的**手脚架工具**，官方用webpack/browserfy, 可以自定义模板, 包括eslint  karma 等完整技术栈

> Useage

`vue init <template-name> <project-name>`

> 默认包含这几个模板

* webpack - A full-featured Webpack + vue-loader setup with hot reload, linting, testing & css extraction.

* webpack-simple - A simple Webpack + vue-loader setup for quick prototyping.

* browserify - A full-featured Browserify + vueify setup with hot-reload, linting & unit testing.

* browserify-simple - A simple Browserify + vueify setup for quick prototyping.

* simple - The simplest possible Vue setup in a single HTML file

> 自定义模板

`vue init username/repo my-project`

### webpack入门


### [vuex](http://vuex.vuejs.org/zh-cn/intro.html)

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以**相应的规则**保证状态以一种可预测的方式发生变化

状态其实也是数据，在一个整体应用中，状态被分散了在各个组件内部，然而某些场景是需要将状态共享给其他组件，或者其他组件也需要修改同一状态。

vuex的解决方法是将这些状态和改变状态的行为都提出来，做一个集中式的管理, 所有组件只需要和store进行就交互就可以了。

对比全局的事件中心，它只提供了行为的共享，并未提供状态的共享。

vuex可以看成是全局变量 + 全局事件中心的结合, 同时具有响应式的能力。

> 单项数据流

多个视图依赖于同一状态。
来自不同视图的行为需要变更同一状态

对于问题一，传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。对于问题二，我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致无法维护的代码。

> 什么情况下使用vuex

如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 global event bus 就足够您所需了。但是，如果您需要构建是一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，


使用 Vuex 并不意味着你需要将所有的状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

> 使用

要使用 store ，首先必须 Vue.use(Vuex) ，然后将 store const store = new Vuex.store() inject 定义到 Vue 实例 app 中 new Vue({store}) ，实现从根组件注入到所有子组件中，接着就可以在子组件中使用 this.$store 调用了。

当一个组件需要使用多个某 store 的状态属性或 getters ，可以使用 shared helper —— 共享帮手 mapState ，它会返回一个对象 。


state可以设置计算属性


一条重要的原则就是要记住 mutation 必须是同步函数
store.commit('increment')
// 任何由 "increment" 导致的状态变更都应该在此刻完成。

store是响应式的，变化后，会触发vue组件更新


## vue1.x 迁移 vue2.x

## vuex1.x 迁移 vuex2.x

1. mutation的触发变成了commit
2. action可以写入到store里面
3. mapGetter，mapAction，mapMution，帮助我们映射store,getter到本地
4. action参数的改变，只有2个参数


## 引用
[vuex示例](https://segmentfault.com/a/1190000005018970)
