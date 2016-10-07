---
title: js设计模式-桥接模式
date: 2016-09-24 23:29:04
type: "categories"
categories: "设计模式"
tags:
---


桥接模式的作用在于将实现部分和抽象部分分离开来， 以便两者可以独立的变化。在实现api的时候， 桥接模式特别有用。
运用好桥接模式，对软件的扩展性和健壮性而言有莫大好处。它提醒开发者，要分清楚什么是实现，什么抽象。在面对多种环境的时候,我们要做的是面向接口编程，面向抽象编程。实现可以在运行时确定。将纵向的依赖拉平。或者说是找到抽象与实现之间真实的依赖,它是不容易改变的。


动态类型语言对变量类型的宽容给实际编码带来了很大的灵活性。由于无需进行类型检测，我们可以尝试调用任何对象的任意方法，而无需去考虑它原本是否被设计为拥有该方法。
这一切都建立在鸭子类型（duck typing）的概念上，鸭子类型的通俗说法是：“如果它走起路来像鸭子，叫起来也是鸭子，那么它就是鸭子。”

这个故事告诉我们，国王要听的只是鸭子的叫声，这个声音的主人到底是鸡还是鸭并不重要。鸭子类型指导我们只关注对象的行为，而不关注对象本身，也就是关注HAS-A, 而不是IS-A。

javscript不需要接口这种超类型的帮助，它并没有真实的类，只有对象。任意对象的任意方法都可以被复用。

什么时候去使用桥接模式:
1. 你想在运行时确认实现，


常用的桥接手段
1. 代码复用 （实现继承是强依赖，接口继承是若依赖）
2. 扩展性，稳定性强

将事物抽象，面向抽象编程。因为抽象的东西是不容易变的。

> 回调

运行时确认方法逻辑。

> 抽象接口

运行时确定类的定义。