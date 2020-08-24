---
title: chrome浏览器的渲染过程
date: 2020-08-17 15:04:50
categories: 前端 
tags: [浏览器原理]
---

## 浏览器的渲染过程分为以下几步：
1.解析HTML，构建DOM树
2.解析CSS样式表，构建CSSOM(CSS Object Model)
3.将DOM和CSSOM进行合并生成Render Tree(渲染树)
4.根据Render Tree计算布局
5.依据Render Tree进行渲染

## 关于回流和重绘
1. 回流(reflow/layout)：当Render Tree中的一部分(或所有)因为其中元素的规模尺寸、布局(计算确切位置)、隐藏等改变而需要重新构建Render Tree。
2. 重绘(repaint/painting)：当Render Tree中的一些元素需要更新属性，但这些属性只会影响元素的外观，风格，而不会影响布局，无需重新构建render tree。

导致回流的具体情况：
* 添加或删除可见的DOM元素
* 元素的位置发生变化
* 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
* 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。
* 页面一开始渲染的时候（这肯定避免不了）
* 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）

回流一定会触发重绘，但是重绘不一定触发回流。

## 优化方法
* 利用cssTest属性和修改class更改元素样式，避免直接修改元素样式，最小化重绘和重排。
* 让DOM脱离文档流，进行修改完毕后，再回到文档流。脱离文档流后的改动不会引起回流。
* 现代浏览器大部分有一个队列，用于优化重排过程。而类似offsetTop的方法会强制队列刷新。所以需要避免在修改样式时直接引用以上属性，即避免触发同步布局事件。
* 复杂动画最好能脱离文档流。
* css3硬件加速。

### 参考资料
1. 谷歌开发者web基础：https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction?hl=zh-cn
2. 你真的了解重流和重绘吗：https://segmentfault.com/a/1190000017329980