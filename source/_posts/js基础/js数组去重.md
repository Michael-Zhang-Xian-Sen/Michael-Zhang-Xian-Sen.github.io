---
title: js数组去重
date: 2020-09-24 22:50:30 #文章生成时间，一般不改，当然也可以任意修改
categories: js #分类
tags: [js] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 你不知道的JavaScript的读书笔记
---
根据是否改变原数组可分为两类，然后根据是否使用ES6特性，再进行细分。

<!-- TOC -->

1. [不改变原数组](#不改变原数组)
    1. [利用ES6的Set](#利用es6的set)
    2. [利用indexOf+额外数组](#利用indexof额外数组)
    3. [利用filter+indexOf](#利用filterindexof)
2. [改变原数组](#改变原数组)
    1. [利用splice方法](#利用splice方法)
3. [其他注意内容](#其他注意内容)

<!-- /TOC -->

# 不改变原数组
## 利用ES6的Set
```js
function unique(arr){
    return Array.from(new Set(arr));
} 
```
注意：无法去掉`{}`空对象。

## 利用indexOf+额外数组
借助额外的数组array。遍历原数组，如果该元素在array中不存在，则加入至array，否则跳过。
```js
function unique(arr) {
    var array = [],
        len = arr.length;
    for(var i=0;i<len;i++){
        if(array.indexOf(arr[i]) === -1) array.push(arr[i]);
    }
    return array;
}
```

## 利用filter+indexOf
利用filter方法遍历数组，使用indexOf判断元素是否重复，将重复的元素剔除。
```js
function unique(arr){
    return arr.filter((cur,idx,array)=>array.indexOf(cur) === idx)
}
```


# 改变原数组
## 利用splice方法
遍历数组，使用indexOf判断元素是否重复，如果重复则利用splice从原数组中剔除该元素。
```js
function unique(arr){
    for(var i=0;i<arr.length;i++){
        if(arr.indexOf(arr[i]) !== i){
            arr.splice(i--,1);
        }
    }
    return arr;
}
```

# 其他注意内容
需要判断接收的参数是否为数组，若不为数组则提示"Type Error"并返回一个空数组。