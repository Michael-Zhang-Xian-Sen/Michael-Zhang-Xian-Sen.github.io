---
title: vue学习笔记：工具
date: 2020-04-24 11:23:50
categories: 前端
tags: [vue, 前端]
description: 
---

主要学习vue的单文件组件、单元测试、TypeScript及生产环境的部署。

<!-- more -->

## 目录

## 1. 单文件组件
单文件组件大致指的是将一个vue文件作为一个组件。使用vue-cli搭建的项目中的`.vue`文件即单文件组件。

## 2. 单元测试
将组件当作黑盒，只关注输入和输出，只针对输入和输出进行测试。不论如何改变组件的内部逻辑，只要该组件响应输入的输出内容正确，能够通过测试，则该组件就是正确的。

进行单元测试需要安装如下插件：
1. `Vue Test Utils`。安装命令：`npm install --save-dev @vue/test-utils`
2. 一个测试运行器（`jest`或`mocha`）。安装命令：`npm install --save-dev jest`

相关学习资料
* Vue Test Utils：https://vue-test-utils.vuejs.org/zh/
* jest：https://jestjs.io/docs/en/getting-started

## 3. TypeScript支持


## 4. 生产环境部署