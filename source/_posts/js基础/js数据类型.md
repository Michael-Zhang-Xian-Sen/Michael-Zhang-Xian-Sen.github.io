---
title:  js数据类型 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-05-04 18:13:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 前端 #分类

tags: [前端]  #文章标签，可空，多标签请用格式，注意:后面有个空格

---

#  数据类型
共八种，分为基础类型和引用类型。

## 基本类型（又称原始类型、简单类型。）
值本身无法被改变。尤其注意的是基本类型和内置对象是不一样的。比如Boolean的基本类型的值只有false和true，而Boolean对象只是用来承载Boolean基本类型的。
1. Boolean：值仅有`true`或`false`
    * 判断基本类型：`typeof variable === "boolean" `
2. Null
    * 判断基本类型：`variable === null`
3. Undefined
    * 判断基本类型方法1：`variable === undefined`
    * 判断基本类型方法2：`typeof a === "undefined"`
4. Number
    * 判断基本类型：`typeof variable === "number"`
    * 判断整数：`Number.isInteger()`。
    * 判断是否为NaN：`Number.isNan()`。
    * 判断是否为有穷数：`Number.isFinite()`。
    * 字符串转浮点数：`Number.parseFloat()`。
    * 字符串转整数：`Number.parseInt()`。和全局的`parseInt()`方法一致。
5. BigInt
    * 判断基本类型：`typeof variable === "bigint"`
    * 注意：不能用于Math对象中的方法。
6. String
    * 判断基本类型：`typeof variable === "String"`
7. Symbol
    * 判断基本类型：`typeof variable === "Symbol"`

## 引用类型Object
值本身可以被改变。

### 对象判空的方法
1. 使用JSON.stringify()。
2. 使用`for in`。
3. 使用Object.keys()。

### 1. 使用JSON.stringify()
```
function isObjEmpty(obj){
    return JSON.stringify(obj) === "{}"
}
```

### 2. 使用`for in`
```
function isObjEmpty(obj){
    for(let attr in obj){
        return false;
    }
    return true;
}
```

### 3. 使用Object.keys()
```
function isObjEmpty(obj){
    return Object.keys(obj).length === 0
}
```