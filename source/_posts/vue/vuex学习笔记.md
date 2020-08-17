---
title: vuex学习笔记
date: 2020-04-24 11:23:50
categories: 前端
tags: [vue, 前端]
description: 
---

### mapState辅助函数
第一个可选参数不太懂。
第二个参数可以为对象或者数组。
数组，适用于计算属性的名称和state的名称相同时。
对象，key为computed的key，value如果是字符串，直接返回该字符串对应的State。如果是函数，则函数的第一个参数为state。
辅助函数常与扩展运算符一起用。


### 使用常量替代 Mutation 事件类型
#### 具体实践：
1. 将mutation的名称全部作为常量提取到另一个文件`mutations-type.js`，并将全部常量export。
2. 定义mutation时，导入`mutations-type.js`文件中的常量。并且在创建vuex的mutations中全部用常量作为类型名。
3. 触发mutation，即调用commit时，需要将调用的类型名从`mutations-type.js`中引入，并使用常量作为commit方法的类型名。

优点：
1. 如果需要修改mutation的类型名，只需将常量的值进行修改即可，无需改动其他内容。降低改动成本。
2. 可以让合作者对整个应用的mutation一目了然。即mutation是如何定义的、何处调用的commit会更加清晰。
