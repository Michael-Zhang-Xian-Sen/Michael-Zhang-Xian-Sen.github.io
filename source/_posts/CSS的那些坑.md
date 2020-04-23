---
title: CSS的那些坑 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-03-27 22:05:30 #文章生成时间，一般不改，当然也可以任意修改

categories: css #分类

tags: css #文章标签，可空，多标签请用格式，注意:后面有个空格

description: css的那些坑

---

css中的坑不少啊。

<!-- more -->

### font-size
1. webkit内核的浏览器中，该属性不支持小于`12px`的值。若要将字体设置的更小，可以使用css3中的`transform:scale()`。

