---
title: 包管理器npm #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-08-27 16:04:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [前端] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 包管理器npm
---

## 关于npx
* npx 想要解决的主要问题，就是调用项目内部安装的模块。
* npx还可以安装模块，且默认不是全局安装，而是安装到一个临时目录，用完即删。可以通过命令进行指定是否使用本地的模块而不是安装模块（--no-install）、强制安装并使用远程模块（--ignore-existing）
* `-p`参数，指定要安装的模块。
* `-c`参数，使得所有命令交由npx解释。
* npx还可以远程执行github源码。
* npm从5.2版本开始支持npx

### dependencies和devDependencies的区别
* `devDependencies`：开发环境使用。npm i -D 是 npm install --save-dev 的简写，是指安装模块并保存到 package.json 的 devDependencies。在开发环境，npm i ，安装所有devDependencies 和 dependencies里面的模块
* `dependencies`：生产环境使用。npm i -S 在生产环境，使用npm install --production安装 dependencies 部分的模块。

## 常用命令
删除一个依赖：`npm remove`

## 参考资料
* 阮一峰的npx使用教程：http://www.ruanyifeng.com/blog/2019/02/npx.html