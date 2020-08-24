---
title: 关于箭头函数 
date: 2020-05-20 11:09:00 
categories: es6
tags: [es6, js]
---

### 常用方法
* `console.log` 用于输出普通信息
* `console.info` 用于输出提示性信息
* `console.error` 用于输出错误信息
* `console.warn` 用于输出警示信息
* `console.debug` 用于输出调试信息

针对不同类型的信息，大多数浏览器在consoole会使用不同的标志进行标识，并可以根据信息类型进行筛选。

### 辅助方法
* `console.table(obj)` ：可以将对象或者数组以表格的形式直观地打印出来
* `console.count()`：以参数为标识记录调用的次数，调用时在控制台打印标识以及调用次数。
* `console.countReset()`：重置指定标签的计数器值。
* `console.time()`：启动一个以入参作为特定名称的计时器，在显示页面中可同时运行的计时器上限为10,000.
* `console.timeEnd()`：结束特定的 计时器 并以豪秒打印其从开始到结束所用的时间。
* `console.timeLog()`：打印特定 计时器 所运行的时间。

### 占位符
* css占位符（仅在chrome支持？）：`console.log("%cHello World",padding:50px;font-size:40px;color:gray);`