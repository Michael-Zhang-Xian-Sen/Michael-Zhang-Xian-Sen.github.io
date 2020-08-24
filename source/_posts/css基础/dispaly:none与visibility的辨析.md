---
title: display:none与visibility:hidden的辨析 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-08-17 11:09:30 #文章生成时间，一般不改，当然也可以任意修改
categories: css #分类
tags: css #文章标签，可空，多标签请用格式，注意:后面有个空格
description: css的那些坑
---

### 相同点
1. 都能把网页上某个元素隐藏起来。

### 不同点
1. display:none隐藏的元素不占据物理空间（设置该属性的元素会产生回流，不会加入到render tree），visibility:hidden隐藏的元素占据物理空间（不会产生回流，会加入到render tree）。
2. display:none会跳过ol的计数器，而visibility不会跳过。
3. css3的transition支持visibility属性，不支持display属性。
4. visibility:hidden具有继承性，子元素也会继承visibility:hidden属性。display:none没有继承性。
