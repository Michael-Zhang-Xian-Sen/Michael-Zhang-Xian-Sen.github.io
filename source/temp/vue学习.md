---
title: vue学习：可复用性&组合
date: 2020-10-27 15:11:50
categories: Linux
tags: []
description: vim中文乱码解决方案。
thumbnail: http://opqksc9nz.bkt.clouddn.com/vim2.jpg
---

## 目录
1. 混入（mixin）
2. 自定义指令
3. 渲染函数&JSX

## 1. 混入
理解：Vue中使用全局变量的方案。顾名思义，混入功能可以将vue组件的一些选项如data、created、methods，置入到每一个组件中，从而被每个组件使用。混入的内容将涉及所有组件——十分混乱；混入的内容可注入到每个组件中——十分深入。

* 选项合并：解决混入时的同名选项冲突问题。
    * 数据对象：递归合并，组件优先。
    * 生命周期钩子：合并为数组，mixin中的钩子先调用。
    * 值为对象的选项：合并为同一个对象，键名冲突时组件优先（如methods中的同名方法）。
* 全局混入：即全局注册，混入的主要使用场景。
* 自定义选项合并策略（不太清楚这一块的作用。）

### 全局混入用法实践
* 将混入内容单独写到一个文件中，例如：
```javascript
const mixin = {
    data(){
        return {
            msg:"Hello~"
        }
    }
}
export default mixin
```
* 在main.js文件中引入：
```javascript
import mixin from '路径/mixin.js'
Vue.mixin(mixin)
```

## 2. 自定义指令
理解：通过自定义指令可以更方便地控制dom的样式及行为，而不仅仅局限于v-bind、v-model、v-show等。

* 钩子函数：类似组件的生命周期。这就是钩子函数就是自定义指令的生命周期。
* 钩子函数参数：所绑定元素的DOM、包含指令属性的对象、vnode、上一个vnode。
* 对象字面量：指令函数能够接收所有合法js语句。

## 3. 渲染函数
理解：使用js操作dom。

* 