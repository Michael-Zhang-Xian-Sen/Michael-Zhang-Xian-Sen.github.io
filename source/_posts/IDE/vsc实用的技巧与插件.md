---
title: vsc的使用技巧及实用插件
date: 2020-09-03 18:25:30 
categories: vsc
tags: [vsc]
---

<!-- TOC -->

1. [实用插件](#实用插件)
    1. [EditorConfig](#editorconfig)
    2. [Markdown TOC](#markdown-toc)

<!-- /TOC -->

# 实用插件
## EditorConfig
用于统一项目的编码风格，如采用何种方式缩进。该插件的优先级高于编辑器默认的代码格式化规则。

```js
/* .editorconfig文件 */
[*.{js,jsx,ts,tsx,vue}]
root = true
# 缩进风格：空格
indent_style = space
# 缩进大小：4
indent_size = 4
# 是否删除行尾的空格：true
trim_trailing_whitespace = true
# 是否在文件的最后插入一个空行：true
insert_final_newline = true
```

## Markdown TOC
用于生成markdown的目录(Table of Content)。本文的目录便是使用该插件生成。

该插件在vsc的插件市场中下载量极高，但是原作者不维护了，当前的可下载版本存在一些问题，大致如下：
1. 生成的目录没有换行，且出现大量`auto`。错误原因是当前vsc配置的EOL为`LF(\n)`，修改为`CRLF(\r\n)`即可。
2. 生成的标题从0开始计数。原因是标题必须从`#`开始。个人感觉这条规定很不合理，有些场景根本不适合用那么大的标题，但是若想正常使用插件便必须按照他的规定来。

默默吐槽一下。由于原作者已不再维护该插件，于是一些用户fork源码后自行维护，导致vsc插件市场上有三四个类似的插件，但是...仍然都有bug...直观感受是类似拆了东墙补西墙...而且很少有人讨论修改后冒出来的奇葩问题...算了就用原作者的吧。