---
title: 使用idea开发springboot项目生成文件的含义详解
date: 2020-07-01 23:35:30 
categories: es6
tags: [es6, js]
---
* .iml：idea的工程配置文件。包含当前project的一些配置信息，如模块开发的相关信息，比如java组件，maven组件，插件组件等，还可能会存储一些模块路径信息，依赖信息以及一些别的信息。
* mvnw：一个执行脚本，用于命令行环境。mvnw是一个maven wrapper script,它可以让你在没有安装maven或者maven版本不兼容的条件下运行maven的命令.
* mvnw.cmd：作用与mvnw相同，只是用于win环境。