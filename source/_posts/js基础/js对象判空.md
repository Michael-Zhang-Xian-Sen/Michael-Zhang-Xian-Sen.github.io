---
title:  js对象判空 #文章页面上的显示名称，可以任意修改，不会出现在URL中

date: 2020-08-01 11:35:30 #文章生成时间，一般不改，当然也可以任意修改

categories: 前端 #分类

tags: [前端]  #文章标签，可空，多标签请用格式，注意:后面有个空格

---

js判断一个对象为空的方法：
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