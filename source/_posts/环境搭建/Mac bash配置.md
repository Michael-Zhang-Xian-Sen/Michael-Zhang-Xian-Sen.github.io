---
title: 打造舒适的Mac工作环境：CLI配置 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-07-02 21:47:50 #文章生成时间，一般不改，当然也可以任意修改
categories: mac #分类
tags: [mac, 工具] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: mac工具
---

mac环境下的CLI终端工具及bash配置推荐。

<!-- more -->

## 目录
1. CLI终端推荐：iTerm2
2. bash推荐：oh-my-zsh
3. bsah相关的命令

## 1. CLI终端推荐：iTerm2
1. 下载地址：https://www.iterm2.com/

### 实用小技巧
* 分屏：`command+d`。

## 2. bash推荐：oh-my-zsh
oh-my-zsh是一款傻瓜化的zsh配置工具。优点：省心，功能强大。
1. 安装oh-my-zsh：`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

### 2.1 插件
如何安装插件？
1. 在`oh-my-zsh安装位置/.oh-my-zsh/custom/plugins`文件夹下，使用插件名创建文件夹`mkdir 插件名`
2. 将下载好的插件放入该文件夹中。
3. 打开oh-my-zsh配置文件：`vim ~/.zshrc`
4. 在配置文件结束添加如下内容：`source $ZSH/custom/plugins/插件名文件夹/插件名`（vim中`shit+G`可以快速跳转到最后一行）
5. 更新配置`source ~/.zshrc`

#### 2.1.1 git plugin（自带插件）
这款插件默认开启。该插件为大量的git的命令设置了别名，

文档地址：https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git/

#### 2.1.2 extract plugin（自带插件）
一款功能强大的解压软件。仅通过一个命令`extract`即可解压大部分的压缩文件，包括rar、zip、tar等。

文档地址：https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/extract

#### 2.1.3 自动补全插件
这款插件可以在用户输入命令时，在光标下方提示可以使用的命令、文件等信息，再也不用为记不住linux命令名、不停地输入ls而苦恼。

插件下载地址：http://mimosa-pudica.net/src/incr-0.2.zsh

## 2. bash相关的命令
* 查看当前终端使用的bash：`echo $SHELL`
* 查看当前安装的所有bash：`cat /etc/shells`