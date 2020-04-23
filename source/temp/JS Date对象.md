---
title: JavaScript中的Date对象及其常用方法总结 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-03-16 10:25:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [前端, date] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: JavaScript中的Date对象及其常用方法总结
---

本文主要总结了理解和使用Date对象的几个关键的常识性问题，然后根据mdn整理并运行了Date对象的所有方法。

<!-- more -->

Date()对象表示某个时刻。

## 关于Date对象，比较关键的几个问题。

### UTC和GMT的区别？

### 


## 构造器

### 1. 无参数
* 语法：`new Date()`
* 返回一个Date对象，该对象表示实例化Date()时的时刻。

### 2. Unix时间戳
* 语法`new Date(timestamp)`
* 示例：`new Date((new Date()).getTime()-1000*60*60*24)`，获取表示昨天当前时刻的date对象。
* 什么是Unix时间戳？一个 Unix 时间戳（Unix Time Stamp），它是一个整数值，表示自1970年1月1日00:00:00 UTC（the Unix epoch）以来的毫秒数，忽略了闰秒。

### 3. 字符串（不建议）
* 形式：`new Date(dateString)`
* 示例：`new Date('2020-03-16')`，获取表示当前日期、时间的小时数等于当前时区（博主是东八区），分、秒皆为0的时刻。该示例返回的时间日期为：`"Sun Feb 02 2020 08:00:00 GMT+0800 (中国标准时间)"`
* 字符串的要求：该dateString需要能被`date.parse()`识别。

### 4. 提供时间与日期的每一项成员
* 语法：`new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]])`
* 示例1：`new Date(2020,2)`，返回的date对象时间和日期为：`"Sun Mar 01 2020 00:00:00 GMT+0800 (中国标准时间)"`
    * **注意1**：月数的索引从0开始，与字符串形式的索引不一致。
    * **注意2**：时间默认从当前时区的0点开始。
* 示例2：执行语句`new Date(2020,3,16)`返回的date对象的时间和日期为：`"Thu Apr 16 2020 00:00:00 GMT+0800 (中国标准时间)"`

## 静态方法
* `Date.now()`，返回自UTC时间1970-1-1 00:00:00至今所经过的毫秒数。
    * 执行语句：`Date.now()`。执行结果：`1584452901581`
* `Date.parse()`，解析一个表示日期的字符串，返回自UTC时间1970-1-1 00:00:00至今所经过的毫秒数。
    * 执行语句：`Date.parse('2020-03-17')`，执行结果：`1584403200000`
    * 执行语句：`(new Date(Date.parse("2020-03-17"))).toString()`，执行结果：`"Tue Mar 17 2020 08:00:00 GMT+0800 (中国标准时间)"`
    * **不建议使用，因浏览器的实现有差异**
* `Date.UTC()`接受参数同构造器4“提供时间与日期的每一项成员”，返回自UTC时间1970-1-1 00:00:00至今所经过的毫秒数。
    * 执行语句：`Date.parse(2020,02,17)`，执行结果：`1584403200000`。注意此处的月份索引从0开始。
    * 执行语句：`(new Date(Date.parse(2020,02,17))).toString()`，执行结果：`"Tue Mar 17 2020 08:00:00 GMT+0800 (中国标准时间)"`

## Getter方法

## Setter方法

## 转换型Getter方法

### `Date.prototype.toDateString()`
* 作用：以人类易读（human-readable）的形式返回该日期对象日期部分的字符串。
* 执行语句：`(new Date()).toDateString()`。执行结果：`"Tue Mar 17 2020"`

###  `Date.prototype.toISOString()`
* 作用：把一个日期转换为符合 ISO 8601 扩展格式的字符串。》》》》》这里最好也做一个跳链。
* ISO 8601：由国际标准化组织（ISO）提出的编号为8601的标准。该标准的全部内容需要收费阅读，但作为开发我们只需要看万维网联盟（W3C）根据ISO内容制定的标准即可[Date and Time Formats](https://www.w3.org/TR/NOTE-datetime)。比较重要的内容如下：
    * 完整的日期与时间格式，包含年、月、日、时、分、秒、秒的小数位和时区指示符：`YYYY-MM-DDThh:mm:ssTZD`，例如：`1997-07-16T19:20:30+01:00`。
    * 使用UTC时区，则显示时区指示符`Z`
    * 使用本地时区，则显示时区指示符`+hh:mm`或`-hh:mm`。`+hh:mm`含义为"本地时区相较于UTC时区，要快hh个小时、mm分钟；`-hh:mm`含义为"本地时区相较于UTC时区要慢hh个小时、mm分钟"。
* 执行语句：`(new Date()).toISOString()`。执行结果：`"2020-03-17T14:22:12.947Z"`（注：此时本地时间为2020-03-17,22:22:12）
 
### `Date.prototype.toJSON()`
* 作用：使用 toISOString() 返回一个表示该日期的字符串。为了在 JSON.stringify() 方法中使用。
* 调用`toJSON()`作用和调用`toISOString()`一样一样的。为什么呢？请看后文》》》》这里给个跳链。
* 执行语句：`(new Date()).toJSON()`。执行结果：`"2020-03-17T15:30:30.979Z"`。

### `Date.prototype.toGMTString()`
* 作用：返回一个基于 GMT (UT) 时区的字符串来表示该日期。**mdn官方不建议使用，请使用 toUTCString() 方法代替。**
* 执行语句：`(new Date()).toGMTString()`。执行结果：`"Tue, 17 Mar 2020 15:37:56 GMT"`

### `Date.prototype.toLocaleDateString()`
* 作用：返回一个表示该日期对象日期部分的字符串，该字符串格式与系统设置的地区关联（locality sensitive）。
* 执行语句：`(new Date()).toLocaleDateString()`。执行结果：`"2020/3/18"`

### `Date.prototype.toLocaleString()`
* 作用：返回一个表示该日期对象的字符串，该字符串与系统设置的地区关联（locality sensitive）。覆盖了 Object.prototype.toLocaleString() 方法。
* 执行语句：`(new Date()).toLocaleString()`。执行结果：`"2020/3/18 下午11:45:03"`

### `Date.prototype.toLocaleTimeString()`
* 作用：返回一个表示该日期对象时间部分的字符串，该字符串格式与系统设置的地区关联（locality sensitive）。
* 执行语句：`(new Date()).toLocaleTimeString()`。执行结果：`"下午11:47:12"`

### `Date.prototype.toString()`
* 作用：返回一个表示该日期对象的字符串。覆盖了Object.prototype.toString() 方法。
* 执行语句：`(new Date()).toString()`。执行结果：`"Wed Mar 18 2020 23:48:31 GMT+0800 (中国标准时间)"`

### `Date.prototype.toTimeString()`
* 作用：以人类易读格式返回日期对象时间部分的字符串。
* 执行语句：`(new Date()).toTimeString()`。执行结果：`"23:50:22 GMT+0800 (中国标准时间)"`

### `Date.prototype.toUTCString()`
* 作用：把一个日期对象转换为一个以UTC时区计时的字符串。
* 执行语句：`(new Date()).toUTCString()`。执行结果：`"Wed, 18 Mar 2020 15:51:49 GMT"`

### `Date.prototype.valueOf`
* 作用：返回一个日期对象的原始值。覆盖了 Object.prototype.valueOf() 方法。
* 执行语句：`(new Date()).valueOf()`。执行结果：`1584546764856`

