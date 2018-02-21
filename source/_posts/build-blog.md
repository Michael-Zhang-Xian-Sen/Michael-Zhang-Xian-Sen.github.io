---
title: GitHub Page + Hexo 搭建博客（win10） #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2017-05-10 21:12:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 环境搭建 #分类
tags: [win10,博客] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 简要介绍了一下搭建博客的过程、遇到的问题及解决方案。
---

简要介绍一下搭建博客的过程、遇到的问题及解决方案。

<!-- more -->


## 搭建
  https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/ 这篇博客写的相当详细。
        但有些问题还是没有说清楚，也踩了不少坑。
	问题及解决方案如下：
        问题1：没说清楚如何创建GitHub pages。
        解决方案：参考官网介绍可以将其搭建出来，并不难。https://pages.github.com
        问题2：搭建完成GitHub pages以后，如何将主题放到GitHub pages中。
        解决方案：这时根据第一个博客所讲述，使用hexo deploy即可。

### 发表文章
1. 新建文章
hexo n 命令，会在项目\source_posts中生成my new post.md文件，用编辑器打开即可进行编写。
或者写好.md文件后，在\source_posts中新建md文件。
2. 推送文章
执行：
> hexo g #生成文章
> hexo d #部署，可与hexo g合并为hexo d -g
3. md文章标头格式
> title： 文章页面上的显示名称，可以任意修改，不会出现在URL中。
> date：文章生成时间。格式为xxxx-xx-xx xx:xx:xx
> categories：文章所属分类
> tags：文章标签。

### 更改主题配置
在hexo下的/themes/(你的主题名称)/_config.yml文件，对其进行更改。
最后通过hexo d -g提交更改。

### 常用命令：
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
hexo help # 查看帮助
hexo version #查看Hexo的版本

### 部分简写：
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
