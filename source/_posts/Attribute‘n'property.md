---
title:  关于Attribute和property的区别 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2017-08-01 10:39:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [前端] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 关于attribute和property的区别
---

阅读提示：

1. 本篇文章中的attribute，全部翻译为“属性”。而property，全部翻译为“特性”。
2. 点表示法指js对象通过“.”获取特性，方括号表示法指js对象通过“[]”获取特性。

<!-- more -->

## 抛砖

前段时间，有位学长问了我一个题目：在html文档中，给一个标签添加了一个属性，但是js通过点表示法无法对其进行引用。这是为什么？

当时我是脸上是大写的“懵”字。于是乎写了段代码进行尝试，如图：
![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute1.png)
![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute2.png)
![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute3.png)

点击标题后，并没有返回value的值，而是undefined。结果很是出人意料。
再做一次实验，这回获取的是DOM对象的id，代码及结果如下

![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute4.png)
![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute5.png)
![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute6.png)

对比两次的结果，是不是感觉非常奇怪？

## 引玉

关系到点表示法、括号表示法与”getAttribute”和”Attribute”与”Property”的区别了。

首先，为什么取不到value属性的值呢？

在入门的时候，我们应该都学过这两种操作方式，对一个对象用点表示法或方括号表示法，表示获取该对象的属性。对象的getAttribute方法，也可获取对象的属性。但是，此属性非彼属性，一个是Attribute，另一个是property。

那么，为什么给"<\h3>"标签设置的value，只添加了Attributes中value的值呢？

## 尝试

打开我们的神器——开发人员工具，选择<h3>的DOM节点后，在Elements选项卡下找到Properties，如图：

![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute7.png)

仔细观察上边的键值对，是不是有一个叫做attributes的键？

![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute8.png)

而我们的id、value都在里边有所显示。继续展开0:id、1:value来一探究竟。

![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute9.png)

![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute10.png)

可以发现，二者展开后都具有nodeValue字段，并且他们的值为html标签中的属性值。

但是在properties中，我们可以找到名为id的键，且其绑定值为myHeader，而无法找到名叫value的键。可见，我们在html标签中声明的id，同时存在于property和attribute中，而value只在attribute中具有。

之所以会出现“抛砖”中的现象，是因为点表示法和方括号表示法，获取到的是对象的property，而getAttribute方法获取的是对象的Attributes。

## 疑问

#### Question:

为什么在html标签中声明的不同属性，一个存在于对象的property，而另一个在对象的property和attribute中都存在呢？

#### Answer:

区分以下几种情况：

1. 在html标签申明属性。
    1. 若该属性在对象的property中存在，properties和attributes二者都可能会更新（若有特殊限制，如dir，对于值的格式有要求，则有可能在property中不会更新）
    2. 若属性不存在对象的property中，则只会在attributes中刷新。
2. 通过js添加属性
    1. 如果是通过使用点表示法和方括号表示法添加属性，则只会在properties中添加，而不会在attributes中添加。
    ![啊图片](http://opqksc9nz.bkt.clouddn.com/attribute11.png)
    2. 同理，通过对对象使用setAttributes方法添加属性，则会在对象的attributes中添加该属性，在对象的property有可能添加该属性（若该属性之前存在于property中，且对该属性的值如果有特殊要求，本次赋值对其满足，则可添加）。


## 总结

让我们再来回顾一下本篇文章中所涉及的知识点：

1. 对象使用点表示法和括号表示法与使用“getAttribute()”方法有何不同。
2. 如何通过js或开发人员工具查看对象的properties和attributes。
3. 如何通过js对对象的property和attribute进行设置。
