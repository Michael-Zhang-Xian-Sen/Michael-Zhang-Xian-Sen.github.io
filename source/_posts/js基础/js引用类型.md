---
title: JavaScript中的引用类型总结 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-03-16 10:25:50 #文章生成时间，一般不改，当然也可以任意修改
categories: 前端 #分类
tags: [前端, js] #文章标签，可空，多标签请用格式，注意:后面有个空格
---

常用的引用类型：
1. Object类型。
2. Array类型。
3. Date类型。
4. RegExp类型。
5. Function类型。
6. 基本包装类型（Boolean、Number、String）。
7.  Global对象。

其他：
1. instanceof运算符

## 1. Objet类型
### 创建实例的方法
1. 使用Object构造函数：`var a = new Object()`
2. 对象字面量方法：`var person = {name:'Michael Zhang'}`

### 访问对象属性
1. 使用点表示法：`alert(person.name)`
2. 使用方括号语法：`alert(person['name'])`

## 2. Array类型
### 创建数组的方法
1. 使用Array构造函数：`var nums = new Array(arrayLength)`，`var nums = Array(1,2,3,4,5)`
2. 使用数组字面量的方法：`var colors = ["red","green"]`

### 判断Array的方式
1. `Array.isArray(arr)`最推荐！
2. `instanceof` 运算符。如果有多个全局作用域可能会失效。

### 数组的排序方法
1. `sort()`，默认调用每个数组项的`toString()`方法，默认比较数组项的UTF-16代码单元值序列，按升序排列。接收一个比较函数作为参数，用于对数组项的两两比较。比较函数接收两个参数，分别代表当前比较的两个元素。如果比较函数的参数分别为a,b,返回值小于0，则a在b前。若等于0，二者相对位置不变，若返回值大于0，则b在a前。

### 数组与数据结构相关的方法
1. 栈方法。
    * 入栈`push()`，如`colors.push("brown","yellow")`，返回值为入栈后的数组长度。
    * 出栈`pop()`，返回值为数组的最后一项。
2. 队列方法。
    * 入队`push()`，返回值为入队列后的数组长度。
    * 出队`shift()`，返回值为数组的第一项。`unshift`与之相反，是添加任意项元素至数组头部，返回值为插入后的数组长度。

### 奇淫技巧
1. length属性——数组的长度，是可以手动设置的。可以用来给数组末尾添加项：`var colors = ["red"];colors[colors.length] = "green";`

## 3. Date类型
Date()对象表示某个时刻。

### 构造器

#### 1. 无参数
* 语法：`new Date()`
* 返回一个Date对象，该对象表示实例化Date()时的时刻。

#### 2. Unix时间戳
* 语法`new Date(timestamp)`
* 示例：`new Date((new Date()).getTime()-1000*60*60*24)`，获取表示昨天当前时刻的date对象。
* 什么是Unix时间戳？一个 Unix 时间戳（Unix Time Stamp），它是一个整数值，表示自1970年1月1日00:00:00 UTC（the Unix epoch）以来的毫秒数，忽略了闰秒。

#### 3. 字符串（不建议）
* 形式：`new Date(dateString)`
* 示例：`new Date('2020-03-16')`，获取表示当前日期、时间的小时数等于当前时区（博主是东八区），分、秒皆为0的时刻。该示例返回的时间日期为：`"Sun Feb 02 2020 08:00:00 GMT+0800 (中国标准时间)"`
* 字符串的要求：该dateString需要能被`date.parse()`识别。

#### 4. 提供时间与日期的每一项成员
* 语法：`new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]])`
* 示例1：`new Date(2020,2)`，返回的date对象时间和日期为：`"Sun Mar 01 2020 00:00:00 GMT+0800 (中国标准时间)"`
    * **注意1**：月数的索引从0开始，与字符串形式的索引不一致。
    * **注意2**：时间默认从当前时区的0点开始。
* 示例2：执行语句`new Date(2020,3,16)`返回的date对象的时间和日期为：`"Thu Apr 16 2020 00:00:00 GMT+0800 (中国标准时间)"`

### 静态方法
* `Date.now()`，返回自UTC时间1970-1-1 00:00:00至今所经过的毫秒数。
    * 执行语句：`Date.now()`。执行结果：`1584452901581`
* `Date.parse()`，解析一个表示日期的字符串，返回自UTC时间1970-1-1 00:00:00至今所经过的毫秒数。
    * 执行语句：`Date.parse('2020-03-17')`，执行结果：`1584403200000`
    * 执行语句：`(new Date(Date.parse("2020-03-17"))).toString()`，执行结果：`"Tue Mar 17 2020 08:00:00 GMT+0800 (中国标准时间)"`
    * **不建议使用，因浏览器的实现有差异**
* `Date.UTC()`接受参数同构造器4“提供时间与日期的每一项成员”，返回自UTC时间1970-1-1 00:00:00至今所经过的毫秒数。
    * 执行语句：`Date.parse(2020,02,17)`，执行结果：`1584403200000`。注意此处的月份索引从0开始。
    * 执行语句：`(new Date(Date.parse(2020,02,17))).toString()`，执行结果：`"Tue Mar 17 2020 08:00:00 GMT+0800 (中国标准时间)"`

#### `Date.prototype.toDateString()`
* 作用：以人类易读（human-readable）的形式返回该日期对象日期部分的字符串。
* 执行语句：`(new Date()).toDateString()`。执行结果：`"Tue Mar 17 2020"`

####  `Date.prototype.toISOString()`
* 作用：把一个日期转换为符合 ISO 8601 扩展格式的字符串。》》》》》这里最好也做一个跳链。
* ISO 8601：由国际标准化组织（ISO）提出的编号为8601的标准。该标准的全部内容需要收费阅读，但作为开发我们只需要看万维网联盟（W3C）根据ISO内容制定的标准即可[Date and Time Formats](https://www.w3.org/TR/NOTE-datetime)。比较重要的内容如下：
    * 完整的日期与时间格式，包含年、月、日、时、分、秒、秒的小数位和时区指示符：`YYYY-MM-DDThh:mm:ssTZD`，例如：`1997-07-16T19:20:30+01:00`。
    * 使用UTC时区，则显示时区指示符`Z`
    * 使用本地时区，则显示时区指示符`+hh:mm`或`-hh:mm`。`+hh:mm`含义为"本地时区相较于UTC时区，要快hh个小时、mm分钟；`-hh:mm`含义为"本地时区相较于UTC时区要慢hh个小时、mm分钟"。
* 执行语句：`(new Date()).toISOString()`。执行结果：`"2020-03-17T14:22:12.947Z"`（注：此时本地时间为2020-03-17,22:22:12）
 
#### `Date.prototype.toJSON()`
* 作用：使用 toISOString() 返回一个表示该日期的字符串。为了在 JSON.stringify() 方法中使用。
* 调用`toJSON()`作用和调用`toISOString()`一样一样的。为什么呢？请看后文》》》》这里给个跳链。
* 执行语句：`(new Date()).toJSON()`。执行结果：`"2020-03-17T15:30:30.979Z"`。

#### `Date.prototype.toGMTString()`
* 作用：返回一个基于 GMT (UT) 时区的字符串来表示该日期。**mdn官方不建议使用，请使用 toUTCString() 方法代替。**
* 执行语句：`(new Date()).toGMTString()`。执行结果：`"Tue, 17 Mar 2020 15:37:56 GMT"`

#### `Date.prototype.toLocaleDateString()`
* 作用：返回一个表示该日期对象日期部分的字符串，该字符串格式与系统设置的地区关联（locality sensitive）。
* 执行语句：`(new Date()).toLocaleDateString()`。执行结果：`"2020/3/18"`

#### `Date.prototype.toLocaleString()`
* 作用：返回一个表示该日期对象的字符串，该字符串与系统设置的地区关联（locality sensitive）。覆盖了 Object.prototype.toLocaleString() 方法。
* 执行语句：`(new Date()).toLocaleString()`。执行结果：`"2020/3/18 下午11:45:03"`

#### `Date.prototype.toLocaleTimeString()`
* 作用：返回一个表示该日期对象时间部分的字符串，该字符串格式与系统设置的地区关联（locality sensitive）。
* 执行语句：`(new Date()).toLocaleTimeString()`。执行结果：`"下午11:47:12"`

#### `Date.prototype.toString()`
* 作用：返回一个表示该日期对象的字符串。覆盖了Object.prototype.toString() 方法。
* 执行语句：`(new Date()).toString()`。执行结果：`"Wed Mar 18 2020 23:48:31 GMT+0800 (中国标准时间)"`

#### `Date.prototype.toTimeString()`
* 作用：以人类易读格式返回日期对象时间部分的字符串。
* 执行语句：`(new Date()).toTimeString()`。执行结果：`"23:50:22 GMT+0800 (中国标准时间)"`

#### `Date.prototype.toUTCString()`
* 作用：把一个日期对象转换为一个以UTC时区计时的字符串。
* 执行语句：`(new Date()).toUTCString()`。执行结果：`"Wed, 18 Mar 2020 15:51:49 GMT"`

#### `Date.prototype.valueOf`
* 作用：返回一个日期对象的原始值。覆盖了 Object.prototype.valueOf() 方法。
* 执行语句：`(new Date()).valueOf()`。执行结果：`1584546764856`





## 4. RegExp类型
### 创建实例的方法
1. 使用RegExp构造函数：`var pattern1 = new RegExp("[bc]at","i")`，`var pattern1 = new RegExp(/ab+c/,"i")`
2. 使用字面量形式：`var pattern2 = /[bc]at/i`

### 匹配方法
1. `exec()`，如：`var arr =  regex1.exec(str1)`。可以获取匹配的字符位于原字符串的索引、分组捕获、原始字符串等信息，同时RegExp对象也会更新下一次匹配开始的位置。更多信息详见：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec
2. `test()`，用来查看正则表达式与指定的字符串是否匹配，如：`let result = /^hello/.test(str)`

## 5. Function类型。
### 创建Function的方法
1. 函数声明。如：`function sum(num1,num2){return num1+num2}`
2. 函数表达式。如：`var sum = function(num1,num2){return num1+num2}`
注意，函数声明存在提升。

### 函数内部的两个特殊变量
1. `arguments`，保存函数参数。`arguments`有一个特殊属性`callee`，可用于与函数名解耦。
2. `this`。

### 函数的属性和方法
1. `length`属性，表示函数希望接收的命名参数的个数。
2. `proptotype`属性。
3. `apply`、`call`和`bind`，更改函数的作用域。`functionName.apply(作用域，arguments数组)`,`functionName.call(作用域，数组1，数组2)`，`var newFunction = functionName.bind(作用域)`

## 6. 基本包装类型
基本包装类型是对应基本类型值的特殊的引用类型
读取String、Boolean或者Number基本类型值时，后台可能会执行如下操作：
1. 创建String类型的一个实例。
2. 在实例上调用指定的方法。
3. 销毁这个实例。
注意：后台自动生成的基本包装类型对象，和通过`new`操作符生成的基本包装类型对象，生命周期不同。后台自动生成的对象，只存在于一行代码执行的瞬间。而`new`操作符生成的基本包装类型的对象，在执行流离开当前作用域之前都一直保存在内存中。

### Boolean包装类型
注意，在布尔表达式中使用Boolean对象，会将其转化为true。
建议永远不要使用Boolean对象。

### Number包装类型
1. `toString()`方法可以传递一个表示基数的参数。
2. `toFixed()`方法可以传递指定小数位数的数字，将按指定的小数位返回数值的字符串表示。

### String包装类型
1. `concat()`，将一个或多个字符串拼接起来，返回拼接得到的新字符串。

## 7. Global对象
Math对象、Window对象等

## 其他
## 1. instance运算符
用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。