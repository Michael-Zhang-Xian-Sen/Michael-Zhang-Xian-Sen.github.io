---
title: 前端代码检测工具：eslint #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-03-11 15:04:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [代码质量检查, eslint] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: eslint
---

eslint配置简单、功能强大，官网文档丰富、实用。

一款难得的好工具。

<!-- more-->

# vue项目中使用eslint（vue-cli搭建）

## 1. 安装eslint
* vue-cli安装eslint：在使用vue-cli搭建vue项目时，可选择安装eslint。
* 手动安装eslint至当前项目：`npm i eslint --save-dev`
* 全局安装eslint命令行工具：`npm i eslint -g `

## 2. 配置eslint
### 使用vue-cli安装eslint时的配置文件
vue-cli安装eslint，有两种可能的配置文件方式
1. 配置文件集成在`package.json`文件中
2. 配置文件为`.eslintrc.js`
二者可同时生效

## 3. 配置文件内容解读
以我使用vue-cli搭建的项目的eslint配置文件为例：
```javascript
/* eslint配置 */
module.exports = {
    root: true,     // 只在项目目录中寻找eslint配置文件，禁止向父级目录寻找配置文件。
    env: {          // 在env中指定脚本的运行环境
        node: true  // 使用 Node.js 全局变量和 Node.js 作用域。
    },
    extends: [                  // 扩展配置
        "plugin:vue/essential", // 启用esline-plugin-vue的essential配置
        "eslint:recommended"    // 启用eslint推荐的规则
    ],
    parserOptions: {            // 解析器选项
        parser: "babel-eslint"  // 一个对Babel解析器的包装，使其能够与 ESLint 兼容。
    },
    plugins: [  // 插件
        'vue',  // eslint-plugin-vue插件
        'html'  // eslint-plugin-html插件
    ],
    rules: {    // 规则
        "no-multiple-empty-lines": [2, { "max": 3 }],   // 空行不得连续超过三行。
        "no-extra-boolean-cast": 2,  // 禁止不必要的布尔类型转换
        "no-extra-semi": 2,          // 禁止使用额外的分号，禁止情况如：";;"
    }
}
```

## 4. 规则的级别
1. "off" or 0 - 关闭规则
2. "warn" or 1 - 将规则视为一个警告（不会影响退出码）
3. "error" or 2 - 将规则视为一个错误 (退出码为1)

> 这三个错误级别可以允许你细粒度的控制 ESLint 是如何应用规则。（摘自官方文档）

## 扩展阅读
* eslint官方文档：https://eslint.bootcss.com/
* plugin-vue-eslint官方文档：https://eslint.vuejs.org/